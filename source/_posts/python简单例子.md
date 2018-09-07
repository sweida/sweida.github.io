---
title: python简单例子
abbrlink: 56097
date: 2018-09-07 14:37:20
categories: 后端
tags:
  - python
---

### python 简易猜数字游戏
用户可以一直猜，每猜一次都要重复运行一次程序，直到猜对为止。 这巧妙地阐述了 while 语句的用法。
```python
number = 23
running = True

while running:
    guess = int(input('Enter an integer : '))

    if guess == number:
        print('Congratulations, you guessed it.')
        # 这会导致 while 循环停止
        running = False
    elif guess < number:
        print('No, it is a little higher than that.')
    else:
        print('No, it is a little lower than that.')
else:
    print('The while loop is over.')
    # 你可以在此处继续进行其它你想做的操作

print('Done')
```

输出
```python
$ python while.py
Enter an integer : 50
No, it is a little lower than that.
Enter an integer : 22
No, it is a little higher than that.
Enter an integer : 23
Congratulations, you guessed it.
The while loop is over.
Done
```
`else`语句块会在 `while` 循环的条件变为 `False` 时执行——甚至有可能在第一次检查条件时，条件就是 `False` 。如果 `while` 循环中有一个 `else` 从句，它总是会执行到，除非用 `break` 语句跳出循环。

我们将`True` 和 `False` 称为布尔类型，而且你可以认为它们分别等于数值 `1` 和 `0` 。


### for 循环
`for..in` 语句是另一种循环语句，它会 迭代 对象序列，即会遍历序列中的的每个项。在后面的章节中，我们将详细了解 序列 。目前你只需要知道的是，序列只是一个有序的项的集合。
示例（保存为 for.py）：
```python
for i in range(1, 5):
    print(i)
else:
    print('The for loop is over')
```
输出：
```python
$ python for.py
1
2
3
4
The for loop is over
```

### 查找字符串
```python
# 这是一个字符串对象
name = 'Swaroop'

if name.startswith('Swa'):
    print('Yes, the string starts with "Swa"')

if 'a' in name:
    print('Yes, it contains the string "a"')

if name.find('war') != -1:
    print('Yes, it contains the string "war"')

delimiter = '_*_'
mylist = ['Brazil', 'Russia', 'India', 'China']
print(delimiter.join(mylist))
```
输出：
```python
$ python ds_str_methods.py
Yes, the string starts with "Swa"
Yes, it contains the string "a"
Yes, it contains the string "war"
Brazil_*_Russia_*_India_*_China
```

### 模块的 `__name__`
每一个模块都有一个名称，在模块中我们可以通过判断语句来确定模块的名称。这在一种情形下特别有用：确定模块被导入了？还是在独立的运行。如之前提到过的，当模块第一次被导入的时候，模块的代码将被执行。
我们可以通过这一点，**让模块在被导入和独立运行时执行不同的操作**。通过模块的 `__name__` 属性可以实现这个功能。

示例（另存为 module_using_name.py）：
```pythonpython
if __name__ == '__main__':
    print('我是自己运行时显示')
else:
    print('我是在被import时显示的')
```

输出：
```python
$ python module_using_name.py
我是自己运行时显示

$ python
>>> import module_using_name
我是在被import时显示的
>>>
```
代码是如何工作的？

每一个 `Python` 模块都定义了各自的 `__name__`。如果其值为 `__main__`，这说明用户正在单独运行这个模块，这时我们可以进行合适的操作

### `__init__` 方法
对 `Python` 类来说，许多方法名有特别的重要性。现在，我们来考察一个重要的 `__init__` 方法。

`__init__` 方法将在类的对象被初始化（也就是创建）的时候自动的调用。这个方法将按照你的想法 初始化 对象（通过给对象传递初始值）。请注意这个名字的开头和结束都是双下划线。

例子（保存为文件  oop_init.py ）：
```python
class Person:
    def __init__(self, name):
        self.name = name

    def say_hi(self):
        print('Hello, my name is', self.name)

p = Person('Swaroop')
p.say_hi()
# 上面两行也可以写成下面这种形式
# Person('Swaroop').say_hi()
```

输出
```python
$ python oop_init.py
Hello, my name is Swaroop
```
这是如何工作的

这里，我们定义了 `__init__` 方法。这个方法除了通常的 `self` 变量之外，还有一个参数 `name` 。 这里我们创建了一个新的也叫做 `name` 的域。注意这里有两个不同的变量却都被叫做 `name` 。这是没有问题的，因为带点的标记 `self.name` 表示有一个叫做 `name` 的域是这个类的一部分，而另外一个  `name` 是一个局部变量。这里我们显式地指出使用哪个变量，因此没有任何冲突。

当新建一个新的 `Person` 类的实例 `p` 的时候，我们通过调用类名的方式来创建这个新的实例，在紧跟着的括号中填入初始化参数： `p = Person('Swaroop')` 。

我们没有显式的调用 `__init__` 这个方法，这是这个方法特殊之处。

正如 `say_hi` 方法所示的，现在我们在我们的方法之中可以使用 `self.name` 这个域了。
