---
title: laravel创建api
abbrlink: 61614
date: 2018-09-18 16:58:58
categories: 后端
tags:
  - laravel
  - PHP
---



### 数据库操作之 - Eloquent ORM

模型的建立及查询数据

简介：laravel所自带的Eloquent ORM 是一个ActiveRecord实现，用于数据库操作。每个数据表都有一个与之对应的模型，用于数据表交互。

创建模块model
```
php artisan make:model User
```


mysql添加数据库
```
insert into users (username, `password`) value('佟丽娅', '123456')
```
