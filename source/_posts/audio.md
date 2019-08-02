---
title: 有关audio的一些事
layout: post
tag: javascript
---
### 前言
之前有做过一些跟audio相关的工作，大多数是h5页面中引入bgm，音效，在这里总结一下之前遇见的问题和解决的办法以及一些优化用户体验的实践。

### audio标签
以前的项目中使用方法
```html
  <audio src="http://static.guobaoyoo.com/mp3/naonao/bgm.mp3" autoplay="autoplay" controls="controls" loop="loop"></audio>
```

<!--more--> 

### 之前项目需求
第一种：页面加载后，自动播放bgm
第二种：为点击添加音效

### 部分代码
```javascript
  <audio src="http://static.guobaoyoo.com/mp3/naonao/bgm.mp3" id="myAudio" autoplay="autoplay" controls="controls" loop="loop"></audio>
  const audio = document.getElementById('myAudio')
  audio.currentTime = 0
  audio.play()
```

### 遇见问题以及解决办法
>Note: Sites that automatically play audio (or videos with an audio track) can be an unpleasant experience for users, so should be avoided when possible. If you must offer autoplay functionality, you should make it opt-in (requiring a user to specifically enable it). However, this can be useful when creating media elements whose source will be set at a later time, under user control.  

mdn上给出的note，大概意思就是尽量避免使用autoplay，如果不得不用的话，应该让用户做操作，所以产品产出调整，页面加载后触发用户一个行为后让audio加载，完事再播放就ok了，`但是需要让用户做出操作`。
```javascript
  <audio src="http://static.guobaoyoo.com/mp3/naonao/bgm.mp3" id="myAudio" autoplay="autoplay" controls="controls" loop="loop"></audio>
  const audio = document.getElementById('myAudio')
  // 在这里用户的一个行为触发，比如是个页面click事件
  document.body.addEventListener('click',function () {
    audio.load()
  }, false)
  audio.currentTime = 0
  audio.play()
```

### Web Audio API AudioContext
>Web Audio API 提供了在Web上控制音频的一个非常有效通用的系统，允许开发者来自选音频源，对音频添加特效，使音频可视化，添加空间效果 （如平移），等等。

api接口比较多，也没有一条一条具体去看，针对我想要的，引入第三方资源，然后控制播放，停止播放等等。

上代码
```javascript
  let AudioContext = window.AudioContext || window.webkitAudioContext
  let audioCtx = AudioContext ? new AudioContext() : ''

  let bgmBuffer = {
    getBuffer (link) {
      return new Promise((resolve, reject) => {
        if (audioCtx) {
          let request = new XMLHttpRequest()
          request.open('GET', link, true)
          request.responseType = 'arraybuffer'
          request.onload = function () {
            audioCtx.decodeAudioData(request.response, function (buffer) {
              resolve(buffer)
            }, function (e) {
              console.log('reject')
              reject(e)
            })
          }
          request.send()
        } else {
          reject(new Error('not support AudioContext'))
        }
      })
    },
    createSound (buffer) {
      if (audioCtx.state === 'suspended') {
        audioCtx.suspend().then(() => {
          console.log('重启audioCtx')
        })
      }
      let source = audioCtx.createBufferSource()
      source.buffer = buffer
      source.connect(audioCtx.destination)
      return source
    }
  }
  export default bgmBuffer

  import Audio from '@/utils/audio'
  data () {
    return {
      transitionName: '',
      bgmBuffer: null,
      bgmSound: null,
      audioPlaying: false
    }
  },
  computed: {
    playing () {
      return this.$store.state.audioPlaying
    }
  },
  methods: {
    getBgmSound () {
      Audio.getBuffer('http://static.guobaoyoo.com/mp3/naonao/bgm.mp3').then(res => {
        this.bgmBuffer = res
        this.bgmPlay(res)
      }).catch(err => {
        console.log(err)
      })
    },
    bgmPlay (buffer) {
      this.bgmSound = Audio.createSound(buffer)
      this.bgmSound.loop = true
      console.log(this.bgmSound)
      if (this.bgmSound !== null) {
        this.bgmSound.start()
        this.$store.commit('BGM_PLAY', true)
      }
    },
    bgmStop () {
      if (this.bgmSound !== null) {
        this.bgmSound.stop()
        this.$store.commit('BGM_PLAY', false)
      }
    },
    bgmControl () {
      console.log(this.playing)
      if (!this.playing) {
        this.bgmPlay(this.bgmBuffer)
      } else {
        this.bgmStop()
      }
    },
    created () {
      this.getBgmSound()
    }
  }
```
### 探索遗留问题
- 放到手机中还是不能自动播放
- 不能暂停

### 追加
移动端微信中播放视频
```javascript
let video = document.getElementById('video')
if (window.WeixinJSBridge) {
  window.WeixinJSBridge.invoke('getNetworkType', {}, function (e) {
    video.play()
  }, false)
} else {
  document.addEventListener('WeixinJSBridgeReady', function () {
    window.WeixinJSBridge.invoke('getNetworkType', {}, function (e) {
      video.play()
    })
  }, false)
}
video.play()
```