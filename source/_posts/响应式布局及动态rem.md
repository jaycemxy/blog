---
title: 响应式布局及动态rem
date: 2018-08-03 16:54:20
tags: 响应式 rem
---
移动端适配及动态rem方案
<!-- more -->
## 使用meta标签
- - -
在做移动端页面时，要使用meta标签，它用来控制页面在移动端不要缩放，如果没有指定，将会默认为980px，它指定了以下内容：
```
<meta name="viewport" content="width=device-width, initial-scale=1, user-scalable=no, maximum-scale=1, minimum-scale=1">
```
芳芳老师曾经给过一个答案，它的作用是这样来的：一开始的页面都是为PC端准备的，在乔布斯推出了iPhone3GS手机后，PC端的页面不能适应手机屏幕的大小，所以苹果工程师想出了一个办法，默认将手机模拟成980px，使得页面缩小可以适应手机屏幕，后来智能手机普及，这个功能部分网站不再需要，所以就用meta:vp来控制手机页面不要缩放。
## @media媒体查询功能
- - -
有两种方式加入媒体查询
- 在style标签中直接插入 @media 后接查询条件，如果满足条件，里面的CSS代码就生效
```
<style>
  @media(min-width: 320px) and (max-width: 375px){  //在设备最小宽度为320px，最大宽度375px时body的背景颜色才会被改变
    body{
        background: pink;
    }
  }
</style>
```
- 在link标签中引用media
```
<link rel="stylesheet" href="style.css" media="(max-width: 320px)">  //在最大宽度为320px时，才会执行style.css文件
```
这里有一点要注意，虽然css代码是有条件的执行，但是css文件总是会被下载，只是看是否生效
### 用很多套css样式来匹配不同的设备宽度
- - -
```
<style>
  @media (min-width: 769px){  /*769~正无穷*/
    body{
        background: purple;
    }
  }
  @media (max-width: 768px){  /* 0~768 */
      body{
          background: yellow;
      }
  }
  @media (max-width: 425px){  /* 0~425 */
      body{
          background: orange;
      }
  }
  @media (max-width: 375px){  /* 0~375 */
      body{
          background: pink;
      }
  }
  @media (max-width: 320px){  /* 0~320 */
      body{
          background: green;
      }
  }
</style>
```
这里要注意一个优先级的问题，如果将最大宽度的顺序从小往大写，可能存在前面的样式被覆盖的问题，解决方法可以是上面那样将顺序颠倒，使得最大宽度的顺序从大往小；或者将查询条件写到很具体，使得条件之间没有交集
```
<style>
  @media (max-width: 320px){  /* 0~320 */
      body{
          background: yellow;
      }
  }
  @media (min-width: 321px) and (max-width: 375px){  /* 321~375 */
      body{
          background: orange;
      }
  }
  @media (min-width: 376px) and (max-width: 425px){  /* 376~425 */
      body{
          background: pink;
      }
  }
  @media (min-width: 426px) and (max-width: 768px){  /* 426~728 */
      body{
          background: green;
      }
  }
  @media (min-width: 769px){  /*769~正无穷*/
    body{
        background: purple;
    }
  }
</style>
```
## 动态rem
- - -
- 页面中，默认的字体大小(font-size)是16px，Chrome默认最小字体是12px
- rem指的是根元素的 font-size 大小（例如 <html> 元素的font-size）

```
html:
<body>
  <p>
    你好，我是xxx
  </p>
</body>

css:
p{
    font-size: 2rem;  /* 这里的2rem是指字体大小为32px，因为页面默认font-size为16px，1rem = 16px */
}
```

```
css:
body{
    font-size: 20px;
}
p{
    font-size: 2rem;  /* 这里的2rem仍然是32px，虽然设置了body的字体大小，但是rem指的是根元素html的字体大小，所以这里仍然是1rem = 16px的默认值 */
}
```

如果像下面这样设置了html的字体大小，那么 1rem 就是设置的字体大小
```
css:
html{
    font-size: 20px;
}
p{
    font-size: 2rem;  /* 这时p标签里的内容就是40px，因为设置了根元素的字体大小为20px，那么 1rem = 20px，而不是默认值16px了 */
}
```
### 适配手机的页面布局（百分比布局）
- - -
![百分比布局.jpg](https://i.loli.net/2018/07/03/5b3b8ecd37145.jpg)

这样的设计方法可以适配不同宽度的手机，但缺点是宽度和高度不能调试，因为宽度不是一个固定的值，所以高度也没办法确定
### 引入动态rem方案
- - -
- 它的主要思路是所有元素按比例放大或缩小（一切以宽度为基准），同时还解决了高度和宽度没有关联的问题
- 用JS来设置html的font-size等于页面宽度，而rem是依赖html的font-size的，进而使得rem间接依赖于页面宽度，可以这么理解 1 rem == html font-size == viewport width
- rem也可以与其他单位同时存在
```
 font-size: 16px;
 border: 1px solid red;
 width: 0.5rem;
```

下面以实现这样一个设计稿为例：

![设计稿.png](https://i.loli.net/2018/07/03/5b3b8ecdbd500.png)

用JS获取屏幕宽度做单位换算
```
<script>
  var pageWidth = window.innerWidth  //获取屏幕宽度
  document.write('<style> html{ font-size:' + pageWidth/10 + 'px;}</style>')  //用给定像素值除以屏幕宽度再乘以10得到的数字是以rem为单位
</script>
```
```
html:
  <div class="parent clearfix">
    <div class="child">40%</div>
    <div class="child">40%</div>
    <div class="child">40%</div>
    <div class="child">40%</div>
  </div>

css:
<style>
    *{
        margin: 0;
        padding: 0;
    }
    body{
        font-size: 16px;
    }
    div.child{
        float: left;
        background: #ddd;
        width: 4rem;
        height: 2rem;
        margin: .5rem .5rem;
    }
    .clearfix::before{
    content: '';
    display: block;
    clear: both;
    }
</style>
```
效果图：

![效果图.png](https://i.loli.net/2018/07/03/5b3b8ecb99473.png)

但是有个缺点就是每个元素的宽高都需要通过计算来得到，这样工作量会很大，解决方法是使用sass函数减少计算量

首先，使用命令行安装sass
- npm config set registry https://registry.npm.taobao.org/
- touch ~/.bashrc
- echo 'export
- SASS_BINARY_SITE="https://npm.taobao.org/mirrors/node-sass"' >> ~/.bashrc
- source ~/.bashrc
- npm i -g node-sass
- mkdir ~/Desktop/scss-demo
- cd ~/Desktop/scss-demo
- mkdir scss css
- touch scss/style.scss
- start scss/style.scss
- node-sass -wr scss -o css

然后，在sass文件中添加下面的代码
```
@function pxToRem( $px ){
  @return $px/$designWidth*10 + rem;
}  //这个函数可以将像素值转换成以rem为单位

$designWidth : 640; // 640是设计稿的宽度，根据实际要求修改即可

div.child{
  width: pxToRem(320);
  height: pxToRem(160);
  margin: pxToRem(40) pxToRem(40);
  border: 1px solid red;
  float: left;
  font-size: 1.2em;
}
```
通过sass函数的转译，我们可以直接使用px为单位，这样大大节省了计算时间
