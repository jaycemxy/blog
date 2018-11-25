---
title: axios的使用
date: 2018-08-23 22:22:49
tags: axios
---
基于Vue搭建CNODE社区
<!-- more -->
## 什么是axios
axios 是一个基于 promise 的 HTTP 库，可以用在浏览器和 node.js 中，它本身具有以下特征：
- 从浏览器中创建XMLHttpRequest
- 从 nodejs 发出http请求
- 支持 Promise API
- 拦截请求和响应
- 转换请求和响应数据
- 取消请求
- 自动转换JSON数据
- 客户端支持防止 CSRF/XSRF

## 在Vue中使用
### get请求
1、安装
```
npm install axios
```

2、引入加载
```
import axios from 'axios'
```

3、将axios全局挂载到Vue原型上
```
Vue.prototype.$http = axios;  // 这样就可以直接使用this.$http发送请求
```

4、发送请求（以CNODE社区API为例）
```
使用传统的function：
getData(){
    var self = this;  // 传统的function需要将this赋值给一个新的变量
    this.$http.get('https://cnodejs.org/api/v1/topics')
        .then(function (res) {
            // 如果直接使用this.items = res.data.data 会出错，因为此处的this指向的不是当前vue实例
            self.items = res.data.data
            console.log(res.data.data)
        })
        .catch(function (err) {
            console.log(err)
        })
}

ES6箭头函数的写法
getData(){
    this.$http.get('https://cnodejs.org/api/v1/topics')
        .then(res =>{  // 可以直接使用this，因为箭头函会继承它父类的this
            this.items = res.data.data
            console.log(res)
        })
}
```

两种传递参数的形式：
```
推荐：（都是CNODE官方提供的API）
axios.get('url', {
    params: {
        page: 1,  // 页码
        limit: 10  // 每页显示的数量
    }
})

将参数直接写在地址后面：
axios.get('https://cnodejs.org/api/v1/topics?page=1&limit=15')
```

### post请求
POST传递数据有两种格式：
- form-data?page=1&limit=10
- x­www­form­urlencoded { page: 1,limit: 10 }

但因为在axios中post请求接收的参数必须是form-data的格式，所以需要用到一个qs插件来转换参数格式
```
首先 npm install qs
qs.stringify可以将格式转换成form-data的形式

postData(){
    this.$http.post('url',qs.stringify({
        page:1,
        limit:10
    })).then(res=>{
        this.items = res.data.data;
        console.log(res)
    })
}
```
