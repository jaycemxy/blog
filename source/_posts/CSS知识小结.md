---
title: CSS知识小结
date: 2022-03-08 13:44:37
tags:
---
## 浏览器渲染原理
1. 解析html文档并构建DOM树
2. 解析CSS标签并构建CSSOM树
3. 结合DOM树和CSSOM树成一颗渲染树（render tree）
4. 布局渲染树（layout），将所有渲染树的所有节点进行平面合成
5. 绘制渲染树（painting），将布局绘制在屏幕上
   
![1480597-08e6f204c42595ae.png](https://s2.loli.net/2022/03/08/ryYILf5aVwxQ9u1.png)

第4、5步合称为渲染，网页生成的时候至少会渲染一次，用户访问过程中还会不断重新渲染

重新渲染就是要重新生成布局和重新绘制，分别称为“重排”和“重绘”。重绘不一定重排，但重排必然导致重绘。

参考文章：
https://www.ruanyifeng.com/blog/2015/09/web-page-performance-in-depth.html

https://developer.mozilla.org/zh-CN/docs/Web/Performance/How_browsers_work

## CSS动画--transform
1. 位移translate
```
translateX(2em); translateY(-50px); translate(12px, 50%);
```

用translate(-50%, -50%)做元素绝对居中
http://js.jirengu.com/sisefaxuzo/2/edit?html,css,output

2. 缩放scale
```
scaleX(2); scaleY(0.5); scale(2, 0.5);
```
3. 旋转rotate
```
rotateX(10deg); rotateY(10deg); rotate(45deg);
```
4. 倾斜skew
```
skewX(30deg); skewY(1.07rad); skew(30deg, 20deg);
```

一般配合transition使用
```
transition: 属性名 | 时长 | 过渡方式 | 延迟 
eg：transition：left 200ms ease

过渡方式：linear | ease | ease-in | ease-out | ease-in-out | step-start | step-end 
```
tips：
- 可用逗号分隔两个属性
transition：left 200ms,top 400ms;
- 可用all代表所有属性
transition：all 2s;

举个栗子🌰
http://js.jirengu.com/veceporavo/1/edit?html,css,output

## CSS动画--animation
第一步：声明关键帧@keyframes xxx，有两种语法from...to...和百分数
```CSS
@keyframes xxx {
  from {
    transform: translateX(0%);
  }
  to {
    transform: translateX(100%);
  }
}

@keyframes yyy {
  0% { top: 0; left: 0; }
  30% { top: 50px; }
  68%, 72% { left: 50px; }
  100% { top: 100px; left: 100%; }
}
```

第二步：添加动画animation
```css
animation: 动画名 | 时长 | 过渡方式 | 延迟 | 次数 | 方向 | 填充模式 | 是否暂停

时长：1s或1000ms
过渡方式：linear | ease | ease-in | ease-out | ease-in-out | step-start | step-end 
次数：1 | 10 | infinite
方向：reverse | alternate | alternate-reverse
填充模式：none | forwards | backwards | both 
是否暂停：paused | running
```

举个栗子🌰
http://js.jirengu.com/bivavexere/1/edit?html,css,output

