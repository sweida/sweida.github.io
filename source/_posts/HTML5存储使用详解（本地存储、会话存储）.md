---
title: HTML5存储使用详解（本地存储、会话存储）
date: 2018-06-15 11:44:02
tags:
  - HTML5
categories: 前端
id: 2323
---

### 1，Web存储介绍
HTML5的Web存储功能是让网页在用户计算机上保存一些信息。Web存储又分为两种：
（1）本地存储，对应 localStorage 对象。用于长期保存网站的数据，并且站内任何页面都可以访问该数据。
（2）会话存储，对应 sessionStorage 对象。用于临时保存针对一个窗口（或标签页）的数据。在访客关闭窗口或者标签页之前，这些数据是存在的，而关闭之后就会被浏览器删除。


### 2，本地存储与会话存储的异同
（1）本地存储和会话存储的操作代码完全相同，它们的区别仅在于数据的寿命。
（2）本地存储主要用来保存访客将来还能看到的数据。
（3）会话存储则用于保存那些需要从一个页面传递给下一个页面的数据。

### 3，Web存储容量限制
大多数浏览器都把本地存储限制为 5MB 以下。这个是和网站所在的域联系在一起的。

### 4，Web存储的使用样例
下面以本地存储（localStorage）为例，会话存储改成 sessionStorage 对象即可。

#### （1）文本数据的保存和读取
```javascript
localStorage.setItem("user_name","hangge.com");
var userName = localStorage.getItem("user_name");
```

#### （2）数值的保存和读取
```javascript
localStorage.setItem("user_age",100);
var userAge = Number(localStorage.getItem("user_age"));
```

#### （3）日期的保存和读取
```javascript
//创建日期对象
var today = new Date();

//按照YYY/MM/DD的标准格式把日期转换成文本字符串，然后保存为文本
var todayString = today.getFullYear() + "/" + today.getMonth() + "/" + today.getDate();
localStorage.setItem("session_started", todayString);

//取得日期文本，并基于该文本创建新的日期对象
var newToday = new Date(localStorage.getItem("session_started"));
alert(newToday.getFullYear());
```
#### （4）自定义对象的保存和读取
对象的保存和读取可以通过JSON编码转换来实现。
JSON.stringify()：把任何对象连同其数据转换为文本形似。
JSON.parse()：把文本转换回对象。
```javascript
//自定义一个User对象
function User(n, a, t) {
    this.name = n;
    this.age = a;
    this.telephone = t;
}

//创建User对象
var user = new User("hangge", 100, "123456");
//将其保存为方便的JSON格式
sessionStorage.setItem("user", JSON.stringify(user));

//跳转页面
//window.location = "hangge.html";

//将JSON文本转回原来的对象
var user2 = JSON.parse(sessionStorage.getItem("user"));
alert(user2.name);
```

#### （5）检测某个键的值是否为空，可以直接测试是否等于null
```javascript
if(localStorage.getItem("user_name") == null){
    alert("用户名不存在！");
}
```

#### （6）删除数据项
```javascript
localStorage.removeItem("user_name");
```

#### （7）清除所有数据
```javascript
localStorage.clear();
```

#### （8）查找所有的数据项
不知道键名，可以使用 key() 方法从本地或者会话存储中取得所有的数据项。下面样例，点击按钮后就会列出所有本地存储中的数据。
```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>Find All Items</title>

  <script>
    function findAllItems() {
      //取得用于保存数据项的<ul>元素
      var itemList = document.getElementById("itemList");
      //清除列表
      itemList.innerHTML = "";

      //遍历所有数据项
      for (var i=0; i<localStorage.length; i++) {
        //取得当前位置数据项的键
        var key = localStorage.key(i);
        //取得以该键保存的数据值
        var item = localStorage.getItem(key);

        //用以上数据创建一个列表项添加到页面中
        var newItem = document.createElement("li");
        newItem.innerHTML = key + ": " + item;
        itemList.appendChild(newItem);
      }
    }
  </script>
<body>
  <button onclick="findAllItems()">导出所有本地存储数据</button>
  <ul id="itemList">
  </ul>
</body>
</html>
```

### 5，响应存储变化
Web存储也为我们提供了在不同浏览器窗口间通信的机制。也就是说在本地存储或会话存储发生变化时，其他查看同一页面或者同一站点中其他页面的窗口就会触发 window.onStorage 事件。
这里说的存储变化，指的是向存储中添加新数据项，修改既有数据项，删除数据项或者清除所有数据。而对存储不产生任何影响的操作（用既有键名保存相同的值，或者清除原本就是空的存储空间），不会引发onStorage 事件。
