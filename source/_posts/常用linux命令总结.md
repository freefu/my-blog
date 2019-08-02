---
title: 常用linux命令汇总
layout: post
tag: linux
---

身为一个前端，也经常会使用到一些linux命令，so在此记录一下经常会用到的linux命令，方便以后查询和记忆。

<!--more-->

##### cd
```javascript
'cd 文件夹名' // 进入文件夹
'cd' .. // 返回上一级
```

##### ls
```javascript
'cd naonao' // 进入naonao文件夹
'ls'  // 列出闹闹文件夹下包含的内容
'll'  // 列出闹闹文件夹下包含的详细内容
```

##### mkdir
```javascript
'mkdir dist'  // 新建dist文件夹
```

##### touch
```javascript
'cd dist'  // 进入dist文件夹
'touch index.html'  // 创建index.html
```

##### clear
```javascript
'clear' // 清除当前窗口内容
```

##### rm
```javascript
'rm 文件名' // 删除文件
'rm 空文件夹名' // 删除空文件夹
'rm -r 文件夹名'  // 删除文件夹，不管其下有多少级目录，一并删除
'rm -f 文件名'  // 强制删除，没有提示
'rm -rf 文件夹名' // 删除当前目录下的所有文件并不提示（不能恢复，慎用）
```

##### scp
```javascript
'scp -r ./* root@xx.xx.xxx.xx:/xxx1'  // 复制当前文件夹下所有内容到服务器上xxx1目录
// scp -r ./* root@47.94.230.43:/root/www/test
```