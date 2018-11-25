---
title: CRM法入门node.js
date: 2018-08-24 20:30:45
tags: nodejs
---
CRM套路入门node.js
<!-- more -->
## CRM套路
- C（copy）抄
- R（run）运行
- M（modify）修改
## fs 文件系统
### fs.readFile(path[, options], callback)异步读取文件
1、新建text.js文件，编辑text.js文件如下：（官方文档抄）
```
fs.readFile('/404', (err, data) => {  // 首先给出一个绝对不存在的文件/404
  if (err) throw err;
  console.log(data);
});
```

2、命令行输入
```
node text.js  // 运行文件会看到抛出错误
```
![alt text](https://i.loli.net/2018/08/24/5b7ffea7254cf.png)

3、解决方法是在文件顶部加入：
```
let fs = require('fs')  // 从node中加载该模块
```

4、此时重新运行，看到报错为没有该/404文件
![alt text](https://i.loli.net/2018/08/24/5b7ffea6e9735.png)

5、此时在桌面创建一个1.txt文件
```
let fs = require('fs')
fs.readFile('/Users/ringcrl/Desktop/1.txt', (err, data) => {  // 切记这里创建的文件不能写成~/Desktop/1.txt，命令行可以识别~，没说nodejs也可以！！！
  console.log(err);  // 这里将错误和数据都打印出来
  console.log(data);
});
```
![alt text](https://i.loli.net/2018/08/24/5b80008e4a363.png)

6、调用data.toString
```
let fs = require('fs')
fs.readFile('/Users/ringcrl/Desktop/1.txt', (err, data) => {
  console.log(err);
  console.log(data.toString);
});
```
修改1.txt中的内容为： 我是1.txt ， 运行可以看到打印出1.txt中的内容

### fs.readFileSync(path[, options])同步读取文件
1、官方文档抄代码
```
let fs = require('fs')

let data = fs.readFileSync('/Users/ringcrl/Desktop/1.txt')
console.log(data.toString());
```
可以看到同样打印出了1.txt中的内容
![alt text](https://i.loli.net/2018/08/24/5b80033684428.png)

## 创建一个http server
一般来说stackoverflow上都有可以直接拿过来跑的代码，注意搜索时的关键词eg：
```
stackoverflow nodejs set response -express  // -express的意思是除去带有express关键字的内容
```

text.js
```
let http = require('http')

let server = http.createServer(function(req,res){
    console.log(req.url)
    res.statusCode = 201
    res.write('hello');
    res.end()  // 不调用的话就一直在发送请求
})

server.listen(9999)  // 监听的端口号
console.log('9999')
```

