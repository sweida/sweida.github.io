---
title: 三张图搞懂JavaScript的原型对象与原型链
categories: 前端
tags:
- JavaScript
abbrlink: 16268
date: 2018-06-15 11:31:41
---

对于新人来说，JavaScript的原型是一个很让人头疼的事情，一来prototype容易与__proto__混淆，二来它们之间的各种指向实在有些复杂，其实市面上已经有非常多的文章在尝试说清楚，有一张所谓很经典的图，上面画了各种线条，一会连接这个一会连接那个，说实话我自己看得就非常头晕，更谈不上完全理解了。所以我自己也想尝试一下，看看能不能把原型中的重要知识点拆分出来，用最简单的图表形式说清楚。

我们知道原型是一个对象，其他对象可以通过它实现属性继承。但是尼玛除了prototype，又有一个__proto__是用来干嘛的？长那么像，让人怎么区分呢？它们都指向谁，那么混乱怎么记啊？原型链又是什么鬼？相信不少初学者甚至有一定经验的老鸟都不一定能完全说清楚，下面用三张简单的图，配合一些示例代码来理解一下。
![787416-20160323103557261-114570044](https://user-images.githubusercontent.com/23181508/32229578-1d7b12d0-be8c-11e7-875a-61d8e8e9418a.png)
```javascript
var a = {};
console.log(a.prototype);  //undefined
console.log(a.__proto__);  //Object {}

var b = function(){}
console.log(b.prototype);  //b {}
console.log(b.__proto__);  //function() {}
```


![787416-20160323103622089-1134417169](https://user-images.githubusercontent.com/23181508/32230145-75fd3356-be8d-11e7-909b-906aad2f1d6c.png)
```javascript
/*1、字面量方式*/
var a = {};
console.log(a.__proto__);  //Object {}

console.log(a.__proto__ === a.constructor.prototype); //true

/*2、构造器方式*/
var A = function(){};
var a = new A();
console.log(a.__proto__); //A {}

console.log(a.__proto__ === a.constructor.prototype); //true

/*3、Object.create()方式*/
var a1 = {a:1}
var a2 = Object.create(a1);
console.log(a2.__proto__); //Object {a: 1}

console.log(a.__proto__ === a.constructor.prototype); //false（此处即为图1中的例外情况）
```


![787416-20160322110905589-2039017350](https://user-images.githubusercontent.com/23181508/32230148-772e5aa2-be8d-11e7-8a75-05ad77ea46af.png)
```javascript
var A = function(){};
var a = new A();
console.log(a.__proto__); //A {}（即构造器function A 的原型对象）
console.log(a.__proto__.__proto__); //Object {}（即构造器function Object 的原型对象）
console.log(a.__proto__.__proto__.__proto__); //null
```
