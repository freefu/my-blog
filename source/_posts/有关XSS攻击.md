---
title: 有关 XSS
layout: post
tag: javascript
---

记录一下`XSS`的攻击方式和防御

##### XSS 全称
XSS: 
跨站脚本
cross site script
为了避免与样式 CSS 混淆，所以简称为 XSS

<!--more-->

##### XSS 攻击分类
###### 反射性
定义：
发出请求时，XSS代码出现在URL中，作为输入提交到服务器端，服务器端解析后响应，XSS代码随响应内容一起传回给浏览器，最后浏览器解析执行XSS代码。这个过程像一次反射，故叫反射性XSS

示例：
```javascript
// 参数上的内容渲染到页面
// https://www.test.com/index.php?xss=hello
// 正常显示hello
// 换成XSS脚本
// https://www.test.com/index.php?xss=<img src="null" onerror="alert(1)">
// alert(1)可以换成任何攻击型代码
```

###### 存储型
定义：
存储型XSS和反射型XSS的差别仅在于，提交的代码会存储在服务器端（数据库，内存，文件系统等），下次请求目标页面时不用再次提交XSS代码

示例：
```javascript
// 用户提交评论处提交以下代码
// css : body {display: none}
// javascript : <script>alert(1)</script>
// iframe : <iframe src="/test.html"></iframe>
```

##### 防御
- 编码：字符对应转义字符
- 过滤：移除用户上传的DOM属性，如onerror等，移除用户上传的style节点，script节点，iframe、frame节点
- 校正：避免直接对HTML Entity解码，使用DOM Parse转换，校正不配对的DOM标签