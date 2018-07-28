---
title: ES6使你的代码更简洁
abbrlink: 57328
date: 2018-07-29 02:05:28
tags:
  - JavaScript
categories: 前端
---

### 展开操作符
ES6引入了 `...` 操作符，它被称为 “展开操作符”。 它有两个主要用途：将数组或对象传播到新的数组或对象中，并将多个参数合并到一个数组中。

第一个用例是你可能遇到的最多的用例，所以我们先看看。
```JavaScript
let a = [3, 4, 5]
let b = [1, 2, ...a, 6]
console.log(b)
// [1, 2, 3, 4, 5, 6]
```

这对于将数组中的元素作为一组变量传递给函数时非常有用。
```JavaScript
function foo(a, b, c) { 
  console.log(`a=${a}, b=${b}, c=${c}`)
} 
let data = [5, 15, 2]
foo( ...data)  
// a=5, b=15, c=2
```

它也可以展开对象，将每个键值对输入到新对象中。（对象展开实际上是在提案的第4阶段，并将在 ES2018 中正式推出，目前仅支持 Chrome 60 或更高版本，Firefox 55 或更高版本 ，以及 node 6.4.0 或更高版本）
```JavaScript
let car = { type: 'vehicle ', wheels: 4}
let fordGt = { make: 'Ford', ...car, model: 'GT'}
console.log(fordGt)  
// {make: 'Ford', model: 'GT', type: 'vehicle', wheels: 4}
```

函数可以使用 rest（剩余）参数语法接受任意数量的参数作为数组。
```JavaScript
function ArgumentsArray(...theArguments) {
  console.log(theArguments)
}
 
ArgumentsArray('hi', 'there', 'bud')
// 将打印 ['hi', 'there', 'bud']
```



### Object.values()
返回一个对象属性值的数组。
```JavaScript
const icecreamColors = {
    chocolate: 'brown',
    vanilla: 'white',
    strawberry: 'red',
}
 
const colors = Object.values(icecreamColors)
// colors = ["brown", "white", "red"]
```

### Object.keys()
返回一个对象属性名的数组。
```JavaScript
const icecreamColors = {
  chocolate: 'brown',
  vanilla: 'white',
  strawberry: 'red',
}
 
const types = Object.keys(icecreamColors)
// types = ["chocolate", "vanilla", "strawberry"]
```

### Object.entries()
创建一个数组，其中包含一个对象的键/值对数组。
```JavaScript
const weather = {
  rain: 0,
  temperature: 24,
  humidity: 33,
}
 
const entries = Object.entries(weather)
// entries = [['rain', 0], ['temperature', 24], ['humidity', 33]]
```

### Object.assign()
允许对象组合在一起。不再需要这个方法，因为您可以使用上面说的对象展开语法。与对象展开操作符一样，Object.assign() 也不会执行深层克隆。当谈到深度克隆对象时，Lodash 是你最好的朋友。
```JavaScript
const firstObject = {
  firstName: 'Robert'
}
 
const secondObject = {
  lastName: 'Cooper'
}
 
const combinedObject = Object.assign(firstObject, secondObject)
// combinedObject = { firstName: 'Robert', lastName: 'Cooper' }
```

### 随机排列数组
使用 `Array.sort()` 来重新排序元素，比较器中使用 `Math.random()` 。
```JavaScript
const shuffle = arr => arr.sort(
  () => Math.random() - 0.5
)
// shuffle([1,2,3]) -> [2,3,1]
```

### 数字数组求和
使用  `Array.reduce()` 将每个值添加到累加器，并使用0值初始化。
```JavaScript
const sum = arr => arr.reduce(
  (acc, val) => acc + val, 0
)
// sum([1,2,3,4]) -> 10
```

### 数组取样随，机获取数组中的1个元素
使用  `Math.random()` 生成一个随机数，乘以 length，并使用 `Math.floor()` 舍去小数获得到最接近的整数。这个方法也适用于字符串。
```JavaScript
const sample = arr => arr[Math.floor(Math.random() * arr.length)]
// sample([3, 7, 9, 11]) -> 9
```

### 反转一个字符串
用数组解构和 `Array.reverse()` 来反转字符串中字符的顺序。使用 join('')合并字符串。
```JavaScript
const reverseString = str => [...str].reverse().join('')
// reverseString('foobar') -> 'raboof'
```

### 在指定的范围内生成一个随机整数
使用 `Math.random()` 生成一个随机数并将其映射到所需的范围，使用 `Math.floor()` 使其成为一个整数。
```JavaScript
const num = (min, max) => Math.floor(Math.random() * (max - min + 1)) + min
num(0, 5) -> 2
```