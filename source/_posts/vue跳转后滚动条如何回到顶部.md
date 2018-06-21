---
title: vue跳转后滚动条如何回到顶部
date: 2017-07-14 17:03:25
abbrlink: 13522
categories: 前端
tags:
  - vue
---

>当你页面滑动到页面中间，然后页面跳转时滚动条会保持在页面中间位置
如何让跳转后滚动条回到顶部呢？
试试在main.js入口文件配合vue-router写这个

```js
router.afterEach((to,from,next) => {
    window.scrollTo(0,0);
});
```
