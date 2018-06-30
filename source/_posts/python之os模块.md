---
title: python之os模块
abbrlink: 26568
date: 2018-07-01 02:28:30
categories: 后端
tags:
  - python
comments: true
---

Python的标准库中的os模块包含普遍的操作系统功能。
os模块负责程序与操作系统的交互，提供了访问操作系统底层的接口
### 加载
```
import os
```

### os 常用方法
```python
# 取得当前目录
os.getcwd()

# 列出指定目录的所有文件
os.listdir('dirname') 

# 删除文件
os.remove(‘path/filename’) 

# 重命名文件
os.rename(oldname, newname) 

# 生成目录树下的所有文件名
os.walk() 

# 改变目录
os.chdir('dirname') 

# 创建目录/多层目录
os.mkdir/makedirs('dirname')

# 删除目录/多层目录
os.rmdir/removedirs('dirname') 

# 改变目录权限
os.chmod() 

# 去掉目录路径，返回文件名
os.path.basename(‘path/filename’) 

# 去掉文件名，返回目录路径
os.path.dirname(‘path/filename’) 

# 将分离的各部分组合成一个路径名
os.path.join(path1[,path2[,...]]) 

# 返回( dirname(), basename())元组
os.path.split('path') 

# 返回 (filename, extension) 元组
os.path.splitext() 

# 分别返回最近访问、创建、修改时间
os.path.getatime\ctime\mtime 

# 返回文件大小
os.path.getsize() 

# 是否存在
os.path.exists() 

# 是否为绝对路径
os.path.isabs() 

# 是否为目录
os.path.isdir() 

# 是否为文件
os.path.isfile() 
```