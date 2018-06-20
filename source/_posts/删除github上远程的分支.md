---
title: 删除github上远程的分支
date: 2018-06-15 15:37:56
categories: 工具
tags:
  - git
comments: true
abbrlink: 26064
---

#### 如果想删除github上的source分支,通过下面命令推送一个空分支到远程分支，其实就相当于删除远程分支
```
git push origin :source
```
如图
![刪除分支](26064/001.png)

#### 切换并新建分支
```
git checkout -b dev
```
它是下面两条命令的简写，新建分支，然后切换分支
```
git branch dev
git checkout dev
```
