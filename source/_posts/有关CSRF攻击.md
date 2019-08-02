---
title: 有关 CSRF
layout: post
tag: javascript
---

记录一下`CSRF`的攻击方式和防御

##### CSRF 全称
CSRF: 
Cross-site request forgery
跨站请求伪造

<!--more-->

##### 攻击原理
<div style="width: 600px; height: 500px;">
![image](http://static.guobaoyoo.com/img/blog/CSRF.png?imageView2/0/q/75|watermark/2/text/YmxvZy5ndW9iYW95b28uY29t/font/5b6u6L2v6ZuF6buR/fontsize/400/fill/I0NDQ0NDQw==/dissolve/50/gravity/SouthEast/dx/10/dy/10)
</div>

- 用户是网站A的注册用户，登录A网站。
- A网站核实用户身份是否正确（账号密码验证），如果用户身份正确，则下发cookie，cookie则保存到用户的浏览器中。
- 当保存在用户浏览器中的cookie未过期时，用户又访问网站B。
- 网站B中有引诱用户的一个点击，这个点击可能是一个链接，这个链接则是一个访问网站API的一个接口。
- 那么用户一点击引诱链接则发出了对网站A的API的请求，此时请求上会带着保存在用户处的cookie，网站A收到请求，对身份确认时还认为是正确合法的用户，那么就会执行收到的请求。
  
##### 防御
- 通过referer检测用户提交（攻击者可以修改referer值）
- 加token验证
- 隐藏令牌（跟第二条类似，可以加在请求头中）
