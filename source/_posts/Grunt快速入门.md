---
title: Grunt快速入门
date: 2018-06-21 09:16:25
abbrlink: 12322
categories: 工具
tags:
  - Grunt
img: archives/12322/001.png
---

![grunt](12322/000.png)
## 前言
各位web前端开发人员，如果你现在还不知道grunt或者听说过、但但是不会熟练使用grunt，那你就真的真的真的out了。还有一点，它完全免费，没有盗版。既强大又免费的东西，为何不用？

当然了，你如果你能找到更好的替代grunt的其他工具也是可以的，例如gulp。Gulp未来有可能替代grunt，但是现在来说市场占有率还是不如grunt。而这种工具咱们是现在就需要用的，所有不要再犹豫了，拿来主义，先用grunt，即学即用。

Grunt是一套前端自动化工具，一个基于nodeJs的命令行工具，最基本的用法有：
1. 压缩文件
2. 合并文件
3. 简单语法检查

## 安装 CLI
```
npm install -g grunt-cli
```
安装grunt-cli并不等于安装了 Grunt！Grunt CLI的任务很简单：调用与Gruntfile在同一目录中 Grunt。

> 开始使用grunt很简单，在项目的根目录中添加两份文件：package.json 和 Gruntfile.js。

```json
{
    "name": "grunt_demo",
    "version": "1.0.0",
    "description": "A grunt project",
    "author": "sweida <weida@ifm360.cn>",
    "devDependencies": {
    }
}
```
devDependencies是什么意思？字面意思解释是“开发依赖项”。即我们现在这个系统，将会依赖于哪些工具来开发。现在代码一行都没有写，依赖于谁？谁也不依赖！所以，就先写一个空对象。但是下文会慢慢把它填充起来。

## 安装Grunt
下面这条命令将安装Grunt最新版本到项目目录中，并将其添加到package.json内的devDependencies内：
```
npm install grunt --save-dev
```
安装后package.json
```json
{
    "name": "grunt_demo",
    "version": "1.0.0",
    "description": "A grunt project",
    "author": "sweida <weida@ifm360.cn>",
    "devDependencies": {
        "grunt": "^1.0.2"
    }
}
```

## 一般项目代码目录结构如下
![代码目录](12322/002.jpg)

## 插件
### 合并：grunt-contrib-concat
安装如下（下面插件同样方法）
```
npm install grunt-contrib-concat --save-dev
```
合并代码是我们最需要的一个功能了，当项目变大并且模块很多的时候，随便拿一个项目来说，index页面会有一列的js代码，如下图所示：
![js](12322/003.png)

我们需要将这些js合并为一个文件，大大减少网络请求数量因此来提升性能。grunt-contrib-concat完美胜任，下面我们来看看基本配置用法：
```js
// Gruntfile.js文件
module.exports = function(grunt) {
    // 任务配置，所有插件的配置信息
    grunt.initConfig({
        pkg: grunt.file.readJSON('package.json'),
        concat: {
            allInOne: {
                src: ['src/js/*.js'],
                dest: 'dest/js/<%= pkg.name %>.js'
            }
        }
    });
    // 告诉grunt 我们将要使用的插件
    grunt.loadNpmTasks('grunt-contrib-concat');

    // 告诉grunt 我们在终端输入grunt时需要做些什么（注意先后顺序）
    grunt.registerTask('default', ['concat']);
};
```
将src/js中所有js文件合并为一个js，放在dest/js目录下，js名字为package.json配置中项目name。这时候项目目录中就会出现一个dest的文件夹，如下所示：
![js合并后目录](12322/004.jpg)

### 压缩：grunt-contrib-uglify

合并文件后，体积仍然比较大，上线之前要将代码压缩，因此我们接着将上一步合并后的代码压缩，这里就需要用到grunt-contrib-uglify插件。
在使用grunt-contrib-uglify插件之前如果有用es6语法，需要将es6语法用babel转成es5才能正常压缩

```
# 安装压缩
npm install --save-dev grunt-contrib-uglify
# 安装babel
npm install --save-dev grunt-babel babel-preset-env babel-core
```

