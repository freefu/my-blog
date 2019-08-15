---
title: node操作mysql
layout: post
tag: mysql nodejs
---

##### 常用简单的sql语句
```sql
use `my-blog`;

-- show tables;

-- 插入
-- insert into users (username, `password`, realname) values ('lisi', '123', '李四');

-- 查询
-- select * from users;
-- select id, username from users;
-- select * from users where username='zhangsan';
-- select * from users where username='zhangsan' and password='123';
-- select * from users where username like '%zhang%';
-- select * from users where password like '%1%' order by id desc; --排序

-- 更新
-- update users set realname='李四2' where username='lisi';

-- 删除和软删除
-- delete from users where username='lisi';
-- update users set state='0' where username='lisi';
```

##### 安装mysql
```javascript
npm install mysql --save
```

##### node 操作 mysql
```javascript
// 引入
const mysql = require('mysql')

// 创建连接
const con = mysql.createConnection({
  host: 'localhost',
  user: 'root',
  password: 'pwd',
  port: '3306',
  databases: 'my-blog'
})

// 连接
con.connect()

// 查询
const sql = 'select id, username from users;'
con.query(sql, (err, res) => {
  if (err) {
    console.error(err)
    return
  }
  console.log(res)
})

// 关闭连接
con.end()
```