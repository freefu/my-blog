---
title: requestAnimationFrame
layout: post
tag: javascript
---

前端实现动画的方式有很多种
- html5 canvas
- css3 animation + @keyframes
- css3 transition
- js setTimeout / setInterval

之前工作当中多多少少都有接触到也用过这些，但是没有用过requestAnimationFrame(以下都简写为RAF)

##### RAF是什么
> window.requestAnimationFrame() 告诉浏览器——你希望执行一个动画，并且要求浏览器在下次重绘之前调用指定的回调函数更新动画。该方法需要传入一个回调函数作为参数，该回调函数会在浏览器下一次重绘之前执行