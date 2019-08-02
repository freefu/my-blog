---
title: vue.config.js 配置项
layout: post
tag: vue
---

```javascript
const path = require('path')

const assetsDir = 'static'
const resolve = dir => path.join(__dirname, dir)
// posix兼容方式处理路径
const posixJoin = _path => path.posix.join(assetsDir, _path)

const lastVersion = new Date().getTime()
const isProd = process.env.NODE_ENV === 'production'

// cdn开关
const OPENCDN = true
const webpackHtmlOptions = {
  // dns预加载，优化接口请求
  dnsPrefetch: ['https://fff.exmaple.com'],
  externals: {
    vue: 'Vue',
    'vue-router': 'VueRouter',
    vuex: 'vuex'
    vant: 'vant',
    fastClick: 'FastClick'
  },
  cdn: {
    // 生产环境
    build: {
      css: ['https://cdn.jsdelivr.net/npm/vant@1.5/lib/index.css'],
      js: [
        'https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.min.js',
        'https://cdn.jsdelivr.net/npm/vue-router@3.0.1/dist/vue-router.min.js',
        'https://unpkg.com/vuex@3.0.1/dist/vuex.min.js',
        'https://cdn.jsdelivr.net/npm/vant@1.5/lib/vant.min.js',
        'https://cdn.jsdelivr.net/npm/fastclick@1.0.6/lib/fastclick.min.js'
      ]
    }
  }
}

module.exports = {
  publicPath: process.env.NODE_ENV === 'production' ? '/' : '/',
  outputDir: 'dist',
  lintOnSave: true,
  transpileDependencies: [],
  productionSourceMap: false,
  chainWebpack: config => {
    /**
     * 删除懒加载模块的 prefetch preload，降低带宽压力
     */
    config.plugins.delete('prefetch').delete('preload')
    config.resolve.alias
      .set('vue$', 'vue/dist/vue.esm.js')
      .set('common', resolve('src/common'))
      .set('@', resolve('src'))
    // 清除警告
    config.performance.set('hints', false)
    // 将版本号写入环境变量
    config.plugin('define').tap(args => {
      args[0]['app_build_version'] = lastVersion
      return args
    })
    config.when(isProd, config =>
      // 生产环境js增加版本号
      config.output
        .set('filename', posixJoin(`js/${lastVersion}-[name].[chunkhash].js`))
        .set(
          'chunkFilename',
          posixJoin(`js/${lastVersion}-[id].[chunkhash].js`)
        )
    )
    /**
     * 添加CDN参数到htmlWebpackPlugin配置中， 修改 public/index.html
     */
    config.plugin('html').tap(args => {
      // 生产环境将cdn写入webpackHtmlOptions，在public/index.html应用
      if (isProd && OPENCDN) {
        args[0].cdn = webpackHtmlOptions.cdn.build
      }
      // dns预加载
      args[0].dnsPrefetch = webpackHtmlOptions.dnsPrefetch
      return args
    })
  },
  configureWebpack: config => {
    config.resolve.extensions = ['.js', '.vue', '.json']
    if (isProd) {
      // 开启cdn状态：externals不进入webpack打包
      if (OPENCDN) {
        config.externals = webpackHtmlOptions.externals
      }
    }
  },
  css: {
    extract: !isProd
      ? false
      : {
        filename: posixJoin(`css/${lastVersion}-[name].[contenthash:8].css`),
        chunkFilename: posixJoin(
          `css/${lastVersion}-[name].[contenthash:8].css`
        )
      },
    sourceMap: false,
    loaderOptions: {
      css: {},
      postcss: {
        plugins: [
          require('autoprefixer')({
            browsers: [
              'last 10 Chrome versions',
              'last 5 Firefox versions',
              'Safari >= 6',
              'ie> 8'
            ]
          }),
          require('postcss-px2rem')({
            remUnit: 37.5
          })
        ]
      }
    }
    // modules: false
  },
  parallel: require('os').cpus().length > 1,
  pwa: {},
  devServer: {
    open: true, // 配置自启浏览器
    host: '0.0.0.0',
    port: 8088,
    https: false,
    hot: true,
    hotOnly: true,
    disableHostCheck: true
  },
  pluginOptions: {},
  runtimeCompiler: true
}
```