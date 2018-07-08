---
title: vue项目首屏优化之路由懒加载
abbrlink: 28920
date: 2018-07-08 23:54:32
categories: 前端
tags:
  - vue
comments: true
---

当打包构建应用时，Javascript 包会变得非常大，影响页面加载。如果我们能把不同路由对应的组件分割成不同的代码块，然后当路由被访问的时候才加载对应组件，这样就更加高效了。
结合 Vue 的异步组件和 Webpack 的代码分割功能，轻松实现路由组件的懒加载。
### 实现
* 首先，可以将异步组件定义为返回一个 Promise 的工厂函数
* 然后使用动态 import语法来定义代码分块点。
* 结合这两者，定义一个能够被 Webpack 自动代码分割的异步组件。

```js
// 第一种方法，需要配合babel的syntax-dynamic-import插件使用
const Foo = () => import('./Foo.vue')
// 第二种方法
const Foo = resolve => require(['./Foo.vue'], resolve)
```

在路由配置中什么都不需要改变，只需要像往常一样使用 Foo：
```js
const router = new VueRouter({
  routes: [
    { path: '/foo', component: Foo }
  ]
})
```

### 结合上面，路由配置如下
```js
// router 文件夹下的index.js文件
import Vue from 'vue'
improt Router from 'vue-router'
improt Foo from '@/pages/Foo' // 某页面

Vue.use(VueRouter)

// 非懒加载
/* const routes = [
    {
      path: '/',
      name: 'foo',
      component: foo
    }
] */
// 懒加载
const routes= [
  {
    path: '/',
    name: 'holly',
    componet: () => improt('@/pages/Holly') //其实就相当于按需加载
  }
]

const router = new Router({
    routes
})

export default router
```
这就是将不同路由对应的组件分割成不同的代码块，然后当路由被访问时才加载对应组件。

### main.js入口文件关于路由的配置
```js
improt router from './router' //引入路由配置文件

new Vue({
    el: '#app',
    router, // 注入到根实例中
    render: h => h(App)
})
```
