---
title: PHP安装和PHP包管理器composer安装
abbrlink: 47504
date: 2018-08-04 00:11:01
categories: 后端
tags:
  - PHP
img: archives/47504/logo.png
---

### 下载PHP
你可以从 [windows.php.net/download](https://windows.php.net/download/) 下载二进制安装包。 解压后, 最好将你的 PHP 所在的根目录（php.exe 所在的文件夹）添加到 PATH 环境变量中，这样就可以从命令行中直接执行 PHP。

### 添加到 PATH 环境变量步骤
* 进入控制面板并打开“系统”图标（我的电脑 -> 系统属性 -> 高级系统属性）
* 点击“环境变量”按钮
* 在“系统变量”栏中
* 找到 Path 这一项
* 鼠标双击 Path 这一项
* 在最后加入你的 PHP 目录，包括前面的“;”  例如（;D:\php-7.2.8）

保存后重新打开命令行输入php -v, 输出版本号即成功
![php](47504/php.png)

### Windows环境下安装composer
对于Windows 的用户而言最简单的获取及执行方法就是使用 [ComposerSetup](https://getcomposer.org/Composer-Setup.exe) 安装程序，它会执行一个全局安装并设置你的 $PATH，所以你在任意目录下在命令行中使用 composer。

下载完成后自动选取php.exe所在目录，然后一直点下一步直到完成
完成后打开命令行输入 composer，输出下面即成功
![composer](47504/composer.png)
