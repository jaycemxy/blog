---
title: grid布局
date: 2018-11-25 17:19:31
tags:
---
## grid布局术语
- Grid Container
设置了 display: grid; 的元素，他是所有grid item的直接父项，如下面的.container
```html
<div class="container">
  <div class="item item-1"></div>
  <div class="item item-2"></div>
  <div class="item item-3"></div>
</div>
```

- Grid Item
Grid容器的直接子元素，如下的item，但注意sub-item不是
```html
<div class="container">
  <div class="item"></div> 
  <div class="item">
    <p class="sub-item"></p>
  </div>
  <div class="item"></div>
</div>
```

- Grid Line
网格线，可以是垂直的也可以是水平的

- Grid Track
两个相邻网格线之间的空间，可以想象成网格的列或行

- Grid Cell
代表了一个网格单元

- Grid Area
网格区域，可以由任意数量的网格单元组成

## 列表属性
### 父容器Grid Container
```html
<div class="container"></div>

<style>
    .container {
        width: 400px;
        height: 300px;
        border: 1px solid pink;

        display: grid;
        grid-template-columns: 10% auto 10%;
        grid-template-rows: 50px auto 50px;
    }
</style>
```
![alt text](../images/grid-container.jpeg)

### 子元素Grid Items
```html
<div class="container">
    <div class="header">header</div>
    <div class="main">main</div>
    <div class="aside">aside</div>
    <div class="footer">footer</div>
</div>

<style>
    .container {
        height: 100vh;
        border: 1px solid pink;
        display: grid;

        grid-template-columns: 10% auto 10%;  // 设置布局结构（画网格）
        grid-template-rows: 50px auto 50px;

        grid-template-areas:  // 因为上面设置的结构为三行三列
        "header header header"  // 布局也按三行三列
        ".      main   aside"  // .为空 表示该单元格不填充内容
        "footer footer footer"

        /* grid-template的简写形式，布局效果与上面相同 */
        grid-template: 
            "header header header" 50px;  // 引号内是填充单元格的名称（此处为三行），后跟该行的行宽
            ".      main   aside" auto;
            "footer footer footer" 50px;
            / 10% auto 10%;  // 规定列宽（此处为三列）
    }

    .header {
        grid-area: header;  // 给每种想要填充的网格单元命名
        background: #42b983;
    }

    .main {
        grid-area: main;
        background: #409eff;
    }

    .aside {
        grid-area: aside;
        background: #e6a23c;
    }

    .footer {
        grid-area: footer;
        background: #909399;
    }
</style>
```
最终布局效果如图：
![alt text](../images/grid-item.jpeg)

### 行、列间缝隙
只能在行/列之间创建缝隙，而不能在外部边缘创建。grid-gap是两者的缩写
```css
grid-row-gap: 10px;
grid-column-gap: 5px;

grid-gap: 10px 5px; // 等同于上面的写法
```

### justify-items和align-items（每个网格元素items相对于网格整体，和下面的属性作区分）
沿着横轴（行）或沿着纵轴（列）对齐网格内的内容
```
start: 内容与网格区域的左端对齐
end: 内容与网格区域的右端对齐
center: 内容位于网格区域的中间位置
stretch: 内容宽度占据整个网格区域空间(默认值)
```

例如，给上面的布局里的main里的内容居中
```css
...
.main {
    grid-area: main;
    background: #409eff;
    display: grid;
    justify-items: center;
    align-items: center;
}
...
```
![alt text](../images/items-center.jpeg)

### justify-content（网格整体相对于容器居中）
当网格总大小小于其容器大小时
```css
.container {
    height: 100vh;
    border: 1px solid pink;
    display: grid;

    grid-template-columns: 10% 400px 10%;  // 和之前的变化在于auto改为400px
    grid-template-rows: 50px auto 50px;
    justify-content: center;  // 使得整个容器居中

    grid-template-areas:
    "header header header"
    ".      main   aside"
    "footer footer footer"
```
![alt text](../images/container-center.jpeg)

### grid简写、grid-area简写（grid布局最终写法）
```css
.container {
  height: 100vh;
  border: 1px solid pink;
  
  display: grid;
  grid: 50px auto 50px / 10% auto 10%;  // 斜线前为横向，后为纵向
}

.header {
  background: #42b983;
  grid-row: 1 / 2;  // 所占行数
  grid-column: 1 / span 3;  // span可以理解为跨度，也可以直接写为grid-column: 1 / 4; 效果相同
}

.main {
  background: #409eff;
  grid-row: 2;
  grid-column: 2;
} 

.aside {
  background: #e6a23c;
  grid-row: 2;
  grid-column: 3;
}

.footer {
  background: #909399;
  grid-row: 3;
  grid-column: 1 / 4;
}
```

链接：https://blog.jirengu.com/?p=990
