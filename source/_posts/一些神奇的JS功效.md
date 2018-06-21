---
title: 一些神奇的JS功效
tags:
  - JavaScript
categories: 前端
abbrlink: 63713
date: 2017-04-23 17:16:55
---

### 沉睡排序
```js
var numbers=[1,2,3,4,5,5,99,4,20,11,200];
numbers.forEach((num)=>{
   setTimeout(()=>{
       console.log(num)
   },num)
})
```

### 快速去重 (ES6)
```js
var arr = Array.from(new Set([1,2,3,4,4,3,5,6,7,8,8]));
```

### 单行写一个评级组件
```js
"★★★★★☆☆☆☆☆".slice(5 - rate, 10 - rate);
//定义一个变量rate是1到5的值，然后执行上面代码
```

### 论如何优雅的取整
```js
var a = ~~2.33

var b= 2.33 | 0

var c= 2.33 >> 0
```

### 短路表达式
```js
// 条件判断

var a = b && 1
// 相当于
if (b) {
    a = 1
} else {
    a = b
}

var a = b || 1
// 相当于
if (b) {
    a = b
} else {
    a = 1
}
```
