---
title: vue-cli脚手架搭建项目
date: 2018-08-23 18:14:24
tags: vue-cli
---
项目搭建、目录结构分析及vue-router、Vuex的使用
<!-- more -->
## 项目搭建
1、在电脑上安装最新版的nodeJS，安装完成后安装淘宝镜像
```
npm install -g cnpm --registry=https://registry.npm.taobao.org
```

2、全局安装vue-cli
```
npm install -g vue-cli
```
// 如果已经安装过nodejs和vue-cli，可直接进行项目初始化

3、进入想要存放项目文件的目录下，然后初始化项目
```
vue init webpack my-project
```

4、进入项目文件
```
cd my-project
```

5、安装依赖
```
npm install
```

6、启动项目
```
npm run dev
```

打开http://localhost:8080，你会看到
![alt text](https://i.loli.net/2018/08/23/5b7e8b818bc8e.png)

## 目录结构
build  项目构建(webpack)相关代码
- build.js  // 生产环境构建代码
- check-version.js  // 检查node&npm等版本
- dev-client.js  // 热加载相关
- dev-server.js  // 构建本地服务器
- utils.js  // 构建配置共用工具
- vue-loader.conf.js  // vue加载器
-  webpack.base.conf.js  // webpack基础环境配置
- webpack.dev.conf.js  // webpack开发环境配置
- webpack.prod.conf.js  // webpack生产环境配置

config  项目开发环境配置相关代码
- dev.env.js  // 开发环境变量
- index.js  // 项目一些配置变量
- prod.env.js  // 生产环境变量

node_modules 项目依赖模块，npm install执行后初始化项目会将所有依赖放在这里

src  源码目录
- assets  // 资源目录 eg：logo.png
- components  // vue公共组件 eg：App.vue
- router  // 前端路由 eg：index.js路由配置文件
- App.vue  // 页面入口文件（根组件）
- main.js  // 程序入口文件（入口js文件）

static  静态文件（比如图片、json数据等）
- .gitkeep

其他
- .babelrc  // ES6语法编译配置
- .editorconfig  // 定义代码格式
- .gitignore  // git上传需要忽略的文件格式
- index.html  // 入口页面
- package.json  // 项目基本信息
- README.md  // 项目说明

## vue-router路由
1、安装
```
npm install --save vue-router
```

2、引用
```
import router from 'vue-router'
Vue.use(router)
```

3、配置路由文件，并在vue实例中注入
```
var rt = new router({
  routes: [  // 配置
    {
      path: '/hello', //指定要跳转的路径
      component: HelloWorld //指定要跳转的组件
    }
  ]
})

new Vue({
    el: '#app',
    router:router,  // 实例中注入
    components: {
        App
    },
    template: '<App/>'
})
```

4、确定视图加载位置
```
<router-view></router-view>
```

### 路由的跳转
<router-link to="/"></router-link>
```
<template>
    <ul>
        <li>
            <router-link to="/helloworld">HELLO WORLD</router-link>
        </li>
        <li>
            <router-link to="/helloearth">HELLO EARTH</router-link>
        </li>
    </ul>
</template>
```

### 路由参数的传递
1、必须在路由内加入路由的name

2、必须在path后加/: +传递的参数

```
<router-link :to="{ name: helloearth, params: {msg: 只有一个地球} }">HELLO WORLD</router-link>
读取参数： $route.params.XXX
方式：===/helloworld/你好世界

<router-link :to="{ path: '/helloearth', query: {msg: 只有一个地球} }">HELLO WORLD</router-link>
方式：===/helloworld?name=XX&count=xxx

函数模式：
可以创建一个函数返回 props，这样便可以将参数转换成另一种类型，将静态值与基于路由的值结合
const router = new VueRouter({
    routes: [
        {
            path: '/search',
            component: SearchUser,
            props: (route) => ({
                query: route.query.q
            })
        }
    ]
})
```
## Vuex的store状态管理
用来管理状态，共享数据，在各个组件之间管理外部状态，使用方法：
- 安装Vuex
```
npm install vuex
```

- 引入vuex，并通过use方法使用
```
import Vuex from 'vuex'
Vue.use(Vuex)
```

- 创建状态仓库
```
//  Store必须是大写，参数state不能改名字
var store = new Vue.Store({
    state: {
        xxx: yyy
    }
})
```

- 通过 this.$store.state.xxx 拿到全局状态

### Vuex下的mutations、actions和getters
vuex状态管理的流程：

view -> actions -> mutations –> state —­> view

mutations和actions的不同之处：
- 参数：mutations接收的参数是state，actions接收的是context
- 异步操作：mutations只能包含同步操作，而actions可以包含异步操作
- mutations直接变更状态（state），而actions提交的是mutation而非直接变更状态
- 调用方式：mutations通过this.$store.commit('')调用，actions通过this.$store.dispatch('')调用

注意：actions是可有可无的，没有actions的话，view直接走mutations中的逻辑，但是如果有异步操作，则必须使用actions

```
var store = new Vue.Store({
    state: {
        xxx: yyy
    },
    mutations: {  // 调用：this.$store.commit('xxx')参数为字符串格式
        aaa:function(state){
            // 一些操作
        }
    },
    actions: {  // 调用：this.$store.dispatch('xxx')
        bbb:function(context){
            context.commit('aaa');
        }
    },
    getters:{  // 调用：this.$store.getters.ccc
        ccc(state){
            return ... ...
        }
    }
})
```
