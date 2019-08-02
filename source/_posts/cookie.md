---
title: cookie
layout: post
tag: javascript
---

平时在工作中经常用到对cookie进行操作的方法

### getCookie
```javascript
function getCookie (c) {
  let dc = document.cookie
  if (dc.length > 0) {
    let cs = dc.indexOf(c + '=')
    if (cs !== -1) {
      cs = cs + c.length + 1
      let ce = dc.indexOf(';', cs)
      if (ce === -1) {
        ce = dc.length
      }
      return decodeURI(dc.substring(cs, ce))
    }
  }
  return ''
}
```

### setCookie
```javascript
function setCookie (c, v, e) {  // e为毫秒，改变setTime改变单位
  var a = new Date()
  a.setTime(a.getTime() + e) 
  document.cookie = c + '=' + escape(v) + ((e == null) ? '' : ';expires=' + a.toUTCString())
}
```