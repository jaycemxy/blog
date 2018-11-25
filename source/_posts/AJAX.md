---
title: AJAX
date: 2018-08-20 22:34:01
tags: ajax
---
Ajax 的主要优势就是对页面的请求以异步的方式发送到服务器，而服务器不会以整个页面来响应请求，它会在后台处理，与此同时用户还能继续浏览页面并与页面交互。
<!-- more -->
Ajax 的核心技术就是 XMLHttpResquest 对象，这个对象充当浏览器中的脚本（客户端）与服务器之间的中间人角色，以往的请求都是由浏览器发出，而JS通过这个对象可以自己发送请求，同时自己处理响应。

### 使用原生JS写一个AJAX请求
```
function getNewContent(){
    let request = new XMLHttpRequest()  // 创建一个新对象
    request.open(method,url);  // open 方法用来指定请求方法，请求地址或路径
    request.onreadystatechange = function(){  // onreadystatechange 是一个事件处理函数，在服务器给 XMLHttpRequest 对象送回响应时触发，函数内容主要是处理响应
        if(request.readyState === 4){  // 监听请求状态的改变，当 readyState 的属性变成4时，说明请求完成了
            if(request.status >= 200 && request.status < 300){  // 如果请求完成，且状态码返回2xx，表示请求成功
                console.log(request.responseText)
            }else if(request.status >= 400){  // 如果请求完成，且状态码返回4xx，表示请求失败
                console.log('请求失败')
            }
        }
    }
    request.send(body)  // 在指定了请求体，也明确了如何处理响应后，就可以用 send 方法来发送请求
}
```

### jQuery对ajax的封装
```
$.get('filename').then(function(response){
    // 这里是response的内容
})
```

目前常见的是用ajax请求JSON格式的数据
```
$.get('/data.php').then(function(response){
    // response内容为 { "name": "jayce" }
})
```
### 同源策略
在使用AJAX时要注意同源策略，使用XMLHttpRequest对象发送请求只能访问与其所在的HTML处于同一个域中的数据，不能向其他域发送请求。同源政策的目的是为了保证用户信息的安全，防止恶意的网站窃取数据。只有（协议+域名+端口）一模一样时才允许发 AJAX 请求，eg：
- http://baidu.com 可以向 http://www.baidu.com 发 AJAX 请求吗？ no协议不同
- http://baidu.com:80 可以向 http://baidu.com:81 发 AJAX 请求吗？ no端口不一样

### CORS跨域
CORS全称是跨域资源共享（Cross-origin resource sharing）能克服 AJAX 只能同源使用的限制，告诉浏览器我们是一家的别阻止他。简单来说就是服务端在响应头中添加一个 Access-Control-Allow-Origin 的头部，头部的值为客户端的域名，eg：
```
response.setHeader('Access-Control-Allow-Origin','url')
```
### JSONP与AJAX
相同点：

- ajax和jsonp的调用方式很像，目的一样，都是请求url，然后把服务器返回的数据进行处理

不同点：

- AJAX（异步的Javascript和XML）核心是通过XMLHttpRequest请求内容，支持get、post、delete等。（通过CORS可以突破同源政策的限制实现跨域请求）

- JSONP的核心则是通过动态添加script标签来调用服务器提供的js脚本（后缀.json)，只支持get请求。（网页通过添加一个script元素向服务器请求JSON数据，这种做法不受同源政策的限制，服务器在收到请求后，将数据打包放在一个指定名字的回调函数里传回来）
