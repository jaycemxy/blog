---
title: vue常见知识点
date: 2018-08-11 20:57:10
tags: vue
---
梳理知识点
<!-- more -->
## vue框架与jQuery的区别
jQuery是使用选择器（$）选取DOM对象，对其进行赋值、取值、事件绑定等操作，和原生的HTML的区别只在于可以更方便的选取和操作DOM对象，而数据和界面是在一起的。比如需要获取label标签的内容：$("lable").val(); 它还是依赖DOM元素的值。

Vue使用MVVM模式，通过数据来显示视图层而不是节点操作，主要业务逻辑都放在viewModel中，视图和数据不直接进行数据交流
## 一个组件实例
```
Vue.component('定义一个组件名,组件名最好是带小横线的写法，尽量不要使用驼峰命名法button-counter', {
    data: function() {  // data必须返回一个函数
        return {
            count: 0
        }
    },
    template: `  // 模版下只能有一个根元素
    <button @click="count++">You clicked me {{ count }}times.</button>
    `
})
```

将这个组件作为自定义元素使用
```
html:
<div id="components-demo">
  <button-counter></button-counter>
</div>

JS:
var vm = new Vue({
  el: '#components-demo'
})
```
## 实例生命周期钩子
1、什么是生命周期

每个Vue实例从创建到销毁的过程就是生命周期，它的经历包括开始创建、初始化数据、编译模版、挂载到DOM、渲染更新、卸载。可以分为10个阶段：创建前/后（create），挂载前/后（mount）、更新前/后（update）、启动前/后（active）、销毁前/后（destroy）

2、生命周期函数的作用

生命周期中有许多事件钩子，让我们在控制整个Vue实例的过程中更容易形成好的逻辑
* created 实例创建完成后调用，此阶段完成了数据的观测等，但尚未挂载， $el 还不可用。需要初始化处理一些数据时会比较有用 ----还未挂载
* mounted el 挂载到实例上后调用，一般我们的第一个业务逻辑会在这里开始 。相当于 $(document).ready() ---刚刚挂载
* beforeDestroy 实例销毁之前调用。主要解绑一些使用 addEventListener 监听的事件等。

3、第一次页面加载会触发哪几个钩子？DOM渲染在哪个周期就完成了？

第一次页面加载会触发beforeCreate、created、beforeMount、mounted这四个钩子

DOM渲染在mounted中就已完成
## computed计算属性的用法？与methods的区别
官方解释：对于复杂的逻辑我们应该用计算属性，例如想多次引用一个翻转字符
```
html:
<div id="demo">
  <p>Original message: "{{ message }}"</p>
  <p>Reversed message: "{{ reversedMessage }}"</p>
</div>

JS:
var vm = new Vue({
  el: 'demo',
  data: {
    message: 'Hello'
  },
  computed: {
    reversedMessage: function(){
      return this.message.split('').reverse().join('')
    }
  }
})
```

与methods的区别：
- 两种方式的最终结果是一致的，但唯一不同的是计算属性是基于它们的依赖进行缓存的，也就是说只有在他的相关依赖发生改变时才会重新求值，而调用方法总是会在触发重新渲染时再次执行函数
- 当有一个性能开销巨大的项目时，它需要计算属性A，它需要遍历一个巨大的数组并且进行大量的运算，也有可能一些其他的计算属性依赖于属性A，假如没有缓存，每次调用属性都会重新对A进行一次计算，如果不希望有这些缓存，那么就可以用methods来代替
## v-if与v-show
1、区别

- 手段：v-if是动态的向DOM树内添加或者删除DOM元素；v-show是通过设置DOM元素的display样式属性控制显隐；
- 编译条件：v-if是惰性的，如果初始条件为假，则什么也不做，直到条件第一次变为真时，才会开始渲染条件块; v-show不管初始条件是什么，元素总是会被渲染，然后被缓存，而且DOM元素保留；
- 编译过程：v-if切换有一个局部编译/卸载的过程，它会在切换过程中合适地销毁和重建内部的事件监听和子组件；v-show只是简单的基于css切换；
- 性能消耗：v-if有更高的切换消耗；v-show有更高的初始渲染消耗；

2、使用场景

