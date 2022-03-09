---
title: JS的诞生
date: 2022-03-09 19:35:48
tags:
---
- 李爵士发明了HTML
- 赖先生发明了CSS
- 布兰登发明了JS

## JS简介
- JavaScript是一种解释性脚本语言（代码不进行预编译），主要用来向HTML页面添加交互行为。
- 可以直接嵌入HTML页面，但写成单独的js文件有利于结构和行为的分离。
- 由ECMA（欧洲电脑制造商协会）通过ECMAScript实现语言的标准化。它被世界上的绝大多数网站所使用，也被世界主流浏览器（Chrome、IE、Firefox、Safari、Opera）支持。
## JS用途
JavaScript常用来完成以下任务：
1. 嵌入动态文本于HTML页面
2. 对浏览器事件作出响应
3. 读写HTML元素
4. 在数据被提交到服务器之前验证数据
5. 检测访客的浏览器信息
6. 控制cookie，包括创建和修改等
## JS诞生
- 1994年，互联网刚兴起，网景公司（Netscape）发布了Navigator浏览器0.9版，但这个版本的浏览器只能用来浏览，不具备与访问者互动的能力。因此网景公司急需一种网页脚本语言，使得浏览器可以与网页互动。
- 1995年，Sun公司将Oak语言更名为Java并推向市场，并宣称“Write Once, Run Anywhere”。网景公司深受Java的影响，网景公司高层都非常信赖Java，所以网景公司决定要蹭Java的流量，新开发一门语言，用于浏览器的交互。
- 1995年4月，BrendanEich（布兰登·艾奇） 加入网景公司，被指定成了“新语言”的设计师，并且要求这个“新语言”要和Java足够的相似（面向对象思想），但是要比Java能够更加简单地上手。Brenden花了10天时间便把这门“新语言”的最初版本设计了出来，命名为JavaScript，对外宣称JavaScript是Java的补充。
- 设计思路

```
（1）借鉴C语言的基本语法；
（2）借鉴Java语言的数据类型和内存管理；
（3）借鉴Scheme语言，将函数提升到"第一等公民"（first class）的地位；
（4）借鉴Self语言，使用基于原型（prototype）的继承机制。
```

## JS命名
- 最初为了紧跟Java（有一种咖啡叫Java），这门“新语言”被命名为Mocha（有一种咖啡叫Mocha）
- 但由于商标的问题，以及网景公司很多产品已经使用了“Live”作为产品名前缀，Mocha更名为LiveScript。
- 由于网景公司与Sun公司有一些合作，Sun把Java这个商标授权给了网景公司，于是LiveScript更名为JavaScript。
- Mocha摩卡 => LiveScript => JavaScript

## JavaScript与Java的关系
- JavaScript的基础语法和对象体系，是模仿Java而设计的。
- JavaScript语言的函数是一种独立的数据类型以及基于原型对象的继承链，是与java语法最大的两点区别。
- JavaScript不需要编译，由解释器直接执行。

## JS与ECMAScript的关系
- ECMAScript是纸上的标准，JS是浏览器的实现
- 纸上标准往往落后于浏览器，先实现，再写进标准
