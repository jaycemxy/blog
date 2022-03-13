---
title: JS基本语法
date: 2022-03-13 16:47:15
tags:
---
## 表达式与语句
- 表达式（expression），指一个为了得到返回值的计算式
1. 1+2表达式的值为3
2. add(1,2)表达式的值为函数的返回值
3. console.log表达式的值为函数本身
4. console.log(3)表达式的值为undefined，3是控制台打印出来的

- 语句（statement）是为了完成某种任务而进行的操作，下面是两行赋值语句。语句以分号结尾，一个分号就表示一个语句结束，多个语句可以写在一行内。
1. var a = 1; 
2. var b = 'abc';

- 语句和表达式的区别
1. 语句主要为了进行某种操作，一般情况下不需要返回值
2. 表达式是为了得到返回值，一定会返回一个值
3. 语句一般会改变环境（声明、赋值）
***
## 大小写敏感
- var a 与 var A 是不同的
- object 与 Object 是不同的
***
## 空格
- 大部分空格没有实际意义
- 加回车大部分时候也不影响，只有在**return**后面不能加回车

![WechatIMG2334.png](https://s2.loli.net/2022/03/13/Q5X7VzRblZKP61f.png)
***
## 标识符
- 第一个字符，可以是unicode字母 $ _ 中文（但不可以以数字开头）
- 变量名是标识符
```js
var _ = 1
var $ = 2
var _____ = 6
var 你好 = ‘hi’
```
***
## 区块block
将代码包裹在一起，且常与if/for/while合用
```js
{
    let a = 1
    let b = 2
}
```
***
## if语句
- 语法
1. if(表达式) {语句1}else{语句2}
2. {}在语句只有一句的时候可以省略，但不建议这样做
- 变态情况
1. 表达式里可以很变态，如 a = 1
2. 语句1里可以非常变态，如嵌套if else
3. 语句2里可以非常变态，如嵌套 if else
4. 缩进也可以很变态，如面试题常常下套
```js
let a = 2
if(a = 1){ // a = 1，指将1赋值给a 
    console.log('a是1')
}else{
    console.log('a不是1')
    // 会打印出 a是1
}

let a = 1
if(a === 2)
    console.log('a')
    console.log('a等于2')
    // 会打印出 a等于2

let a = 1
if(a === 2)
    console.log('a'); console.log('b')
    // 会打印出b

let a = 1
if(a === 2)
    console.log('a'),console.log('b')
    // 什么也不打印
```

推荐写法
```js
if(表达式) {
    语句1
} else if(表达式){
    语句2
} else {
    语句3
}
```

次推荐写法
```js
function fn() {
    if(表达式) {
        return 表达式
    }
    if(表达式) {
        return 表达式
    }
    return 表达式
}
```
***
## switch语句
```js
switch (fruit){
    case "banana":
        执行语句1;
        break;
    case:"apple":
        执行语句2;
        break;
    default:
        执行语句3;
}

// 根据变量fruit的值，选择执行相应的case。如果所有case都不符合，则执行最后的default部分
// 不要省略break，否则不会退出switch，并且会继续执行下一个case
```
***
## 问号冒号表达式
语法：条件? 表达式1:表达式2
```js
var even = (n % 2 === 0) ? true : false;
```
等同于
```js
var even;
if(n % 2 === 0) {
    even = true;
} else {
    even = false;
}
```
可视为if...else...的简写形式
```js
var msg = '数字' + n + '是' + (n%2 === 0 ? '偶数' : '奇数');
```
***
## &&短路逻辑
```js
window.f1 && console.log('f1存在');

等价于

if(window.f1){
    console.log('f1存在');
} // undefined
```

```js
fn && fn() // 表示fn若存在，就调用fn()

等价于

if (fn) {
    fn();
}

```
- 'A && B': 若A为真，就执行B，若A为假，就不执行后面的
- 'A && B && C && D': 取第一个假值或D
***
## ||短路逻辑
```js 
a || b

等价于

if( !a ){
    b
} else{

}
```
**常常用于给a设定一个兜底值**
```js
a = a || 100

等价于

if(a){
    a = a
}else if{
    a = 100
}

等价于

if(!a) {  // 若a不存在，就给a赋值为100
    a = 100
}
```
***
## 循环语句
### while循环
语法： while(条件){语句};

包括一个循环条件和一段代码块，只要条件为真，就不断循环执行代码块。
```js
var i = 1;
while (i<10) {
    console.log('i当前为' + i);
    i = i+1;
}
```
```js
let a = 0.1
while(a ! == 1){
    console.log(a)
    a = a + 0.1
}  //  由于浮点数的问题，会死循环
```

### for循环
for语句是循环命令的另一种形式（while循环的方便写法），可以指定循环的起点、终点和终止条件。
```js
for (let i = 0; i < 5; i++) {
    console.log(i);
} // 打印出 1 2 3 4 5


for(var i = 0; i < 10; i++) {
    for (var j=101; j<100; j++){
        if(i===5) {
            break;
        }
    }
    console.log(i);
}  // 打印出0，1,2,3,4,5,6,7,8,9  break只会退出距离它最近的for循环，外面的for循环中的i会执行完
```
```js
for (var i = 0; i < 5 ;i++){
    setTimeout(()=> {
        console.log(i);
    },0)
}
// setTimeout是过一段时间执行，结果打印出5个5，在for循环结束后i的值为5，setTimeout作为循环体，会等到for循环执行完后，才会打印
```
```js
for(let i = 0; i < 5; i++){
    setTimeout(()=>{
        console.log(i)
    },0)
}
// 将var换成let，就会打印出0,1,2,3,4
```

### break和continue语句
两者都具有跳转作用，可以让代码不按既有的顺序执行。

break 是跳出离它最近的一个循环。
```js
for (var i = 0; i < 5; i++) {
  console.log(i);
  if (i === 3)
    break;
}  // 执行到i等于3，就会跳出循环。 打印出0,1,2,3
```

continue 会立即跳出当前的一次循环，返回循环结构的头部，开始下一轮循环。
```js
var i = 0;

while (i < 100){
  i++;
  if (i % 2 === 0) continue;
  console.log('i 当前为：' + i);
}  // 打印出i为奇数的值，i为偶数时，直接进入下一轮循环
```

### label标签
```js
{
    a:1;
}
```
一个代码块里有一个label，a是一个label，语句是1***它并不是一个对象**

label标签通常与 break语句 和 continue语句 配合使用，跳出特定循环

```js
foo: {
    console.log(1);
    break foo;
    console.log('本行不会输出');
}
console.log(2);  // 当执行到break foo就会跳出代码块，打印出1,2
```
