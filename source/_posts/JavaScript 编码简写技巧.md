---
title: JavaScript 编码简写技巧
abbrlink: 56432
date: 2018-07-29 01:07:34
tags:
  - JavaScript
categories: 前端
img: archives/56432/javascript.jpg 
---

![javascript](56432/javascript.jpg)
### for 循环简写
普通写法：
```javascript
for (let i = 0; i < allImgs.length; i++){

}
```

简写：
```javascript
for (let index of allImgs){

}
```

### 对象属性简写
在 JavaScript 中定义对象字面量非常容易。ES6 提供了一种更简单的方法来将属性分配给对象。如果 name 和 key 名字相同，则可以利用简写符号。

普通写法：
```javascript
const obj = { x:x, y:y }
```

简写：
```javascript
const obj = { x, y }
```

### 隐式返回简写
我们经常使用 return 关键字来返回一个函数的最终结果。使用单条语句的箭头函数将隐式返回求值结果（该函数必须省略大括号（{}）才能省略 return 关键字）。

要返回多行表达式（如对象字面量），那么需要用 () 而不是 {} 来包裹你的函数体。这样可以确保代码作为一个单独的表达式被求值返回。

普通写法：
```javascript
function calcCircumference(diameter) {
  return Math.PI * diameter
}
```

简写：
```javascript
calcCircumference = diameter => (
  Math.PI * diameter;
)
```

### 解构赋值简写
如果你正在使用任何流行的 web 框架，那么你很有可能会使用数组或者对象字面量形式的数据在组件和 API 之间传递信息。一旦组件接收到数据对象，你就需要将其展开。

普通写法：
```javascript
const observable = require('mobx/observable')
const action = require('mobx/action')
const runInAction = require('mobx/runInAction')
 
const store = this.props.store
const table = this.props.table
const loading = this.props.loading
const errors = this.props.errors
const entity = this.props.entity
```

简写：
```javascript
import { observable, action, runInAction } from 'mobx'

const { store, table, loading, errors, entity } = this.props
```

你甚至可以给变量重新分配变量名：
```javascript
// const contact = this.props.entity
const { store, table, loading, errors, entity:contact } = this.props
```

### 双位非运算符简写
位操作符是你在刚学习 JavaScript 时会学到的一个特性，但是如果你不处理二进制的话，基本上是从来都不会用上的。

但是，双位运算符有一个非常实用的使用场景：可以用来代替 Math.floor。双位非运算符的优势在于它执行相同操作的速度更快。

普通写法：
```JavaScript
Math.floor(4.9) === 4  //true
```

简写：
```JavaScript
~~4.9 === 4  //true
```

### Array.find 简写
如果你曾经使用原生 JavaScript 写一个查找函数，你可能会使用 for 循环。在 ES6 中，你可以使用名为`find()`的新数组函数。

普通写法：
```JavaScript
const pets = [
  { type: 'Dog', name: 'Max'},
  { type: 'Cat', name: 'Karl'},
  { type: 'Dog', name: 'Tommy'},
]
 
function findDog(name) {
  for(let i = 0; i<pets .length; ++i) {
    if(pets[i].type === 'Dog' && pets[i].name === 'Tommy') {
      return pets[i];
    }
  }
}
```

简写：
```JavaScript
pet = pets.find(pet => 
  pet.type ==='Dog' && pet.name === 'Tommy'
)
console.log(pet)   // { type: 'Dog', name: 'Tommy' }
```
