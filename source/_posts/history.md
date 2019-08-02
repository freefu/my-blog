---
title: window.history
layout: post
tag: javascript
---

在工作中遇见一种需求，在用户点击回退按钮时为了挽留用户，会监听这个返回事件从而进行一些操作，就会用到history对象的pushState方法，在这里记录一下

### window.history对象

> Window.history是一个只读属性，用来获取History 对象的引用，History 对象提供了操作浏览器会话历史（浏览器地址栏中访问的页面，以及当前页面中通过框架加载的页面）的接口

<!--more-->

在chrome控制台输入history回车可见，history有 'length', 'scrollRestoration', 'state'三个属性，展开 __proto__ 发现有 back, forward, go, pushState, replaceState 等方法

### pushState 和 replaceState
添加和修改历史记录中的条目
语法 history.pushState(state, title, url)
```javascript
let stateObj = {
  foo: 'bar'
}
history.pushState(stateObj, 'page 1', '#')
```
history.replaceState() 的使用与 history.pushState() 非常相似，区别在于  replaceState()  是修改了当前的历史记录项而不是新建一个。 注意这并不会阻止其在全局浏览器历史记录中创建一个新的历史记录项。
```javascript
let stateObj = {
    foo: "bar"
}
history.replaceState(stateObj, "page 2", "bar.html")
```

### popstate事件
> 每当活动的历史记录项发生变化时， popstate 事件都会被传递给window对象。如果当前活动的历史记录项是被 pushState 创建的，或者是由 replaceState 改变的，那么 popstate 事件的状态属性 state 会包含一个当前历史记录状态对象的拷贝。
```javascript
window.addEventListener('popstate', function (e) {
  console.log('back btn click')
})
```

### 用户点击回退按钮对之进行"挽留"
```javascript
const hisPush = () => {
  let params = {
    key: Date.now()
  }
  window.history.pushState(params, '', '#')
}
const popstateEvent = () => {
  console.log('back btn click')
}
hisPush()
window.addEventListener('popstate', popstateEvent)
// 记得在单页中销毁绑定的回退事件
window.removeEventListener('popstate', popstateEvent)
```