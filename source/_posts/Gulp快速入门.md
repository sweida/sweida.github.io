---
title: Gulp快速入门
abbrlink: 23049
date: 2018-06-22 01:27:40
categories: 工具
tags:
  - Gulp
comments: true
img: archives/23049/gulp.jpg  
---

![gulp](23049/001.jpg)
gulp是前端开发过程中对代码进行构建的工具，是自动化项目的构建利器；她不仅能对网站资源进行优化，而且在开发过程中很多重复的任务能够使用正确的工具自动完成；使用她，我们不仅可以很愉快的编写代码，而且大大提高我们的工作效率。

gulp是基于Nodejs的自动任务运行器， 她能自动化地完成 javascript/sass/less/html/image/css 等文件的的测试、检查、合并、压缩、格式化、浏览器自动刷新、部署文件生成，并监听文件在改动后重复指定的这些步骤。在实现上，她借鉴了Unix操作系统的管道（pipe）思想，前一级的输出，直接变成后一级的输入，使得在操作上非常简单。

gulp 和 grunt 非常类似，但相比于 grunt 的频繁 IO 操作，gulp 的流操作，能更快地更便捷地完成构建工作。
### 全局安装
```
npm install glup -g
```

### 生成package.json文件
```
npm init    # 一路回车
```

### 本地安装gulp
```
npm install glup --save-dev
```

### 安装插件
```bash
# 比较常用的插件
npm install --save-dev gulp-concat       # （合并文件）
npm install --save-dev gulp-copy         # （复制文件）
npm install --save-dev gulp-clean-css    # （压缩css）
npm install --save-dev gulp-less         # （编译less文件）
npm install --save-dev gulp-htmlmin      # （压缩html）
npm install --save-dev gulp-uglify       # （压缩js）
npm install --save-dev gulp-useref       # （修改html引用css、js路径）
npm install --save-dev gulp-livereload   # （自动刷新）
```

### 打包一个项目的基本配置
新建gulpfile.js文件（重要），gulpfile.js是gulp项目的配置文件，是位于项目根目录的普通js文件
```js
//导入工具包 require('node_modules里对应模块')
var gulp        = require('gulp');
var less        = require('gulp-less');
var htmlmin     = require('gulp-htmlmin');
var cleanCSS    = require('gulp-clean-css');
var uglify      = require('gulp-uglify');
var concat      = require('gulp-concat');
var useref      = require('gulp-useref');
var copy        = require('gulp-copy');

// 压缩html
gulp.task('testHtmlmin', ['testUseref'], function () {  // 先修改引用路径，再压缩
    var options = {
        removeComments: true,                 //清除HTML注释
        collapseWhitespace: true,             //压缩HTML
        collapseBooleanAttributes: true,      //省略布尔属性的值 <input checked="true"/> ==> <input />
        removeEmptyAttributes: true,          //删除所有空格作属性值 <input id="" /> ==> <input />
        removeScriptTypeAttributes: true,     //删除<script>的type="text/javascript"
        removeStyleLinkTypeAttributes: true,  //删除<style>和<link>的type="text/css"
        minifyJS: true,                       //压缩页面JS
        minifyCSS: true                       //压缩页面CSS
    };
    return gulp.src('dist/*.html')
        .pipe(htmlmin(options))
        .pipe(gulp.dest('dist'));
});

// 复制图片
gulp.task('copy', function () {
    return gulp.src('src/images/**/*')
        .pipe(gulp.dest('dist/images'));
});

// 编译less
gulp.task('testLess', function () {
    return gulp.src('src/less/*.less')  //该任务针对的文件
        .pipe(less())                   //该任务调用的模块
        .pipe(gulp.dest('src/css'));    //将会在src/css下生成index.css
});

// 压缩css
gulp.task('testCssmin', ['testLess'], function () { // 先执行testLess任务后再执行cssmin任务，必须加上rutrun才能异步
    return gulp.src('src/css/*.css')
        .pipe(concat('libs.min.css'))    // 先合并css后再压缩
        .pipe(cleanCSS())
        .pipe(gulp.dest('dist/css'));
});

// 压缩js
gulp.task('jsmin', function () {
    return gulp.src('src/js/*.js')
        .pipe(concat('libs.min.js'))    // 先合并js后再压缩
        .pipe(uglify())
        .pipe(gulp.dest('dist/js'));
});

// 修改html的引用地址
gulp.task('testUseref', function () {
    return gulp.src('src/*.html')
        .pipe(useref())
        .pipe(gulp.dest('dist'));
});

// watch暂时先不启动

// // watch监听less文件 自动执行less编辑后压缩
// gulp.task('watchLess', function () {
//     gulp.watch('src/less/*.less', ['testCssmin']);
// });
//
// // watch监听js，有更改就输出log
// gulp.task('watchJs', function () {
//     gulp.watch('src/js/*.js', function (event) {
//         console.log('File ' + event.path + ' was ' + event.type + ', running tasks...');
//     });
// });

// 执行定义任务
gulp.task('default',[
  'copy',
  'jsmin',
  'testCssmin',
  'testHtmlmin'
  // 'watchLess',
  // 'watchJs'
]);
```

### 运行gulp
编译less：命令提示符执行:
```bash
# 全部运行
gulp
# 运行指定模块
gulp testLess
```

打包后生成的文件目录dist
![打包后目录](23049/002.png)

* 最后还差一步，引用css、js自动添加版本号，找了很多资料，没有一个能很好解决

### 项目地址： [https://github.com/sweida/gulp-demo](https://github.com/sweida/gulp-demo)
