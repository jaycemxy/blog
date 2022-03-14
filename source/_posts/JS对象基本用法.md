---
title: JS对象基本用法
date: 2022-03-14 14:21:17
tags:
---
## 如何声明对象
```js
let obj = { 'name': 'frank', 'age': 18}

let obj = new Object({ 'name':'frank', 'age': 18})
```
1. 键名是字符串，不是标识符，可以包含任意字符
2. 引号可以省略，省略之后就只能写标识符
3. 就算引号省略，键名key永远都是字符串

## 删除对象的属性
```js
delete obj['name']  // 新手推荐写法

delete obj.name
```

注意区分'不含属性名'和'属性值为undefined'
```js
var obj = {}  // 不含属性xxx
var obj2 = {'xxx': undefined}  // 含属性xxx，但值为undefined

obj.xxx === undefined  // true
obj2.xxx === undefined  // true

'xxx' in obj  // false
'xxx' in obj2  // true

obj.xxx === undefined 不能断定 'xxx' 是否为obj的属性

应该使用'xxx' in obj
```

## 查看对象属性
```js
// 查看自身属性

Object.keys(obj)  // 自身属性
Object.values(obj)  // 自身属性值
Object.entries(obj)  // 查看所有属性及属性值，返回键值对数组
```

```js
// 查看自身属性+共有属性

console.dir(obj)
```

```js
// 判断一个属性是否是自身属性
var obj = { 'name': 'joyce', 'age': 25, 'gender': 'women'}

obj.hasOwnProperty('toString')  // false
obj.hasOwnProperty('name')  // true


// 查看'name'属性是否在obj中，但不能断定是obj的自身属性还是共有属性
'name' in obj // true
```

```js
// 查看obj的一个属性

obj.name
obj['name']  // 两种写法均可，新手推荐第二种，键名是字符串而不是变量，新手容易与obj[name]混淆
```

## 修改或增加对象属性
- 修改或增加自身属性
```js
// 单个赋值
let obj = {'name': 'joyce'}
obj.age = 25
obj['hobby'] = cooking

// 批量赋值
Object.assign(obj, {'age': 25, 'hobby': 'cooking'})
```

- 修改或增加共有属性(原型)
```js
var common = {'nation':'China', 'hair':'brown'}

var person = Object.create(common)  // 以common为原型创建person
Object.assign(person, {  // 批量给person增加属性
    'name' :'joyce',
    'age' : 25,
    'hobby' : 'cooking',
})
```

- 修改或增加共有属性（原型）的值
```js
obj.__proto__.toString = 'yyy'

Object.prototype.toString = 'yyy'
```
