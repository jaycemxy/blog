---
title: ES6常用知识点
date: 2019-03-02 19:10:15
tags:
---
归纳ES6的常考问题
<!-- more -->
### var,let及const
我们都知道使用var声明的变量会被提升到作用域顶部，举个例子来看看三者的区别
```
var a = 1
let b = 1
const c = 1
console.log(window.b) // undefined
console.log(window. c) // undefined

function test(){
  console.log(a)
  let a
}
test()
```

在全局作用域下声明let和const不会被挂载到window上，再者如果我们在声明了a之前使用了a，会报错。报错的原因是存在暂时性死区，我们不能在声明前就使用一个变量。那么为什么会存在变量提升这个事情呢？其实它的存在是为了解决函数间互相调用的情况。
```
function test1() {
    test2()
}
function test2() {
    test1()
}
test1()
```
假如不存在提升这个情况，那么就实现不了上述的代码，因为不可能存在 test1 在 test2 前面然后 test2 又在 test1 前面。

总结几点：
1. 函数提升优于变量提升，函数提升会把整个函数挪到作用域顶部，变量提升只会把声明挪到作用域顶部；
2. var存在提升，我们可以在声明之前使用；let和const存在暂时性死区，不能在声明前使用；
3. var在全局作用域下声明变量会导致变量挂载到window上，其他两者不会；
4. let和const的作用基本一致，但const声明的变量不能被再次赋值；

### 原型继承&&ES6的Class继承
1. 原型继承
```
子类：
function Human(name) {
    this.name = name  // 自有属性
}
Human.prototype.run = function() {  // 原型上的共有属性
    console.log("我叫" + this.name + "，我在跑步")
    return undefined
}

孙类：
function Man(name) {
    Human.call(this.name)  // ①调用，即call一下父类
    this.gender = '男'  // 自有属性
}
Man.prototype.__proto__ = Human.prototype  // ②将Man的原型链到Human的原型上
Man.prototype.fight = function() {
    console.log('捶你胸口');
}
```

2. Class继承
```
class Human {
    constructor(name) {
        this.name = name  // 自有属性
    }
    run() {  // 公有属性，直接写在prototype即原型链上
        console.log("我叫" + this.name + "，我在跑步");
        return undefined
    }
}

class Man extends Human {  // 等价于 Man.prototype.__proto__ = Human.prototype
    constructor(name) {
        super(name)  // 调用父类，等价于Human.call(this,name)，使子类拥有this.name = name
        this.gender = '男'
    }
    fight() {
        console.log('捶你');
    }
}
```
class 实现继承的核心在于使用 extends 表明继承自哪个父类，并且在子类构造函数中必须调用 super，因为这段代码可以看成 Parent.call(this, value)，但JS中并不存在类，所以 class 的本质就是函数

### 模块化（待补充）

### proxy
在Vue3.0中，将通过proxy替代原本的Object.defineProperty来实现数据响应式，Proxy是ES6中的新增功能，它可以用来自定义对象中的操作
```
let p = new Proxy(target,handler)  // target 代表需要添加代理的对象，handler 用来自定义对象中的操作，比如可以用来自定义 set 或者 get 函数。
```

下面通过Proxy来实现一个数据响应式
```
let onWatch = (obj, setBind, getLogger) => {
  let handler = {
    get(target, property, receiver) {
      getLogger(target, property)
      return Reflect.get(target, property, receiver)
    },
    set(target, property, value, receiver) {
      setBind(value, property)
      return Reflect.set(target, property, value)
    }
  }
  return new Proxy(obj, handler)
}

let obj = { a: 1 }
let p = onWatch(
  obj,
  (v, property) => {
    console.log(`监听到属性${property}改变为${v}`)
  },
  (target, property) => {
    console.log(`'${property}' = ${target[property]}`)
  }
)
p.a = 2 // 监听到属性a改变
p.a // 'a' = 2
```
我们通过自定义 set 和 get 函数的方式，在原本的逻辑中插入了我们的函数逻辑，实现了在对对象任何属性进行读写时发出通知。

如果需要实现一个 Vue 中的响应式，需要在 get 中收集依赖，set 派发更新，之所以要使用 Proxy 替换原本的 API 原因在于 Proxy 无需一层层递归为每个属性添加代理，一次即可完成以上操作，性能上更好，并且原本的实现有一些数据更新不能监听到，但是 Proxy 可以完美监听到任何方式的数据改变，唯一缺陷是浏览器兼容不好。

### map、filter、reduce
1. map 作用是生成一个新数组，遍历原数组，将每个元素拿出来做一些变化然后放入到新的数组中。
```
eg:
[1, 2, 3].map(v => v + 1) // -> [2, 3, 4]
```

实现原理
```
Array.prototype.map = function(fn){
    let result = [];
    for(let i=0; i < this.length; i++) {
        if(i in this) {
            result[i] = fn.call(undefined,this[i],i,this)
        }
    }
    return result
}
```

2. filter 有条件地放入map，是真值就push进去，不是真值就不push
```
eg:
a = [1,2,3,4,5]
a.filter( (value) => {return value % 2 === 0})  // [2,4]
a.filter( (value) => {return value % 2 !== 0})  // [1,3,5]
```

实现原理
```
Array.prototype.filter = function(fn){
    let result = [];
    let temp;
    for(let i = 0; i<this.length; i++) {
        if(i in this) {
            if(temp = fn.call(undefined,this[i],i,this)){
                result.push(temp)
            }
        }
    }
    return result
}
```

3. reduce 可以将数组中的元素通过回调函数最终转换为一个值
```
eg:
如果我们想实现一个功能将函数里的元素全部相加得到一个值，可能会这样写代码：
const arr = [1, 2, 3]
let total = 0
for (let i = 0; i < arr.length; i++) {
  total += arr[i]
}
console.log(total) //6 

如果用reduce可以一行实现：
const arr = [1, 2, 3]
const sum = arr.reduce((acc, current) => acc + current, 0)
console.log(sum)
```
reduce 接受两个参数，分别是回调函数和初始值

- 首先初始值为 0，该值会在执行第一次回调函数时作为第一个参数传入；
- 回调函数接受四个参数，分别为累计值、当前元素、当前索引、原数组；
- 在一次执行回调函数时，当前值和初始值相加得出结果 1，该结果会在第二次执行回调函数时当做第一个参数传入；
- 所以在第二次执行回调函数时，相加的值就分别是 1 和 2，以此类推，循环结束后得到结果 6；

实现原理：
```
Array.prototype.reduce = function(fn,init) {
    let result = init;
    for(let i = 0; i < this.length; i++){
        if(i in this) {
            result = fn.call(undefined,result,this[i],i,this)
        }
    }
    return result
}
```

4. 用reduce实现map和filter
reduce -> map
```
array2 = array.map( (v) => v+1)  // 给数组每一项加1

可以写成：
array2 = array.reduce( (result,v) => {
    result.push(v+1);
    return result
}, [])
```

reduce  -> filter
```
array2 = array.filter( (v) => v%2 === 0)

可以写成：
array2 = array.reduce( (result,v) => {
    if( v%2 === 0) {
        result.push(v)
    }
    return result
}, [])
```