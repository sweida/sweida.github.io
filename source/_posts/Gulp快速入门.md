---
title: Gulp快速入门
abbrlink: 23049
date: 2018-06-22 01:27:40
categories: 工具
tags:
  - Glup
img: archives/23049/gulp.jpg  
---
## 全局安装
```
npm install glup -g
```

## 生成package.json文件
```
npm init
# 一路回车
```

## 本地安装gulp
```
npm install glup --save-dev
```

## 安装插件
本示例以gulp-less为例（编译less文件），命令提示符执行
```
npm install gulp-less --save-dev
```

## 新建gulpfile.js文件（重要）
gulpfile.js是gulp项目的配置文件，是位于项目根目录的普通js文件
```js
//导入工具包 require('node_modules里对应模块')
var gulp = require('gulp'), //本地安装gulp所用到的地方
    less = require('gulp-less');

//定义一个testLess任务（自定义任务名称）
gulp.task('testLess', function () {
    gulp.src('src/less/index.less') //该任务针对的文件
        .pipe(less()) //该任务调用的模块
        .pipe(gulp.dest('src/css')); //将会在src/css下生成index.css
});

gulp.task('default',['testLess']); //定义默认任务

//gulp.task(name[, deps], fn) 定义任务  name：任务名称 deps：依赖任务名称 fn：回调函数
//gulp.src(globs[, options]) 执行任务处理的文件  globs：处理的文件路径(字符串或者字符串数组)
//gulp.dest(path[, options]) 处理完后文件生成路径
```

## 运行gulp
编译less：命令提示符执行:
```
gulp testLess
```
* 当执行`gulp default`或`gulp`将会调用default任务里的所有任务['testLess']

## gulp-htmlmin
使用gulp-htmlmin压缩html，可以压缩页面javascript、css，去除页面空格、注释，删除多余属性等操作。
```js
var gulp = require('gulp'), //本地安装gulp所用到的地方
    htmlmin = require('gulp-htmlmin');

gulp.task('testHtmlmin', function () {
    var options = {
        removeComments: true,//清除HTML注释
        collapseWhitespace: true,//压缩HTML
        collapseBooleanAttributes: true,//省略布尔属性的值 <input checked="true"/> ==> <input />
        removeEmptyAttributes: true,//删除所有空格作属性值 <input id="" /> ==> <input />
        removeScriptTypeAttributes: true,//删除<script>的type="text/javascript"
        removeStyleLinkTypeAttributes: true,//删除<style>和<link>的type="text/css"
        minifyJS: true,//压缩页面JS
        minifyCSS: true//压缩页面CSS
    };
    gulp.src('src/*.html')
        .pipe(htmlmin(options))
        .pipe(gulp.dest('dist'));
});
gulp.task('default',['testHtmlmin']);
```

## gulp-clean-css
```js
// 压缩css
gulp.task('testCssmin', function () {
    gulp.src('src/css/*.css')
        .pipe(cssver()) //给css文件里引用文件加版本号（文件MD5）
        .pipe(cleanCSS())
        .pipe(gulp.dest('dist/css'));
});
```
