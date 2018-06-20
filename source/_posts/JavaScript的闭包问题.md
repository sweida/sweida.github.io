---
title: JavaScript的闭包问题
abbrlink: 23155
date: 2018-06-20 11:01:05
categories: 前端
tags:
- JavaScript
---
JavaScript是一种非常强大的函数式编程语言，可以动态创建函数对象。

由于JavaScript还支持闭包（Closure），因此，函数可以引用其作用域外的变量，非常强大。

来看看在JavaScript中使用闭包的陷阱：

```javascript
var tasks = [];

for (var i=0; i<3; i++) {
    tasks.push(function() {
        console.log('>>> ' + i);
    });
}
console.log('end for.');

for (var j=0; j<tasks.length; j++) {
    tasks[j]();
}
```

如果在循环中创建函数，并引用循环变量，原意是打印出0，1，2，但结果却是一样的：

```
end for.
>>> 3
>>> 3
>>> 3
```

这个问题的原因在于，函数创建时并未执行，所以先打印end for.，然后才执行函数。

由于函数引用了循环变量i，在函数执行时，由于i的值已经变成了3，所以，打印出的结果不对。

如果我们用一个变量n来复制一份i传入呢？

```javascript
var tasks = [];

for (var i=0; i<3; i++) {
    var n = i;
    tasks.push(function() {
        console.log('>>> ' + n);
    });
}

console.log('end for.');

for (var j=0; j<tasks.length; j++) {
    tasks[j]();
}
```

结果还是不对：

```
end for.
>>> 2
>>> 2
>>> 2
```

注意到i会比n多加一次，所以n的最终值是2。

这个问题的原因是JavaScript虽然有局部变量，但局部变量只有函数作用域，而没有其他语言通常有的块作用域（循环内部，if内部），所以，无论在函数内部的哪个地方定义变量，变量作用域都是整个函数。

把上述代码的变量提取到函数开头，其实是这样的：

```javascript
var tasks, i, n, j;

tasks = [];

for (i=0; i<3; i++) {
    n = i;
    tasks.push(function() {
        console.log('>>> ' + n);
    });
}

console.log('end for.');

for (j=0; j<tasks.length; j++) {
    tasks[j]();
}
```

完全等价。

根据道爷的说法，JavaScript缺少块作用域是语言设计的一个缺陷。所以，上面的代码有问题，就是因为n的作用域是整个函数，而不是循环。

创建闭包的一条原则就是：
不要引用循环变量！
这条原则对有没有块作用域的函数式编程语言都适用。
如果一定要在闭包中引用循环变量怎么办？？？

方法是再创建一个函数，将循环变量作为函数参数传入：

```javascript
var tasks = [];

for (var i=0; i<3; i++) {
    var fn = function(n) {
        tasks.push(function() {
            console.log('>>> ' + n);
        });
    };
    fn(i);
}
```

这段代码是正确的，是因为循环内部，fn()函数立即被执行，因此，闭包拿到的参数n就是当前循环变量的值的副本。

最后，简化语法，直接用匿名函数的立即执行模式(function() { ... })()改写如下：

```javascript
var tasks = [];

for (var i=0; i<3; i++) {
    (function(n) {
        tasks.push(function() {
            console.log('>>> ' + n);
        });
    })(i);
}
```
