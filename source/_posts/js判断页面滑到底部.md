---
title: js判断页面滑到底部
layout: post
tag: javascript
---

js判断页面滑到底部算是比较简单的一个功能，记录一下涉及到的知识点

### 实现想法
功能涉及到三个长度，分别是
- 屏幕的高度或者说是窗口高度，用变量`CH`代替
- 浏览器所有内容高度，用变量`DH`代替
- 文档发生滚动时，被卷去的高度，用变量`SH`代替

<!--more-->

![image](http://static.guobaoyoo.com/img/blog/scroll.png?imageView2/0/q/75|watermark/2/text/YmxvZy5ndW9iYW95b28uY29t/font/5b6u6L2v6ZuF6buR/fontsize/400/fill/I0NDQ0NDQw==/dissolve/50/gravity/SouthEast/dx/10/dy/10)
怎么算页面滑到底部呢？
就是 屏幕或者窗口的高度 加上文档被卷去的高度 等于 浏览器所有内容高度
即 `CH + SH === DH`
那么就好说了只要获取这三个值做个if判断就可以了

### 存在问题
在不同浏览器和不同规定DTD的情况下获取这三个高度是不同的，所以为了兼容写法
```javascript
scrollBottom () {
  let SH = document.body.scrollTop || document.documentElement.scrollTop
  let CH = document.documentElement.clientHeight || document.body.clientHeight
  let DH = document.documentElement.scrollHeight || document.body.scrollHeight
  return (SH + CH) === DH
}
```