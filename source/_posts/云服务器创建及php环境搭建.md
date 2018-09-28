---
title: 云服务器创建及php环境搭建
abbrlink: 51805
date: 2018-09-26 16:31:08
tags:
  - 服务器搭建
categories: 工具
---

云服务最好选择linux Centos 6.x系统，创建后，默认用户 `root`，设置默认密码

推荐使用 `MobaXterm` (类似工具xshell, putty) 工具连接云服务器。输入服务器公网IP，默认端口22，输入刚刚设置的用户名 `root` 和密码，显示界面如下(`MobaXterm`可以保存用户密码，下次登陆输入 `root` 即可自动登陆用户 `root`)
![001](51805/001.png)

### 安装linux面板，选择宝塔
官网：[https://www.bt.cn/download/linux.html](https://www.bt.cn/download/linux.html)

__Centos 系统安装命令：__
```bash
yum install -y wget

wget -O install.sh http://download.bt.cn/install/install.sh

sh install.sh
```

默认端口是8888，114.116.72.19:8888 打开操作界面，
![003](51805/003.png)

在软件管理里安装 `Nignx、web环境、php、mysql`，选择对应的版本安装

### 安装解压rar文件
```bash
# 下载
wget http://www.rarlab.com/rar/rarlinux-x64-5.3.0.tar.gz

# 解压
tar -zxvf rarlinux-x64-5.3.0.tar.gz

# 进入rar文件夹
cd rar

# 进行配置　
make

# 输出
>>> mkdir -p /usr/local/bin
>>> mkdir -p /usr/local/lib
>>> cp rar unrar /usr/local/bin
>>> cp rarfiles.lst /etc
>>> cp default.sfx /usr/local/lib
# 可以用rar命令开始解压
```

__解压和压缩rar文件__
```bash
# 解压 dist.rar 到当前目录
rar x dist.rar

# 压缩文件，将 dist目录打包为 dist.rar
rar dist.rar ./dist/
```
__解压zip文件__
```
unzip dist.zip
```
