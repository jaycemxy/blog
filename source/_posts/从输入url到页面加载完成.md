---
title: 从输入url到页面加载完成
date: 2018-08-02 15:15:59
tags:
---
网上那么多总结，你理清楚了嘛
<!-- more -->
## 大致流程
- 用户输入url地址，浏览器解析url解析出主机名
- DNS查询（从主机名到一个服务器IP地址的转换）
- 建立一条与目标web服务器的TCP连接（客户端和服务端都需要到各自可收发，因此需要三次握手）
- 发送HTTP请求（请求四部分）
- 后台处理请求（监听80端口、路由、渲染HTML模版、生成响应）
- 发送HTTP响应
- 关闭TCP连接（四次挥手）
- 浏览器解析并渲染页面

### DNS查询（从主机名到一个服务器IP地址的转换）
- 网址：URL（统一资源定位符）用于定位互联网上的资源，格式一般为：协议类型://<主机名>:<端口>/<路径>/文件名
- IP地址：例如代表本机的IP就是127.0.0.1，每个网站都是靠IP来定位的。比方说知乎的网址是 https://www.zhihu.com/ 但浏览器并不知道它是什么，而是需要通过查找该网址所在服务器的IP地址获得目标，这个过程就是域名解析
- DNS查询获得IP地址

a. 查找浏览器缓存看是否有该域名对应的解析过的IP地址；
b. 查找操作系统缓存，浏览器会从hosts文件查找是否储存有DNS信息，是否有目标域名及对应的IP地址；
c. 查找路由器缓存；
d. 查找ISP(Internet Service Provider) DNS 缓存服务器；
e. 迭代查询，从顶级域名服务器的根域名服务器开始递归查询
![DNS解析.png](https://i.loli.net/2018/07/03/5b3b8e7bd5b93.png)
### 建立TCP连接
三次握手：由客户端执行connect来触发

1、client将标志位SYN设置为1，随机产生一个seq=J，并将数据包发送给server，此时client进入SYN_SEND状态，等待server确认

2、server收到数据由标志位SYN=1知道客户端想要请求建立连接，server将标志位SYN和ACK都设置为1，ack=J+1（序号+1），随机产生一个seq=k，并将数据包发送给client确认连接请求，此时server进入SYN_RCVD状态

3、client收到确认后，检查ACK是否为1，ack是否为J+1，如果正确则连接成功，此时client和server都进入ESTABLISHED状态，完成三次握手
![三次握手](https://i.loli.net/2018/08/21/5b7c1e5c7927a.png)

### 发送HTTP请求
在得到IP地址后，浏览器会发送出一个HTTP请求，请求包括请求头和请求体。请求头通常有：请求方法（GET、POST、HEAD、OPTIONS, PUT, DELETE, TRACE 和 CONNECT）、目标url、遵循的协议（http、https、file等）
### 后台处理请求
服务器通过http报文作为信息载体将数据返回给客户端

常用请求头：
```
Accept: 接收类型，表示浏览器支持的MIME类型（对应服务端返回的Content-Type）
Content-Type：客户端发送出去实体内容的类型
Content-length：用来指明发送给接收方的消息主体的大小
Cache-Control: 指定请求和响应遵循的缓存机制，如no-cache
Expires：缓存控制，在这个时间内不会请求，直接使用缓存，http1.0，而且是服务端时间
Max-age：代表资源在本地缓存多少秒，有效时间内不会请求，而是使用缓存，http1.1中
If-None-Match：对应服务端的ETag，用来匹配文件内容是否改变（非常精确），http1.1中
Cookie: 有cookie并且同域访问时会自动带上
Host：请求的服务器URL
Origin：最初的请求是从哪里发起的（只会精确到端口）,Origin比Referer更尊重隐私
Referer：该页面的来源URL(适用于所有类型的请求，会精确到详细页面地址，csrf拦截常用到这个字段)
User-Agent：用户客户端的一些必要信息，如UA头部等
```
常用响应头
```
Access-Control-Allow-Headers: 服务器端允许的请求Headers
Access-Control-Allow-Methods: 服务器端允许的请求方法
Access-Control-Allow-Origin: 服务器端允许的请求Origin头部（譬如为*）
Content-Type：服务端返回的实体内容的类型
Date：数据从服务器发送的时间
Cache-Control：告诉浏览器或其他客户，什么环境可以安全的缓存文档
Last-Modified：请求资源的最后修改时间
Expires：应该在什么时候认为文档已经过期,从而不再缓存它
Max-age：客户端的本地资源应该缓存多少秒，开启了Cache-Control后有效
ETag：请求变量的实体标签的当前值
Set-Cookie：设置和页面关联的cookie，服务器通过这个头部把cookie传给客户端
Keep-Alive：如果客户端有keep-alive，服务端也会有响应（如timeout=38）
Server：服务器的一些相关信息
```
通常请求头和响应头是相互匹配的
```
- 请求头部的Accept要和响应头部的Content-Type匹配，否则会报错
- 跨域请求时，请求头部的Origin要匹配响应头部的Access-Control-Allow-Origin，否则会报跨域错误
- 在使用缓存时，请求头部的If-Modified-Since、If-None-Match分别和响应头部的Last-Modified、ETag对应
```
### 关闭TCP连接
四次挥手
```
主动方：我已经关闭了向你那边的主动通道，此时你只能被动接受
被动方：我收到了通道关闭的消息
被动方：好的那我告诉你，我也关闭了这边向你的主动通道
主动方：最后收到通知，之后双方无法通信
```
### 浏览器解析并渲染页面
大致流程：解析HTML，构建DOM树 -> 解析CSS，生成CSS规则树 -> 下载并解析JS -> 下载并解析图片（异步下载，不会阻塞解析流程） -> 合并DOM和CSS规则，生成render树 -> 布局并渲染render树 -> 执行JS文件

- 浏览器接收 HTML 文件转换为 DOM 树
字节数据 => 字符串 => Token => Node => DOM

- 将 CSS 文件转换为 CSSOM 树
字节数据 => 字符串 => Token => Node => CSSOM

- 将 DOM 树和 CSSOM 树合并生成渲染树
这个过程不只是简单的将两者合并，渲染树只会包括需要现实的节点和这些节点的样式信息，例如 display: none; 就不会显示出来
