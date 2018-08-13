---
title: laravel学习笔记
abbrlink: 22843
date: 2018-08-04 00:43:57
categories: 后端
tags:
  - laravel
img: archives/22843/laravel.jpg
---

## Laravel 5.6学习笔记
要求:
PHP >= 7.1.3

### 通过 Laravel 安装器
通过 Composer 安装 Laravel 安装器 (Composer安装看上一篇文章)
```
composer global require "laravel/installer"
```

安装完成后，通过简单的 laravel new 命令即可在当前目录下创建一个新的 Laravel 应用，例如，laravel new blog 将会创建一个名为 blog 的新应用，且包含所有 Laravel 依赖。该安装方法比通过 Composer 安装要快很多：
```
laravel new blog
```

### 本地开发服务器
如果你在本地安装了 PHP，并且想要使用 PHP 内置的开发环境服务器为应用提供服务，可以使用 Artisan 命令 
```
php artisan serve
```
该命令将会在本地启动开发环境服务器，这样在浏览器中通过 http://localhost:8000 即可访问应用：
![learnlaravel](22843/learnlaravel.png)