---
title: web前端性能优化
date: 2018-08-14 15:00:46
tags: 性能优化
---
对于一个数据请求来说，可以分为发起网络请求、后端处理、浏览器响应三个步骤。浏览器缓存可以帮助我们在第一和第三步骤中优化性能。
<!-- more -->
## 1、发送HTTP请求，使用Cache-Control

通过在 HTTP 协议添加Cache-Control头部来告诉浏览器当前资源是否缓存，帮助浏览器进行有条件请求。Cache-Control 的值可以是 public, private, no-cache, no-store, no-transform 等
- max-age(单位为 s) 设定缓存最大的有效时间，Cache-Control: max-age=3600 表示该资源在浏览器端一个小时内均有效。
- s-maxage(单位是 s) 设定共享缓存时间，比如 CDN 或者代理。
- no-store 网络资源不缓存，每次都到服务器上拉取。
- no-cache 表示网络资源可以缓存一份，但使用前必须询问服务器此资源是不是最新的。
- public 表明响应可以被任何对象（客户端，代理服务器等）缓存。
- private 表明响应只能被单个用户缓存，其它用户或者代理服务器不能缓存这些数据。

## 2、发送HTTP请求，增加域名以并行下载资源
把组件分散到不同的域名下最大化并行下载数。

但是对于需要减少DNS查询来说，就要减少域名，想增加HTTP请求的并发数又要增加域名，两者产生矛盾。权衡之下，如果文件很少，就没有必要增加域名；反之，就增加域名。基于DNS查询的副作用，最佳的不同域名数是2-4。


## 3、接收响应时，使用Etag

Etag是服务器和浏览器之间判断浏览器缓存中某个组件是否未发生改变的一种机制，服务器像下面这样设置Etag：
```
HTTP/1.1 200 OK
Last-Modified: Tue, 12 Dec 2006 03:03:59 GMT
ETag: "10c24bc-4ab-457e1c1f"  // md5
Content-Length: 12195
```
之后，当浏览器要验证该组件是否修改过，会在请求头里设置if-None-Match，并附上这个md5值。于是在请求的时候，如果这个md5值和你之前文件的md5值一样，说明文件没有更新，不需要下载
```
GET /i/yahoo.gif HTTP/1.1
Host: us.yimg.com
If-Modified-Since: Tue, 12 Dec 2006 03:03:59 GMT
If-None-Match: "10c24bc-4ab-457e1c1f"  // 与原文件相同，返回304
HTTP/1.1 304 Not Modified
```

Etag和缓存的区别在于：缓存不会发请求，而ETag会发请求，并比较md5值是否一样，从而返回304。

## 4、接收响应时，使用Gzip压缩

传输时用Gzip压缩，压缩可以通过减少http响应的大小减少响应时间。从HTTP/1.1开始，客户端通过http请求中的Accept-Encoding头部来提示支持的压缩：
```
Accept-Encoding: gzip, deflate
```

如果服务器看到这个头部，它可能会选用列表中的某个方法压缩响应。服务器通过Content-Encoding头部提示客户端：
```
Content-Encoding: gzip
```
gzip一般可减小响应的70%。尽可能去压缩更多类型的文件。html，脚本，样式，xml和json等等都应该被压缩，而图片，pdf等等不应该被gzip，因为它们本身已被压缩过。

## 5、对于CSS和JS文件，使用CDN（Content Delivery Network）

用户离服务器越近，会减少响应时间。CDN是一群不同地点的服务器，它将资源分布到世界各地，这样不同地点的人会根据距离选择离得最近的服务器，从而使页面加载更快

## 6、调整CSS和JS文件位置
将CSS放在头部，让它能够尽早下载；JS放在body最后，确保JS不会阻塞HTML加载，以保证用户先看到一个完整画面

对于CSS文件来说，IE不会阻塞HTML，会在看到标签时就渲染在页面上，回头等CSS加载完后，再次重新渲染页面；而Chrome会阻塞HTML，它会在CSS下载好之后再渲染标签。所以最好的方式是将CSS文件放在一开始就需要加载的位置上

对于JS文件，不管是IE还是Chrome，它都会阻塞HTML的渲染，所以干脆将它放在body最后，让它最后一个加载。

## 7、压缩图片
<1> 优化jpg和png

使用 pngcrush 或其它工具压缩png
使用 jpegtran 或其它工具压缩jpeg

如果真的需要追求各种图片的极限压缩，可以参阅这些工具的文档，但是对于一般的Web应用，面对的图片种类多样，几乎不可能在工程中实现对每种工具的独立配置，因此推荐使用以下工具来进行优化。
- ImageOptim (Mac) 主页：https://imageoptim.com/
Mac平台下非常赞的图片优化工具，只需要把需要优化的图片拖拽进ImageOptim，就能够完成对图片的优化。设置选择的也很丰富，目前支持JPG和PNG的优化。
- Kraken (Web) 主页：https://kraken.io/
在免费模式下可以上传图片，优化后打包下载。适合偶尔有图片优化需求，或者不在开发机上没有优化软件可以使用的情况。
- 智图 (Web) 主页：http://zhitu.tencent.com/
腾讯ISUX团队有篇文章介绍智图：http://isux.tencent.com/zhitu.html

<2> 优化SVG

SVGO 工具可以缩减SVG文件的体积；由于SVG是基于XML的格式，本质上是纯文本，所以还可以采用GZIP压缩来减小传输大小，但这需要一些服务器配置。

### 自动优化
主要介绍CDN、Grunt/Gulp、Google PageSpeed三种方式。

1、CDN

CDN七牛和又拍在这方面都做了大量工作。其工作方式为，向CDN请求图片的URL参数中包含了图片处理的参数（格式、宽高等），CDN服务器根据请求生成所需的图片，发送到用户浏览器。

七牛云存储的图片处理接口极其丰富，覆盖了图片的大部分基本操作，例如：
- 图片裁剪，支持多种裁剪方式（如按长边、短边、填充、拉伸等）
- 图片格式转换，支持JPG, GIF, PNG, WebP等，支持不同的图片压缩率
- 图片处理，支持图片水印、高斯模糊、重心处理等

2、Grunt/Gulp

用于图片优化的Grunt组件：grunt-image。前端工程师的重复性工作，例如合并静态资源、压缩JS和CSS文件、编译SASS等都可以使用Grunt等自动化工具批量完成，图片优化也是如此。

grunt-image非常强大，按照作者的介绍，其内部加载的图片优化工具包括了pngquant, optipng, advpng, zopflipng, pngcrush, pngout, mozjpeg, jpegRecompress, jpegoptim, gifsicle和svgo。支持批量自动优化PNG, JPG, SVG和GIF，速度也不错，配置方式支持单图片优化和全目录优化

3、Google PageSpeed

Google PageSpeed这个服务器模块，可以在apache或ngnix中加载，通过在服务器配置文件中进行设置来进行自动化的优化。对于图片格式转换、图片优化甚至图片LazyLoad都有相关选项。
