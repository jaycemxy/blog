---
title: 浅析URL
date: 2022-03-09 11:28:24
tags:
---
## URL
含义：统一资源定位符(Uniform Resource Locator)，URL=协议+域名或IP+端口号（默认的不显示）+路径+查询参数+锚点
![4158026B-FDAA-400D-B35A-C568BEA9F806.png](https://s2.loli.net/2022/03/09/p7I2Hr3CWZuQTom.png)

- 通过改变**路径**可以请求不同的页面
https://developer.mozilla.org/zh-CN/docs/Web/HTML
https://developer.mozilla.org/zh-CN/docs/Web/CSS

- 通过改变**查询参数**可以访问同一个页面不同内容
https://www.baidu.com/s?wd=hello
https://www.baidu.com/s?wd=parent

- 通过改变**锚点**可以看到同一页面同一内容的不同位置
https://developer.mozilla.org/zh-CN/docs/Web/CSS#%E6%95%99%E7%A8%8B#教程
https://developer.mozilla.org/zh-CN/docs/Web/CSS#%E5%8F%82%E8%80%83%E4%B9%A6#参考书

注意：

锚点不支持中文，所以会把中文变成#%E6%95%99%E7%A8%8B

锚点不会发送给服务器，所以在开发者工具的Network面板看不到

***
IP网际协议（Internet Protocol）也叫域名，是一串字符串，用来定位一台设备

端口（80、443等）用来定位一个设备的服务，IP和端口缺一不可

使用**ping**命令**获取IP地址**
![6A8E48A0-D484-47E6-A982-A91EE7CA601D.png](https://s2.loli.net/2022/03/09/pcKEJ3QUFZBYmuh.png)

使用**nslookup**命令**解析域名**
![E08CB706-EBF4-4C29-AEE1-E7283D5E4D30.png](https://s2.loli.net/2022/03/09/bBQvkqshZ4CGVnx.png)

***
DNS域名系统（Domain Name System），将域名（baidu.com）和它的IP地址（220.181.38.148）对应起来

用**curl**命令**发HTTP请求**
![B6748EB5-3697-43BD-8D77-19A693C08EF9.png](https://s2.loli.net/2022/03/09/6astOqfmZX7v12N.png)