---
title: js中this的指向问题
layout: post
tag: javascript
---

##### this到底是什么呢
this是JavaScript里的一个关键字
this是包含它的函数作为方法被调用时所属的对象
- 包含它的函数
- 被调用
- 所属的对象

<!--more-->

##### 包含this的函数被调用的几种情况
1.<b>纯粹的函数调用</b>
```javascript
function foo () {
  return this
}
foo() === window // 等同于 window.foo() === window
// true
```
由以上代码得知：foo作为纯粹的函数调用时，foo所属对象是全局的window，所以this指向window

2.<b>作为对象的方法被调用</b>
```javascript
var obj = {
  name: 'obj',
  foo: function () {
    console.log(this.name)
  }
}
obj.foo()
// obj
```
由以上代码得知：foo作为obj对象的方法被调用时，foo所属对象是obj，所以this指向obj

3.<b>作为构造函数被调用时</b>
```javascript
function foo () {
  this.name = 'foo'
}
var newFoo = new foo()
console.log(newFoo.name)
// foo
```
这里怎么解释呢？这就得看new运算符到底做了什么操作，call又做了哪些操作。
```javascript
// new内部原理
new foo() = {
  var obj = {}
  obj.__proto__ = foo.prototype
  var result = foo.call(obj)
  return typeof(result === 'object') ? result : obj
}
// call 内部原理
Function.prototype.mycall = function (context) {
  var context = context || window
  var args = []
  var result
  args = [...arguments].slice(1)
  context.f = this
  result = context.f(...args)
  delete context.f()
  return result
}
```
由以上代码得知：foo作为构造函数被调用时，foo所属对象是构造出来的新对象，所以this指向构造出来的新对象newFoo

4.<b>call和apply调用</b>
```javascript
var name = 'windowName'
function foo () {
  console.log(this.name)
}
var obj = {
  name: 'obj'
}
foo.call(obj)
// obj
```
原理同上一个

##### es6箭头函数中的this
```javascript
var name = 'windowName'
var f1 = () => {
  console.log(this.name)
}
f1()
// windowName
var obj = {
  name: 'objName',
  f2: f1
}
obj.f2()
// windowName
```
由此可知<u>箭头函数中的this对象是函数定义时所属对象</u>，并不是函数调用时所属对象

##### 改变this指向的方法
1.<b>call()和apply()</b>
```javascript
var obj1 = {
  name: 'obj1',
  f: function () {
    console.log(this.name)
  }
}
var obj2 = {
  name: 'obj2'
}
obj1.f.call(obj2)
// obj2
```
由于this指向包含他的函数作为方法被调用时所属的对象，所以call和apply都能改变this的指向

2.<b>bind()</b>
这个bind()是怎么改变this指向的呢？需要看一下bind()是做什么的
>定义：bind()方法创建一个新的函数，在调用时设置this关键字为提供的值。并在调用新函数时，将给定参数列表作为原函数的参数序列的前若干项。
```javascript
// bind() 实现原理
Function.prototype.bind = function () {
  var self = this,                        // 保存原函数
      context = [].shift.call(arguments), // 保存需要绑定的this上下文 第一个参数
      args = [].slice.call(arguments);    // 剩余的参数转为数组
  return function () {                    // 返回一个新函数
    self.apply(context,[].concat.call(args, [].slice.call(arguments)));
  }
}
//
var name = 'windowName'
var obj = {
  name: 'obj',
  f: function () {
    console.log(this.name)
  }
}
var f1 = obj.f
f1()
// windowName
var f2 = f1.bind(obj)
f2()
// obj
```
这里可以看出来bind方法返回的是一个方法，并不像call和apply那样直接调用