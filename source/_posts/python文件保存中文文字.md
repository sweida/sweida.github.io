---
title: python保存json文件并输出中文
abbrlink: 55006
date: 2018-09-07 17:46:27
categories: 后端
tags:
  - python
---

### python保存json文件并输出中文
```python
import io

sendData = [
  {
    id: 1,
    name: '奥特曼'
  },{
    id: 2,
    name: '小怪兽'
  }
]

with io.open('data.json', 'w', encoding="utf-8") as file:
    json.dump(sendData, file, ensure_ascii=False, sort_keys=True, indent=2)
print('保存成功')
```
工作原理

我们使用 io.open 然后在第一个 open 语句中使用 encoding 参数对信息进行编码，然后在解码信息时再在第二个 open 语句中使用该参数。 请注意，我们应该只在文本模式下使用 open 语句时的使用编码。

每当我们编写一个使用Unicode文字的程序（通过在字符串之前放置一个 u ）就像我们上面使用的那样，我们必须确保 Python 本身被告知我们的程序使用UTF-8，我们必须把 # encoding=utf-8 注释在我们程序的顶部。

`json.dump()`将其保存为json格式，`ensure_ascii=False`禁止使用ascii编码保存，`indent=2` Tab为2个空格大小

`json.loads()`读取json文件

### 创建文件夹
```python
import os

if not os.path.exists('data'):
     os.mkdir('data')
```

### 输出今天日期
```python
import time

# 获取今天年月日
nowdate = time.localtime(time.time())  # 获得当前时间戳
today = time.strftime('%Y-%m-%d %H:%M:%S', nowdate)  # 转换成指定格式
print(today)

>>> 2018-09-07 18:08:12
```
