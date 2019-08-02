---
title: 理解vue生命周期
tag: vue
---
生命周期图就不放了，官网都有的，链接地址：[vue生命周期图](https://vuejs.org/v2/guide/instance.html#Lifecycle-Diagram)
大概分为几个步骤，其实看英文都能看的出来，都是对应的，一个`before*`，一个`*ed`，<u>理解英文对应的意思能更好的理解生命周期的含义</u>
- **beforeCreate** : **创建之前**，刚刚new Vue()之后，数据还没有挂载呢，只有一个空壳
- **created** : **创建完成之后**，这时候能使用并更改数据了，在这里更改数据不会触发 updated 函数
- **beforeMount** : **安装之前**，虚拟dom已经完成，马上就要渲染，这是最后一次能修改数据了，不会触发 update 函数
- **mounted** : **安装完成之后**，这时候真实dom已经渲染完事了，事件也已经挂载好了，可以进行一些真实的dom操作
- **beforeUpdate** : **更新（重新渲染）之前**，重新渲染之前触发，然后重新构建虚拟dom跟之前的虚拟dom进行 diff 算法比较后重新渲染
- **updated** : **更新完成（重新渲染完成）之后**，数据已经更改完成，dom也已经重新渲染完成
- **beforeDestroy** : **销毁之前**
- **destroy** : **销毁之后**

<!--more-->

具体的表现还有理解看代码吧。
```html
<!DOCTYPE html>
<html>

<head>
  <meta charset="utf-8" />
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <title>vue lifeCycle</title>
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@3.3.7/dist/css/bootstrap.min.css" integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u"
  crossorigin="anonymous">
  <script src="https://cdn.jsdelivr.net/npm/vue@2.5.17/dist/vue.js"></script>
</head>

<body>
  <div id="myApp">
    <section class="container">
      <my-temp></my-temp>
    </section>
  </div>
  <template id="myTemp">
    <div>
      <p></p>
      <p id="myP">my-temp 组件</p>
      <p><input class="form-control" type="text" v-model="myData"></p>
      <p>myData: {{ myData }}</p>
      <button class="btn btn-success" @click="changeData">changeData</button>
      <button class="btn btn-warning" @click="destroyEvent">destroy</button>
    </div>
  </template>
  <script>
    Vue.component('myTemp', {
      template: '#myTemp',
      data: function () {
        return {
          myData: 'hello world -- init'
        }
      },
      methods: {
        changeData () {
          this.myData = 'hello world -- changed'
        },
        destroyEvent () {
          this.$destroy()
        }
      },
      beforeCreate () {
        console.log('beforeCreate: 刚刚new Vue()之后，数据还没有挂载呢，只有一个空壳')
        console.log(this.myData)
        // undefined
        console.log(document.getElementById('myP'))
        // null
      },
      created () {
        console.log('created: 这时候能使用并更改数据了，在这里更改数据不会触发 updated 函数')
        this.myData = 'hello world -- created'
        console.log(this.myData)
        // hello world -- created
        console.log('在这里可以在渲染前倒数第二次更改数据的机会，不会触发其他的钩子函数，一般可以在这里做初始数据的获取')
        console.log('接下来开始找实例或者组件对应的模板，编译模板为虚拟dom放入到render函数中准备渲染')
      },
      beforeMount () {
        console.log('beforeMount: 虚拟dom已经完成，马上就要渲染，这是最后一次能修改数据了，不会触发 update 函数')
        this.myData = 'hello world -- beforeMount'
        console.log(this.myData)
        // hello world -- beforeMount
        console.log(document.getElementById('myP'))
        // null
        console.log('接下来开始render，渲染真实dom')
      },
      mounted () {
        console.log('mounted: 这时候真实dom已经渲染完事了，事件也已经挂载好了，可以进行一些真实的dom操作')
        console.log(document.getElementById('myP'))
        // <p id="myP">A组件</p>
        console.log('当数据改变，进入到 beforeUpdate ，当调用销毁，进入到 beforeDestory')
      },
      beforeUpdate () {
        console.log('beforeUpdate: 重新渲染之前触发，然后重新构建虚拟dom跟之前的虚拟dom进行 diff 算法比较后重新渲染')
        console.log('!!!! 不能在此处更改数据，会陷入死循环')
      },
      updated () {
        console.log('updated: 数据已经更改完成，dom也已经重新渲染完成')
        console.log('!!!! 不能在此处更改数据，会陷入死循环')
      },
      beforeDestroy () {
        console.log('beforeDestroy: 销毁前执行')
        console.log('拆除监听，子组件，事件绑定')
      },
      destroyed () {
        console.log('destroyed: 销毁之后触发')
      }
    })

    new Vue({}).$mount('#myApp')
  </script>
</body>

</html>
```
