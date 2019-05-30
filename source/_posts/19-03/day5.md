---
title: vue的类库vux安装
tags: vue
categories: vue
date: 2019-03-01 15:14:55
---
![avatar](https://raw.githubusercontent.com/langhuonan/wechat/master/mdImages/day1/1.jpg)
### cli环境中安装vux类库

1. npm install vux --save

2. 安装vux-loader （这个vux文档似乎没介绍，当初没安装结果报了一堆错误） 
```
npm install vux-loader --save-dev
```
3. 安装less-loader  （这个是用以正确编译less源码，否则会出现 ' Cannot GET / '）
```
npm install less less-loader --save-dev
```


4. vux2必须配合vux-loader使用, 请在build/webpack.base.conf.js里参照如下代码进行配置：
<!--more-->
const vuxLoader = require('vux-loader')
const webpackConfig = originalConfig 将原来的 module.exports 代码赋值给变量 webpackConfig (将原文件的module.exports改为let webpackConfig) 
5. 在文件最后一行复制下面代码
```
module.exports = vuxLoader.merge(webpackConfig, {
        plugins: ['vux-ui']
}) 
```
6. 在入口文件全局引入
```
import Vue from 'vue'
import { XButton } from 'vux'
```
例：
```
Vue.component('x-button', XButton) 
```
使用
```
        <x-button></x-button> 
```
vux样式改变，可以在APP.vue中用样式覆盖，在组件内用scoped无法覆盖