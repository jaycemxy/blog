---
title: HTML难点标签
date: 2022-03-03 20:01:01
tags:
---
打开html文件的方法
```
http-server -c-1 或简写为 hs -c-1
parcel index.html
```
## a标签
### href属性
1. 网址
```
三种写法（推荐第三种）
http://baidu.com
https://baidu.com
//baidu.com  可自动选择用http或https协议 
```
2. 路径
```
/a/b/c 或 a/b/c
index.html 或 ./index.html
```
3. 其他用法
```
<a href="javascript:;">点击后什么也不发生</a>

<a href="#ddd">找到id为ddd的标签</a>

<a href="mailto:jayce_ma.xa@foxmail.com">给某人发邮件</a>

<a href="tel="18912345678">打电话给某人</a>
```
### target属性
1. 内置名称
```
_blank 在新窗口打开
_top 在最顶层打开
_parent 在当前页面的上一层打开
_self 在当前页面加载
```
2. 给窗口命名（作用是可重复利用窗口）
```
<a href="//baidu.com" target="sss">baidu</a>
<a href="//bilibili.com" target="sss">b站</a>

若有window.name=sss的窗口，就用它打开，若没有就创建一个
```
### 与iframe一起的用法(在网页中内嵌窗口)
```
<a href="//bing.com" target="xxx">必应</a>
<a href="//bilibili.com" target="xxx">b站</a>

<hr/>

<iframe style="border:none; width:100%; height:500px;" name="xxx"></iframe>
```
***
## table标签
```html
<table>
    <thead>
        <th>英文</th>
        <th>中文</th>
    </thead>
    <tbody>
        <tr>
            <td>target</td>
            <td>目标</td>
        </tr>
        <tr>
            <td>document</td>
            <td>文件</td>
        </tr>
        <tr>
            <td>device</td>
            <td>设备</td>
        </tr>
    </tbody>
    <tfoot>
        <td>空</td>
        <td>空</td>
    </tfoot>
</table>
```
![WechatIMG2311.png](https://s2.loli.net/2022/03/03/1nixO6kIXC9Z2Jh.png)

```html
<table>
    <thead>
        <th></th>
        <th>Petter</th>
        <th>Tom</th>
        <th>Micky</th>
    </thead>
    <tbody>
        <tr>
            <th>语文</th>
            <td>44</td>
            <td>44</td>
            <td>44</td>
        </tr>
        <tr>
            <th>数学</th>
            <td>55</td>
            <td>55</td>
            <td>55</td>
        </tr>
        <tr>
            <th>英语</th>
            <td>66</td>
            <td>66</td>
            <td>66</td>
        </tr>
    </tbody>
    <tfoot>
        <tr>
            <th>总分</th>
            <td>166</td>
            <td>166</td>
            <td>166</td>
        </tr>
        <tr>
            <th>排名</th>
            <td>123</td>
            <td>123</td>
            <td>123</td>
        </tr>
    </tfoot>
</table>
```

```css
<style>
    table { 
        width:500px;
        height:100px;
        table-layout:auto;  //自动调整表格布局，表头信息多就宽一些
        border-collapse: collapse;  
        border-spacing: 0;  //修改border默认样式，即合并border并使border间距为0
    }
    tr,td,th {
        border:2px solid pink;
    }
</style>
```
![WX20220303-211724@2x.png](https://s2.loli.net/2022/03/03/4TvReQAsSqaHnEp.png)
***
## img标签
作用：发出get请求，展示一张图片
```html
<img id="ccc" src="/brown-cat.jpeg" alt="棕色小猫" width="800">  //width和height是img标签的属性，！！切记只需设置一项，另外一个会自适应

<script>
    ccc.onload = function(){
        console.log("恭喜加载成功啦~")
    }   //监听图片是否加载成功

    ccc.onerror = function(){
        console.log("抱歉图片不见啦~");
        ccc.src= "/404.png";
    }  //监听图片是否加载失败
</script>
```

```css
<style>
    * {
        margin: 0;
        padding: 0;
        box-sizing: border-box;  //重置默认样式
    }
    img {
        max-width: 100%;  //自适应宽度
    }
</style>
```
***
## form标签
作用：发出一个get或post请求，然后刷新页面
```html
<form action="/yyy" method="POST" autocomplete>  
    //action用于控制请求哪个页面，method控制是POST还是GET
    //autocomplete的用法：在form标签上加autocomplete，在input标签上加name=""
    <input name="username" type="text" />
    <input type="submit" value="" />  //默认是提交，给value赋值改变默认
</form>  //form标签里必须含有一个type=submit的标签，这样表单才能提交
```

***
## input标签
- input与button标签的区别
```html
<input type="submit" value="搞起" />  //input标签里只能是纯文本
<button type="submit"><strong>搞起</strong></button>  //button标签里还可以有其他标签
```
- input标签里type属性的取值
```html
<input type="text" required />  //required意为必须填写，否则不能提交

<input type="radio" name="gender" />男  //单选
<input type="radio" name="gender" />女

<input type="checkbox" name="hobby" />养猫  //多选
<input type="checkbox" name="hobby" />听歌
<input type="checkbox" name="hobby" />写代码

<input type="file" />  //选择单个文件
<input type="file" multiple />  //选择多个文件
```
***
## 其他输入标签
```html
<textarea style="resize:none; width:50%; height:300px;"></textarea>

<select>
    <option value="">- 请选择 -</option>
    <option value="1">星期一</option>
    <option value="2">星期二</option>
</select>
```