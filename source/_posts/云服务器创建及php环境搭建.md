---
title: 云服务器创建及php环境搭建
abbrlink: 51805
date: 2018-09-26 16:31:08
tags:
  - 服务器搭建
categories: 工具
img: archives/51805/001.png
---

云服务最好选择linux Centos 6.x系统，创建后，默认用户 `root`，设置默认密码
![000](51805/000.png)
推荐使用 `MobaXterm` (类似工具xshell, putty) 工具连接云服务器。输入服务器公网IP，默认端口22，输入刚刚设置的用户名 `root` 和密码，显示界面如下(`MobaXterm`可以保存用户密码，下次登陆输入 `root` 即可自动登陆用户 `root`)
![001](51805/001.png)

### 添加端口号
默认是没有开启端口号的，需要去 `安全组` 里吧几个常用的端口号添加上(8080, 80, 有多个站点也需要设置多个端口号)
![003](51805/003.png)
添加后
![004](51805/004.png)

### 安装php环境

lanmp一键安装包安装
官网：[https://www.wdlinux.cn/wdcp/install.html](https://www.wdlinux.cn/wdcp/install.html)

### 安装方法一
```bash
wget http://dl.wdlinux.cn/lanmp_laster.tar.gz
tar zxvf lanmp_laster.tar.gz
sh lanmp.sh

# 卸载(注意备份数据,否则后果自负)
sh install.sh uninstall
```
默认安装N+A的引擎组合

### 安装方法二
RPM包安装 RPM包安装软件版本较老，建议使用源码安装更新的版本
分别运行下面2行命令即可
```bash
wget http://down.wdlinux.cn/in/lanmp_wdcp_ins.sh
# wget 是下载的意思
sh lanmp_wdcp_ins.sh
# sh 是执行lanmp_wdcp_ins.sh文件

# 卸载 (切记备份好数据)
sh lanmp_wdcp_ins.sh uninstall
```
然后就可以了，在浏览器打开云服务器的公网ip `114.116.72.19` 就可以看的界面了，默认端口是8080，`114.116.72.19:8080` 打开操作界面，
![002](51805/002.png)

默认安装php版本是5.2，安装php新版本的方法
```bash
wget http://down.wdlinux.cn/in/phps.sh

sh phps.sh 7.1.4
# 即可安装7.1.4
# (共支持7个版本的PHP，如5.2.17/5.3.29/5.4.45/5.5.38/5.6.30/7.0.18/7.1.4)
```
