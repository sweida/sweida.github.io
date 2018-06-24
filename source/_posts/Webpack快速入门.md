---
title: Webpack快速入门
abbrlink: 45699
date: 2018-06-23 01:11:10
categories: 工具
tags:
  - Webpack
img: archives/45699/webpack.jpg
---

### webpack 是什么东西?
先看看webpack官网怎么吹牛逼介绍自己的：
>Webpack 是当下最热门的前端资源模块化 管理和打包 工具。它可以将许多松散的模块按照依赖和规则打包成符合生产环境部署的前端资源。还可以将按需加载的模块进行代码分割，等到实际需要的时候再异步加载。

### 为什要使用webpack
现今的很多网页其实可以看做是功能丰富的应用，它们拥有着复杂的JavaScript代码和一大堆依赖包。为了简化开发的复杂度，前端社区涌现出了很多好的实践方法

* 模块化，让我们可以把复杂的程序细化为小的文件;
* 类似于TypeScript这种在JavaScript基础上拓展的开发语言：使我们能够实现目前版本的JavaScript不能直接使用的特性，并且之后还能转换为JavaScript文件使浏览器可以识别；
* Scss，less等CSS预处理器
... ...

### 安装
```bash
# 全局安装
npm install -g webpack

# 生成package.json文件,一路回车
npm init

# 安装到你的项目目录package.json中
npm install --save-dev webpack
# webpack v4.0以上还需要安装webpack-cli
npm install --save-dev webpack-cli
```

Webpack配置文件为webpack.config.js，根目录下新建一个名为webpack.config.js的文件
```js
module.exports = {
  entry:  __dirname + "/app/main.js",   // 已多次提及的唯一入口文件
  output: {
    path: __dirname + "/build",         // 打包后的文件存放的地方
    filename: "bundle.js"               // 打包后输出文件的文件名
  }
}
// 注：“__dirname”是node.js中的一个全局变量，它指向当前执行脚本所在的目录。
```
在终端执行`webpack`就会自动引用webpack.config.js文件中的配置选项

在package.json中对scripts对象进行相关设置，即可使用简单的npm build命令来替代上面略微繁琐的命令，设置方法如下的scripts
```json
{
  "name": "webpack-demo",
  "version": "1.0.0",
  "description": "",
  "private": true,
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "webpack"
  },
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "webpack": "^4.12.0"
  }
}
```

### 使用webpack构建本地服务器
想不想让你的浏览器监听你的代码的修改，并自动刷新显示修改后的结果，其实Webpack提供一个可选的本地开发服务器，这个本地服务器基于node.js构建，可以实现你想要的这些功能，不过它是一个单独的组件，在webpack中进行配置之前需要单独安装它作为项目依赖
```
npm install --save-dev webpack-dev-server
```
把这些命令加到webpack的配置文件webpack.config.js中
```js
module.exports = {
  devtool: 'eval-source-map',

  entry:  __dirname + "/app/main.js",   // 唯一入口文件
  output: {
    path: __dirname + "/build",         // 打包后的文件存放的地方
    filename: "bundle.js"               // 打包后输出文件的文件名
  }

  devServer: {
    contentBase: "./build",     // 本地服务器所加载的页面所在的目录
    historyApiFallback: true,   // 不跳转
    port: 8000,                 // 不写默认8080端口
    inline: true                // 实时刷新
  }
}
```

在package.json中的scripts对象中添加如下命令，用以开启本地服务器：
```
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "webpack",
    "server": "webpack-dev-server --open"
  },
```
在终端中输入`npm run server`即可在本地的8000端口查看结果


### Loaders
鼎鼎大名的Loaders登场了！

loader 让 webpack 能够去处理那些非 JavaScript 文件（webpack 自身只理解 JavaScript）。loader 可以将所有类型的文件转换为 webpack 能够处理的有效模块，然后你就可以利用 webpack 的打包能力，对它们进行处理。

Loaders是webpack提供的最激动人心的功能之一了。通过使用不同的loader，webpack有能力调用外部的脚本或工具，实现对不同格式的文件的处理，比如说分析转换scss为css，或者把下一代的JS文件（ES6，ES7)转换为现代浏览器兼容的JS文件，对React的开发而言，合适的Loaders可以把React的中用到的JSX文件转换为JS文件。

Loaders需要单独安装并且需要在webpack.config.js中的modules关键字下进行配置，Loaders的配置包括以下几方面：

在更高层面，在 webpack 的配置中 loader 有两个目标：

1. test 属性，用于标识出应该被对应的 loader 进行转换的某个或某些文件。
2. use 属性，表示进行转换时，应该使用哪个 loader。

webpack.config.js
```js
const path = require('path');

const config = {
  output: {
    filename: 'my-first-webpack.bundle.js'
  },
  module: {
    rules: [
      { test: /\.txt$/, use: 'raw-loader' }
    ]
  }
};

module.exports = config;
```
* test：一个用以匹配loaders所处理文件的拓展名的正则表达式（必须）
* loader：loader的名称（必须）
* include/exclude:手动添加必须处理的文件（文件夹）或屏蔽不需要处理的文件（文件夹）（可选）；
* query：为loaders提供额外的设置选项（可选）

### 插件(plugins)
loader 被用于转换某些类型的模块，而插件则可以用于执行范围更广的任务。插件的范围包括，从打包优化和压缩，一直到重新定义环境中的变量。插件接口功能极其强大，可以用来处理各种各样的任务。

想要使用一个插件，你只需要`require()`它，然后把它添加到`plugins`数组中。多数插件可以通过选项(option)自定义。你也可以在一个配置文件中因为不同目的而多次使用同一个插件，这时需要通过使用 new 操作符来创建它的一个实例。

webpack.config.js
```js
const HtmlWebpackPlugin = require('html-webpack-plugin'); // 通过 npm 安装
const webpack = require('webpack');  // 用于访问内置插件

const config = {
  module: {
    rules: [
      { test: /\.txt$/, use: 'raw-loader' }
    ]
  },
  plugins: [
    new webpack.optimize.UglifyJsPlugin(),
    new HtmlWebpackPlugin({template: './src/index.html'})
  ]
};

module.exports = config;
```
