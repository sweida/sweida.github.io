---
title: 理解Gulp和Webpack
abbrlink: 30083
date: 2018-06-21 08:54:35
categories: 工具
tags:
  - Webpack
  - Gulp
---
![理解Gulp和Webpack](30083/001.jpg)

### Gulp 为何物？
先来听听 Ta 的官网是怎么说：
>Gulp 致力于 自动化和优化 你的工作流，它是一个自动化你开发工作中 痛苦又耗时任务 的工具包。

Gulp 就是为了规范前端开发流程，实现前后端分离、模块化开发、版本控制、文件合并与压缩、mock数据等功能的一个前端自动化构建工具。说的形象点，“Gulp就像是一个产品的流水线，整个产品从无到有，都要受流水线的控制，在流水线上我们可以对产品进行管理

### webpack 又是什么东西?
老规矩，先看看webpack官网怎么吹牛逼介绍自己的：
>Webpack 是当下最热门的前端资源模块化 管理和打包 工具。它可以将许多松散的模块按照依赖和规则打包成符合生产环境部署的前端资源。还可以将按需加载的模块进行代码分割，等到实际需要的时候再异步加载。

单页应用的核心是模块化，ES6 之前 JavaScript 语言本身一直是没有模块系统的，导致 AMD，CMD，UMD 各种轮子模块化方案都蹦出来。对这种模块化乱象，gulp 显得无能为力，gulp 插件对这一块也没有什么想法。不过也可以理解，模块化解决方案可不是谁都能 hold 住的，需要考虑的问题太多了；
对前沿的 SPA 技术 gulp 处理起来显得有些力不从心，例如 Vue 的单文件组件，gulp 配合一些插件可以勉强处理，但是很蹩脚。其实归根结底，还是模块化处理方面的不足；
优化页面加载速度的一条重要法则就是减少 http 请求。gulp 只是对静态资源做流式处理，处理之后并未做有效的优化整合，也就是说 gulp 忽略了系统层面的处理，这一块还有很大的优化空间，尤其是移动端，那才真的是一寸光阴一寸金啊，哪怕是几百毫秒的优化所带来的收益（用户？流量？付费？）绝对超乎你的想象。别跟我说 gulp-concat，CSS Sprites，这俩玩意儿小打小闹还行，遇上大型应用根本拿不上台面。现在的页面动辄上百个零碎资源（图片，样式表，脚本），也就是上百个 http 请求，因此这个优化需求还是相当迫切的。关于为何减少 http 请求可以有效降低页面加载时间戳这里。

### 首先从概念上可以看出，Gulp和Webpack的侧重点是不同的。

>#### Gulp 是基于流的？
流（Stream）不是 gulp 创造的概念，而是从 unix 时代就开始使用的 I/O 机制，一直到现在仍在广泛使用。Node 封装了一个 stream 模块专门用来对流进行操作。gulp 所基于的流即是 Node 封装起来的 stream。上面 `gulp.task()` 代码里面的 pipe 方法并不是 gulp 提供的 api，而是 node 的 api，准确的说应该是 node 的 stream 模块提供的 api。具体是怎么实现的呢：`gulp.src()` 的返回值是 node Stream 的一个实例，之后的 pipe 调用的其实是这个实例的 pipe 方法，而 pipe 方法的返回值依然是 node Stream 实例，以此实现前面的 `.pipe().pipe().pipe()` 这种串联写法。熟悉 jQuery 的同学应该很清楚这种技巧。

>#### webpack 怎么实现像 gulp 一样对大量源文件进行流式处理?
人家 webpack 本来就没打算做这事。webpack 不是以取代 gulp 为目的的，而是为了给大型 SPA 提供更好的构建方案。对大量源文件进行流式处理是 gulp 擅长的事，webpack 不想抢，也没必要抢。即使抢，也无非是再造一个蹩脚的 gulp 出来而已。

>#### 既然 webpack 模块化这么强，那以后模块化就全用 webpack 好了?
webpack 模块化是强，但是他胖啊，不是所有人都抱得动，主要是他为了提供更多的功能封装进了太多东西，所以选择上还是需要因地制宜。如果单纯只是打包 js（多页应用往往是这种需求），完全可以使用 rollup，browserify 这种小而美的实现，因为他们只做一件事——打包js。而如果需要将图片，样式，字体等所有静态资源全部打包，webpack 毫无疑问是首选。这也是为什么越来越多的流行库和框架开始从 webpack 转向使用 rollup 进行打包，因为他们只需要打包 js，webpack 好多强大功能根本用不到。连 rollup 官网也坦言如果你在构建一个库，rollup 绝对是首选，但如果是构建一个应用，那么请选 webpack。

