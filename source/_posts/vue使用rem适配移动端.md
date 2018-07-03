---
title: vue使用rem适配移动端
abbrlink: 33799
date: 2018-07-03 09:56:10
categories: 前端
tags:
  - vue
comments: true
---

vu移动端使用vant-ui、mint-ui，想做移动端适配，可以借助px2rem 插件方便的将px单位转为了rem。

### 安装
```
npm install px2rem-loader  lib-flexible --save 
```

### 在项目入口文件main.js中引入lib-flexible
```
import 'lib-flexible/flexible.js'
```

### 在build下的 utils.js中，找到`exports.cssLoaders = function (options) { }` ，在里面添加：
```js
const px2remLoader = {
  loader: 'px2rem-loader',
  options: {
    remUnit: 37.5
  }
}

// 找到generateLoaders 方法，只需添加 postcssLoader，修改成：
function generateLoaders (loader, loaderOptions) {
  const loaders = options.usePostCSS ? [cssLoader, postcssLoader, px2remLoader] : [cssLoader]

  if (loader) {
    loaders.push({
      loader: loader + '-loader',
      options: Object.assign({}, loaderOptions, {
        sourceMap: options.sourceMap
      })
    })
  }
}
```

### index.html的head里的meta最好修改成下面，禁止手机端放大页面
```html
<meta name="viewport" content="initial-scale=1, maximum-scale=1, minimum-scale=1, user-scalable=no">
```

重启项目，会发现默认的px自动被转为rem 了