v-if适合运行条件不大可能会改变；v-show适合频繁切换
- 对于管理系统的权限列表的展示，可以使用v-if来渲染，如果用到v-show，对于用户没有的权限，在网页的源码中，仍然能够显示出该权限，如果用v-if，网页的源码中就不会显示出该权限。（在前后台分离情况下，后台不负责渲染页面的场景。）
- 对于前台页面的数据展示，推荐使用v-show，这样可以减少开发中不必要的麻烦。

3、总结

两者都是用来控制元素的渲染。v-if会判断是否需要加载，只在需要时加载可减轻服务器压力，但具有更高的切换开销；v-show通过改变DOM元素的CSS的display属性，可以使用户操作更加流畅，但有更高的初始渲染开销。如果需要频繁切换状态就用v-show，若运行条件很少会改变，用v-if较好

## Vuex
1、官方定义：专门为Vue.js应用程序开发的状态管理模式。它采用集中式存储管理应用的所有组件的状态，并以相应的规则保证状态以一种可预测的方式发生变化。

2、Vuex解决了什么问题
- 组件之间的数据通信
- 使用单向数据流的方式进行数据的中心化管理（所谓的单向数据流，就是当用户进行操作的时候，会从组件发出一个 action，这个 action 流到 store 里面，触发 store 对状态进行改动，然后 store 又触发组件基于新的状态重新渲染）对于复杂的应用来说，Vuex实施统一管理，方便维护和跟踪

3、举例说明Vuex的好处（反证法）

我们不好说为什么使用Vuex，但是如果在下面这种情况下不使用Vuex，将会带来很多弊端

假设在一个app里有四个tab，每个tab都需要获取用户的资料，如果数据在每个tab组件里都保存了一份，那么用户在手动更新了资料后，这四个tab都需要更新一遍用户资料来保证用户在每个地方看到的数据永远都是最新的。如果说每进一次tab都重新请求一下：

对于服务器来说，频繁请求数据会耗用很多资源，如果该app的用户数量足够多，那么每多出来的一次请求，对于公司来说都是一笔巨大的开支，但是如果数据都储存在store中，并且这四个tab读取的都是同一份数据，那么在用户更新了资料时，只需要更新store中的数据，这样在进这四个不同的tab时，就减少了四次请求

## vue-router
1、什么是路由

根据路径选择不同的页面展示给用户（所有东西都是页面，也可以说是一个个组件，用路由在之间来回切换）

2、为什么使用路由

一般来说，每次请求一个地址都会发送给服务器进行处理，但是有些用户操作不需要请求服务器，直接在页面下修改逻辑就能达到目的，这种时候用路由就可以了。

3、前端路由是什么？如何实现

前端路由是找到与地址相匹配的组件并将它渲染出来，本质是：改变浏览器地址（更新视图）但不向服务器发出请求，有两种方法可以做到：
- hash模式 利用URL中的hash（“#”）
- history模式 利用 history.pushState API 来完成URL跳转而无须重新加载页面

两种模式的对比：
- Hash 模式只可以更改 # 后面的内容，History 模式可以通过 API 设置任意的同源 URL
- History 模式可以通过 API 添加任意类型的数据到历史记录中，Hash 模式只能更改哈希值，也就是字符串
- Hash 模式无需后端配置且兼容性好。History 模式在用户手动输入地址或者刷新页面的时候会发起 URL 请求，后端需要配置 index.html 页面用于匹配不到静态资源的时候

4、vue-router实现步骤
<1> 在首页中添加两个script标签导入vue和vue-router
```
<script src="js/vue.js"></script>
<script src="js/vue-router.js"></script>

<div id="app">
  <div>
    <!-- 用 router-link 组件来导航、传入 to 属性来指定链接、<router-link> 默认会被渲染成一个 a 标签 -->
    <router-link to="/">Go to home</router-link>
    <router-link to="/topic">Go to topic</router-link>
    <router-link to="/content">Go to content</router-link>
  </div>

  <!-- 路由出口，路由匹配到的组件将在这里被渲染出来 -->
  <router-view></router-view>
</div>
```

<2> 定义vue组件
```
const home = { template: '<div>this is home page</div>' }
const topic = { template: '<div>this is topic page</div>' }
const content = { template: '<div>this is content page</div>' }
```

<3> 定义路由
```
const routes = [
  { path:'/', component: home},
  { path:'/topic', component: topic},
  { path:'/content', component: content},
]
```

<4> 创建router实例，并将定义的路由传入 const router = new VueRouter({ routes: routes })

<5> 创建和挂载根实例
```
const app = new Vue({
  router: router
}).$mount('#app')
```
