---
title: 手机访问PC网站自动跳转到手机版
categories: 前端
abbrlink: 34869
date: 2018-06-14 17:03:57
tags: JavaScript
---

手机访问PC网站自动跳转到手机版，在body下面添加下面代码
```
<script type="text/javascript">
  try {
    var urlhash = window.location.hash
    if (!urlhash.match('fromapp')) {
      if (navigator.userAgent.match(/(iPhone|iPod|Android|ios|iPad)/i)) {
        window.location = '你的手机版地址'
      }
    }
  }
  catch (err) {}
</script>
```
