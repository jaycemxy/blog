---
title: cookie、session及localStorge
date: 2018-08-01 21:43:20
tags: cookie
---
说说cookie和localStorage的区别吧！这个几乎每次面试都会被提一嘴的问题
<!-- more -->
# cookie
## 1、什么是cookie

cookie是服务器发送到用户浏览器并保存在本地的一小块数据，用来记录某些当页面关闭或刷新后仍需记录的信息，它会在下一次浏览器向同一服务器发送请求时携带并发送给该服务器，在控制台用 document.cookie 可以查看当前正在浏览的网站cookie

要点：
- 服务器通过 set-cookie 头给客户端一串字符串
- 浏览器每次访问相同域名的页面时必须带上 cookie 作为请求头（Request-Header）
- cookie一般用来记录用户信息，默认在关闭页面后失效，后台代码可以设置cookie的过期时间

## 2、cookie用例

以注册登录为例：
1、注册账号时，服务器把你的用户名和密码存入数据库

2、登陆的时候，浏览器发送post请求，服务器把你的账号密码跟数据库里的做匹配，若匹配成功，则发送一个响应头给浏览器，如：
```
Set-Cookie: sign_in_email=xxx@xxx.com
```

3、这就是cookie，里面记录着你的登陆信息，浏览器会在一段时间内保存cookie

4、当你再访问相同域名的网页时，浏览器会带着这个cookie发请求，服务器会将cookie里的信息和数据库匹配，若匹配上了，则直接发送给你已经登陆上的页面

5、cookie默认在关闭页面后被清除，而后台可以设置cookie的过期时间

## 3、Cookie的分类

浏览器所持有的cookie分为两种：
- Session Cookie（会话期即非持久Cookie）：是最简单的cookie，它不需要指定过期时间（expires）或是有效期（Max-Age），它仅在会话期内有效，浏览器关闭后它会被自动删除
- Permanent Cookie（持久性Cookie）：可以指定过期时间或有效期

设置cookie过期时间：
- setMaxAge
```
cookie.setMaxAge(60*60)  // 过期时间为1小时
```

- expires(不要用)
```
Set-Cookie: name=Nicholas; expires=Sat, 02 May 2009 23:38:25 GMT  //星期六 5月2号 2009年 23:38:25时过期
```
因为expires设置是根据本地时间，但各个地方时区不同设置时容易造成混乱

## 4、Cookie的作用：
- 保存用户登录状态：例如将用户ID储存在一个cookie内，这样用户在下次访问该页面的时候就不需要重新登录了，常见于很多论坛或社区，还可以设置过期时间，当超过这个期限，cookie会自动消失。因此，系统往往可以提示用户保持登录状态的时间，常见的有一个月、三个月、一年等
- 跟踪用户行为：例如一个天气预报网站，能根据用户选择的地区显示当地天气情况，但如果每次都需要用户选择所在地十分不人性化，当利用了cookie系统就能记住上一次访问的地区，当下次打开页面时，就会自动显示上次用户所在地区的天气情况
- 定制页面：如果网站提供了换肤或者更换布局的功能，使用cookie就可以记录下用户的选项，当用户下次访问时，仍然保持的是上一次设置的页面风格

# cookie和session的区别
HTTP是一个无状态协议，因此cookie最大的作用就是存储sessionID，用来唯一标识用户

cookie：
- 它是由服务器通过Set-Cookie发送给浏览器的一串字符串构成
- cookie被保存在客户端，并且每次都会随请求发送给服务器
- 没有session之前，cookie里记录了用户信息，任何人可读可写，十分不安全

session：
- session是依附于cookie而存在的
- 服务器通过 session 给用户一个 sessionID ，sessionID 对应服务器中的一小块内存（一般是一个随机数），通过 set-cookie 添加到http响应头部中
- 浏览器发送请求时会带上cookie，服务器通过sessionID来识别用户
- 这样暴露给其他人的只有一个随机数sessionID，增加安全性

# localStorage和sessionStorage
localStorage

localStorage是html5提供的一个本地存储web storage特性的API，实质上是一个哈希，有setItem、getItem、clear等属性
- localStorage 与 http 无关，HTTP不会带上localStorage的值
- 只有相同域名的页面才能互相读取 localStorage
- 经常在记录某个信息有没有提示给用户时用到
- localStorage 永久有效，除非用户清理缓存

sessionStorage 与 localStorage 唯一一点区别是，sessionStorage 在用户关闭页面后就会失效

# localStorage与cookie的区别
- cookie会随请求被发送到服务器上，但localStorage不会
- cookie大小一般在4k左右，localStorage一般5MB左右（服务器不同也会有所不同）
