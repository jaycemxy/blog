---
title: HTML入门笔记1
date: 2022-03-02 17:06:38
tags:
---
## HTML简介
- 全称：超文本标记语言（HyperText Markup Language）
- 发明者：由物理学家蒂姆·伯纳斯-李（Tim Berners-Lee）发明
- 特点：支持超链接，点击链接就可以跳转到其他网页，构成整个互联网

## HTML起手写什么
!+tab键生成HTML模板
```
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width,initial-scale=1.0">  <!-- 禁止缩放 -->
    <title>Document</title>
</head>
<body>

</body>
</html>
```

## 全局属性
### 概念：
全局属性是所有 HTML 元素共有的属性，它们可以用于所有元素，即使属性可能对某些元素不起作用。
### 常见的全局属性：
1. class：以空格分隔元素的类名，CSS 和 Javascript 通过类选择器 (class selectors) 或 DOM 方法 ( document.getElementsByClassName) 来选择和访问特定的元素。
```
<div class="mainMessage middle">
    这里是页面中间的信息
</div>
```
- 通过.mainMessage 或 .middle都可以为这个div添加样式
2. id：定义唯一标识符，该标识符在整个文档中必须是唯一的。但不推荐使用，因为id不能命名为window上已有的全局变量，限制太多，不方便新手起名
3. style：规定元素的行内样式（inline style），它是Html的属性，而非CSS。样式展示的优先级行内样式>CSS
![HTML-style.png](https://s2.loli.net/2022/03/02/RLQp7KXBFNr4cyP.png)
4. tabindex：设置元素的 Tab 键控制次序，参数包括正数（按数字顺序访问）、0（最最最后一个访问）、负数（别来访问我）

## 章节标签🏷
![HTML-章节标签.png](https://s2.loli.net/2022/03/02/NnXAbS7gZJjdO91.png)

## 常用内容标签🏷
1. 加入强调
```
斜体强调<em>强调内容</em>
加粗强调<strong>强调内容</strong>
```
2. 加入代码
```
加入一行代码<code> </code>
加入多行代码<pre> </pre>
```
3. 水平横线与回车（无结束标签）
```
回车</br>
水平横线</hr>
```
4. 无序列表&有序列表&自定义标签

新闻信息
```
<ul>
    <li>新闻1</li>
    <li>新闻2</li>
</ul>
```
豆瓣电影排行
```
<ol>
    <li>肖申克的救赎</li>
    <li>教父</li>
</ol>
```
自定义标签（只能有一个dt，但可以有多个dd）
```
<dl>
    <dt>自定义列表title</dt>
    <dd>自定义列表信息</dd>
</dl>
```
5. a标签

创建一个可点击的图片
```
<a href="http://www.imooc.com" target="_blank">
    <img src="https://s2.loli.net/2022/03/02/8xgcYprTRe1ZnEk.png" >
</a>
```