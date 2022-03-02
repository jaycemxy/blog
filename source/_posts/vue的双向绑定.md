---
title: vue的双向绑定
date: 2018-08-11 22:35:45
tags: vue
---
MVVM模式的体现
<!-- more -->
## vue双向绑定
### 原理（采用Object.defineProperty()数据劫持 + 发布订阅模式）
通过ES5提供的Object.defineProperty()方法，监控对数据的操作（劫持各个属性的setter、getter，在数据变动时发布消息给订阅者，触发相应的监听回调），从而自动触发数据同步。并且由于是在不同的数据上触发同步，可以精确地将变更发送给绑定的视图，而不是对所有数据都进行一次检测。简单应用：
```
var obj = {};
Object.defineProperty(obj, 'hello', {
    get: function() {
        console.log('get方法获取值');
    },
    set: function(val) {
        console.log('set方法设置的值为：' + val);
    }
});

obj.hello; // get方法获取值
obj.hello = 'Hello World';
```
Object.defineProperty()函数接受三个参数，且都是必须的：
- 第一个参数：目标对象
- 第二个参数：需要定义的属性或方法的名字。
- 第三个参数：目标属性所拥有的特性。

### 具体步骤：
- 需要observe的数据对象进行递归遍历，包括子属性对象的属性，都加上 setter和getter这样的话，给这个对象的某个值赋值，就会触发setter，那么就能监听到了数据变化
- compile解析模板指令，将模板中的变量替换成数据，然后初始化渲染页面视图，并将每个指令对应的节点绑定更新函数，添加监听数据的订阅者，一旦数据有变动，收到通知，更新视图
- Watcher订阅者是Observer和Compile之间通信的桥梁，主要做的事情是:

1、在自身实例化时往属性订阅器(dep)里面添加自己

2、自身必须有一个update()方法

3、待属性变动dep.notice()通知时，能调用自身的update()方法，并触发Compile中绑定的回调，则功成身退。
- MVVM作为数据绑定的入口，整合Observer、Compile和Watcher三者，通过Observer来监听自己的model数据变化，通过Compile来解析编译模板指令，最终利用Watcher搭起Observer和Compile之间的通信桥梁，达到数据变化 -> 视图更新；视图交互变化(input) -> 数据model变更的双向绑定效果。

### 如何使用
假如我们要实现一个用户在输入框输入内容，页面上会自动更新的效果
```
html:
<div id="app">
  <p>{{ message }}</p>
  <input v-model="message"></input>
</div>

JS:
var vm = new Vue({
  el:'app',
  data: {
    message:'Hello,this is Jayce!'
  }
})
```

v-model只是一个语法糖
```
<input v-model="x">
```

相当于
```
<input v-bind:value="x" @input="x = $event">
```
原理是绑定了一个input事件，将x的值传递给子组件，如果子组件的值更新了，它会发出一个input事件，input会传来x最新的值，再将这个最新的值event赋值给x，从而实现双向绑定

### 优缺点
双向绑定给人最大的优越感就是方便，当data发生变化时，页面也会自动更新，但这也伴随着一个缺点，我们无从知道data是什么时候变的，是谁变的，变化后也没有人来通知你，虽然说watch可以用来监听data的变化，但这样岂不是变得更复杂，不如用单向数据绑定，Vuex的单向数据绑定就满足了这种控制欲，虽然牺牲了一部分便捷性，但是换来的却是更强的控制力。
