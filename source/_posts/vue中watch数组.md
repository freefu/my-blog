---
title: vue中watch数组
layout: post
tag: vue
---

在项目开发中遇见一个小问题，做个记录

##### 场景描述
有一个空数组，我通过某个事件对这个数组进行一些操作，然后用这个数组去做一些判断，但是发现没有起作用，然后我就想可以用watch监听一下是否这个数组已经发生改变，后来发现并没有执行打印，然后通过查询才知道通过下标对数组进行的某些操作是无法watch到变化的。

<!-- more -->

```javascript
data() {
  return {
    arr: []
  }
},
watch: {
  arr (new, old) {
    console.log(new, old) // 这里不会打印
  }
},
methods: {
  changeArr () {
    arr[0] = 'hello world'
  }
}
```
##### 解决方案
通过 <code>this.$set(arr, index, newVal)</code> 来进行赋值


