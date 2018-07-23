---
title: ES6模块的import和export用法总结
tags:
  - js
categories: 前端
abbrlink: 8964
date: 2018-07-23 23:12:00
---

### ES6模块主要有两个功能：export和import
* export用于对外输出本模块（一个文件可以理解为一个模块）变量的接口
* import用于在一个模块中加载另一个含有export接口的模块。

#### 例子一
```js
// foo.js
let sex="boy";
let echo=function(value){
  console.log(value)
}
export {sex, echo}  
```

```js
// boo.js
// 通过import获取foo.js文件的内部变量，{}括号内的变量来自于foo.js文件export出的变量标识符。
import {sex, echo} from "./foo.js"

console.log(sex)   // boy
echo(sex)          // boy
```

foo.js文件也可以按如下export语法写，但不如上边直观，不太推荐。
```js
// foo.js
export let sex="boy";

export function echo(value){
　console.log(value)
}
```

#### 例子二
```js
// foo.js
let a='my name is xiaoming'
let b='this is a bird'
export default {a, b}
```

```js
import anyoneword from './foo.js'
console.log(anyoneword)     //一个对象里面包含a,b两个变量。
console.log(anyoneword.a)   // my name is xiaoming
console.log(anyoneword.b)   // this is a bird
```

### export 和 export default

两个导出，下面我们讲讲它们的区别

* export与export default均可用于导出常量、函数、文件、模块等
* 在一个文件或模块中，export、import可以有多个，export default仅有一个
* 通过export方式导出，在导入时要加{ }，export default则不需要
* export能直接导出变量表达式，export default不行。
* export default输出后，import的模块可以起任何变量名

### export defualt
```js
// xxx.js
export defualt function FunName() {
  return fetch({
    url: '/article/list',
    method: 'get'
  });
}
// 一个文件内最多只能有一个export default
```
引用
```js
// xxx.js文件的export default输出一个叫做default的变量，然后系统允许你为它取任意名字。
// 所以可以为import的模块起任何变量名，且不需要用大括号包含
import anyName from '../xxx.js'
```

### export

```js
// xxx.js
export function FunName() {
  return fetch({
    url: '/article/list',
    method: 'get'
  });
}
```
```js
import {xxx} from '../xxx.js'
```
