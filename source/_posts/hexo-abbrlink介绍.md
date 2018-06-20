---
title: hexo-abbrlink介绍
abbrlink: 58183
date: 2018-06-16 01:50:03
categories: 前端
tags:
  - hexo
---

### 前言
hexo的地址默认提供的方案是使用年/月/日/标题，这简直反人类啊，写博客的标题是中文，中文的网址给我带来了许多麻烦。那要怎么优化地址呢？

### 需求
* 全自动生成唯一连接
* 重复生成不会覆盖
* 尽量短小精悍
* 持久保存可供修改
* 不引用外部模块

### 最优方案
对标题+时间进行md5然后再转base64，保存在front-matter中。

### 实现
首先是注册before_post_render钩子，然后取出来abbrlink这个属性看是否存在，存在的就不管了，否则就生成连接。
其中使用了nodejs自带的crypto模块来获取md5校验值，用hexo-front-matter来转换front-matter，然后用hexo-fs来保存文件。

### 安装
```
npm install hexo-abbrlink --save
```

### 使用
打开根目录_config.yml文件，修改permalink中类似这样
```
permalink: posts/:abbrlink/
```
其中:abbrlink代表连接地址。 posts为你想要使用的地址目录

### 最终
用 hexo new xxxx 生成新博文时就会自动在.md里多生成一行
> posts: 12345（一串数字）

然后博文地址就变成 xxxx.github.io/posts/12345.html