```js
module.exports = function(grunt) {
    grunt.initConfig({
        pkg: grunt.file.readJSON('package.json'),
        concat: {
            allOne: {
                src: ['src/js/*.js'],
                dest: 'dest/js/<%= pkg.name %>.js'
            }         
        },
        // es6转es5
        babel: {
            options: {
                sourceMap: true,
                presets: ['env']
            },
            dist: {
                files: [{
                    expand:true,
                    cwd:'dest/', //js目录下
                    src:['**/*.js'], //所有js文件
                    dest:'dest/'  //输出到此目录下
                }]
            }
        },
        uglify: {
            buildrelease: {
                options: {
                    report: "min" //输出压缩率
                },
                files: [{
                    expand: true,
                    cwd: 'dest/js', //js目录
                    src: '**/*.js', //所有js文件
                    dest: 'dest/js', //输出到此目录下
                    ext: '.min.js' //指定扩展名
                }]
            }
        }
    });
    // 告诉grunt 我们将要使用的插件
    grunt.loadNpmTasks('grunt-contrib-uglify');
    grunt.loadNpmTasks('grunt-babel');
    grunt.loadNpmTasks('grunt-contrib-concat');

    // 告诉grunt 我们在终端输入grunt时需要做些什么（注意先后顺序）
    grunt.registerTask('default', ['concat', 'babel', 'uglify:buildrelease']);
};
```
这里我将concat后的js文件仍然输出到当前目录dest/js下，多个了min.js，如下图所示：
![js压缩后目录](12322/005.jpg)

### 使用watch插件（真正实现自动化）
```
npm install grunt-contrib-watch --save-dev
```

```js
// 在上面基础上填充
module.exports = function(grunt) {
  grunt.initConfig({
    watch: {
      scripts: {
        files: ['src/js/*.js', 'src/css/*.css'],
        tasks: ['uglify'],
        options: {spawn: false}
      }
    }
  });

  grunt.loadNpmTasks('grunt-contrib-watch');

  grunt.registerTask('default', ['concat', 'babel', 'uglify:buildrelease', 'watch']);
}
```
watch插件配置完成。运行grunt命令，控制台提示watch已经开始监听。每次修改js、css文件就会自动运行，想要停止，按ctrl + c即可
![watch监听](12322/007.png)

### 引用替换：grunt-usemin（grunt-contrib-copy，grunt-contrib-clean）

前端优化是尽量减少http请求，所以我们需要尽量合并压缩文件，然后调用压缩后的文件，比如多个css文件压缩成一个，多个js文件合并压缩等，grunt-usemin能够自动在html中使用压缩后的文件，达到上面的目的。

因此首先我们需要用到grunt-contrib-copy插件，将源代码copy一份，然后在副本上进行压缩合并，这样无论是全部压缩还是部分压缩就比较灵活了，copy之后就可以使用grunt-usemin插件了，usemin是一个多任务插件，它包括两个任务，useminPrepare和usemin。

useminPrepare用来检测html页面中的脚本块，包括脚本文件的源路径，目的路径，从而更新后续需要使用到的Grunt任务的配置信息，如前面使用的concat，uglify。useminPrepare只是分析文件，获取文件及路径信息，不更新内容。

而usemin则进行文件引用替换，将源文件中的文件块替换为压缩文件。useminPrepare已经帮助我们自动配置了concat，uglify针对的源文件以及目的文件的路径信息,因此就无需再手动配置concat和uglify任务了。配置代码如下
![js引用](12322/008.png)
注意js需要`<!-- build:css css/App.min.css --><!-- endbuild -->`这样包裹着usemin才能起效
```js
module.exports = function(grunt) {
    grunt.initConfig({
        pkg: grunt.file.readJSON('package.json'),
        clean: {
            src: 'build/'
        },
        useminPrepare: {
            html: 'build/index.html',
            options: {
                dest: 'build'
            }
        },
        uglify: {
            buildrelease: {
                options: {
                    report: "min" //输出压缩率
                }
            }
        },
        usemin: {
            html: 'build/index.html',
            options: {
                dest: 'build'
            }
        },
        copy: {
            html: {
                files: [{
                    expand: true,
                    cwd: 'src',
                    src: '**/*',
                    dest: 'build/'
                }]
            }
        }
    });
    // 告诉grunt 我们将要使用的插件
    grunt.loadNpmTasks('grunt-contrib-concat');
    grunt.loadNpmTasks('grunt-contrib-uglify');
    grunt.loadNpmTasks('grunt-contrib-clean');
    grunt.loadNpmTasks('grunt-contrib-copy');
    grunt.loadNpmTasks('grunt-usemin');

    // 告诉grunt 我们在终端输入grunt时需要做些什么（注意先后顺序）
    grunt.registerTask('default', ['clean', 'copy', 'useminPrepare', 'concat', 'uglify', 'usemin']);
};
```

上面又引入了一个clean插件，每次构建时候先清除build目录，这样build目录就是我们打包后的要的结果了。目录结构如下：
![最后目录](12322/006.jpg)
