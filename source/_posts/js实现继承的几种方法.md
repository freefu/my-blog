---
title: js继承的几种方法以及缺陷
layout: post
tag: javascript
---

通过继承，我们能十分优雅的实现现有代码的重用，记录一下js中实现继承的几种方法

<!--more-->

##### 通过构造函数实现继承
```javascript
function P () {
  this.name = 'parent'
}
function C () {
  P.call(this)
  this.age = '18'
}

let o = new C()
console.log(o.name) // 'parent'

P.prototype.say = function () {
  console.log('hi')
}
console.log(o.say())  // Uncaught TypeError: o.say is not a function
// 这里明显继承不了p原型对象上的方法
```

##### 通过原型链实现继承
```javascript
function P () {
  this.name = 'parent'
  this.class = [1,2,3,4]
}
function C () {
  this.age = '18'
}
C.prototype = new P()

let o = new C()
console.log(o.name) // 'parent'

P.prototype.say = function () {
  console.log('hi')
}
console.log(o.say())  // 'hi'

let o2 = new C()
let o3 = new C()
o3.class.push(5)
console.log(o.class, o2.class, o3.class)  // (5) [1, 2, 3, 4, 5] (5) [1, 2, 3, 4, 5] (5) [1, 2, 3, 4, 5]
// 这里所有实例的class都被修改了，明显不是继承的最好办法
```

##### 混合写法
```javascript
function P () {
  this.name = 'parent'
  this.class = [1,2,3,4]
}
function C () {
  P.call(this)
  this.age = '18'
}
C.prototype = new P()

let o = new C()
console.log(o.name) // 'parent'

P.prototype.say = function () {
  console.log('hi')
}
console.log(o.say())  // 'hi'

let o2 = new C()
let o3 = new C()
o3.class.push(5)
console.log(o.class, o2.class, o3.class)  // (4) [1, 2, 3, 4] (4) [1, 2, 3, 4] (5) [1, 2, 3, 4, 5]
// 这种方法还是ok的，but...
console.log(o.constructor === C, o.constructor === P)  // false true
// 这里分不出实例到底是父构造函数构造的还是子构造函数构造的了，so还不是最好的办法
```

##### 混合办法优化
```javascript
function P () {
  this.name = 'parent'
  this.class = [1,2,3,4]
}
function C () {
  P.call(this)
  this.age = '18'
}
// C.prototype = new P()
// C.prototype = P.prototype
var obj = Object.create(P.prototype)
C.prototype = obj // 这里为了原型链串起来 obj这个新对象的原型对象就是P.prototype这个对象
C.prototype.constructor = C

let o = new C
console.log(o.constructor === C, o.constructor === P) // true false
```

##### es6的继承写法
```javascript
class P {
  constructor () {
    this.name = 'parent'
  }
  say () {
    console.log('hi')
  }
}
class C extends P {
  constructor () {
    super()
    this.age = 18
  }
}
var c = new C()
console.log(c)
// C {name: "parent", age: 18}
```