### 下表是从各个角度对 gulp 和 webpack 做的对比：
   --   |   Gulp        |     Webpack     
:------:  |  :-------:    |     :-----:
定位      |   基于流的自动化构建工具	      |  一个万能模块打包器
目标      |   自动化和优化开发工作流，为通用 website 开发而生	|  通用模块打包加载器，为移动端大型 SPA 应用而生
学习难度  |   易于学习，易于使用，api总共只有5个方法     |   有大量新的概念和api，不过好在有详尽的官方文档
适用场景  |   基于流的作业方式适合多页面应用开发         |   一切皆模块的特点适合单页面应用开发
工作方式  |   对输入（gulp.src）的 js，ts，scss，less 等源文件依次执行打包（bundle）、编译（compile）、压缩、重命名等处理后输出（gulp.dest）到指定目录中去，为了构建而打包     |   对入口文件（entry）递归解析生成依赖关系图，然后将所有依赖打包在一起，在打包之前会将所有依赖转译成可打包的 js 模块，为了打包而构建
使用方式  |   常规 js 开发，编写一系列构建任务（task）   |   编辑各种 JSON 配置项
优点      |   适合多页面开发，易于学习，易于使用，接口优雅  |   可以打包一切资源，适配各种模块系统
缺点      |   在单页面应用方面输出乏力，而且对流行的单页技术有些难以处理（比如 Vue 单文件组件，使用 gulp 处理就会很困难，而 webpack 一个 loader 就能轻松搞定）     |   不适合多页应用开发，灵活度高但同时配置很繁琐复杂。“打包一切” 这个优点对于 HTTP/1.1 尤其重要，因为所有资源打包在一起能明显减少浏览器访问页面时的资源请求数量，从而减少应用程序必须等待的时间。但这个优点可能会随着 HTTP/2 的流行而变得不那么突出，因为 HTTP/2 的多路复用可以有效解决客户端并行请求时的瓶颈问题。
结论     |   浏览器多页应用(MPA)首选方案      |   浏览器单页应用(SPA)首选方案


<!-- ### 插件推荐
#### 下面推荐几个 gulp 的插件吧，比较常用的：

```
gulp-load-plugins：自动加载 package.json 中的 gulp 插件
gulp-rename： 重命名
gulp-uglify：文件压缩
gulp-concat：文件合并
gulp-less：编译 less
gulp-sass：编译 sass
gulp-clean-css：压缩 CSS 文件
gulp-htmlmin：压缩 HTML 文件
gulp-babel：使用 babel 编译 JS 文件
gulp-jshint：jshint 检查
gulp-imagemin：压缩 jpg、png、gif 等图片
gulp-livereload：当代码变化时，它可以帮我们自动刷新页面
```


#### 也推荐几个 webpack 常用的 loader 和 plugin：

Loader 列表
```
less-loader, sass-loader：处理样式
url-loader, file-loader：两个都必须用上。否则超过大小限制的图片无法生成到目标文件夹中
babel-loader, babel-preset-es2015, babel-preset-react：js 处理，转码
expose-loader： 将 js 模块暴露到全局
```

Plugin 列表
```
NormalModuleReplacementPlugin：匹配 resourceRegExp，替换为 newResource
ContextReplacementPlugin：替换上下文的插件
IgnorePlugin：不打包匹配文件
PrefetchPlugin：预加载的插件，提高性能
ResolverPlugin：替换上下文的插件
DedupePlugin：打包的时候删除重复或者相似的文件
MinChunkSizePlugin：把多个小模块进行合并，以减少文件的大小
LimitChunkCountPlugin：限制打包文件的个数
MinChunkSizePlugin：根据 chars 大小，如果小于设定的最小值，就合并这些小模块，以减少文件的大小
OccurrenceOrderPlugin：根据模块调用次数，给模块分配 ids，常被调用的 ids 分配更短的 id，使得 ids 可预测，降低文件大小，该模块推荐使用
UglifyJsPlugin：压缩 js
CommonsChunkPlugin：多个 html 共用一个 js 文件(chunk)
HotModuleReplacementPlugin：模块热替换么，如果不在 dev-server 模式下，需要记录数据，recordPath，生成每个模块的热更新模块
ProgressPlugin：编译进度
NoErrorsPlugin：报错但不退出 webpack 进程
HtmlWebpackPlugin：生成 html
``` -->
