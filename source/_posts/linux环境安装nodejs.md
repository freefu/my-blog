---
title: linux环境安装nodejs
layout: post
tag: javascript
---

### 下载nodejs安装包并上传
再官网选择正确的安装包下载
[https://nodejs.org/en/download/](https://nodejs.org/en/download/)
比如下载的包为 node-v10.14.0-linux-x64.tar.xz
然后用scp命令上传到服务器
```
scp ./node-v10.14.0-linux-x64.tar.xz root@xx.xx.xxx.xx:/app/software
```

<!--more-->

### 解压安装包
```
tar -xvf node-v10.14.0-linux-x64.tar.xz
tar -xvf node-v10.14.0-linux-x64 nodejs
```

### 配置
```
ln -s /app/software/nodejs/bin/npm /usr/local/bin/ 
ln -s /app/software/nodejs/bin/node /usr/local/bin/
```

### 检测
```
node -v
npm -v
```

### 升级node版本
```
npm install -g n

// 安装node最新版本：
n latest

// 安装稳定版
n stable

// 查看已安装版本
n

// 删除指定版本
n rm 0.10.1

// 指定某个版本运行脚本
n use 0.10.1
```