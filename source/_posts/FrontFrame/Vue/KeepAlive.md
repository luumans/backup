title: keep-alive最佳实践
date: 2017-08-18 18:29:00
description:
categories:
- FrontFrame
tags:
- Vue
toc: true
author:
comments:
original:
permalink: 
---
　　**Vue 项目搭建：**文章适合有一定vue经验，只是简单介绍项目中的搭建与开发的优化之处。知识点，请自行查阅！
<!-- more -->

# 使用
## 属性

1. include - 字符串，或正则表达式，或数组。匹配的组件会被缓存。
1. exclude - 字符串，或正则表达式，或数组。匹配的组件不会被缓存。

## 生命周期
当组件在 <keep-alive> 内被切换，它的 activated 和 deactivated 这两个生命周期钩子函数将会被对应执行。

## 作用
主要用于保留组件状态或避免重新渲染。

```
<!-- comma-delimited string -->
<keep-alive include="a,b">
  <component :is="view"></component>
</keep-alive>

<!-- regex (use v-bind) -->
<keep-alive :include="/a|b/">
  <component :is="view"></component>
</keep-alive>

<!-- Array (use v-bind) -->
<keep-alive :include="['a', 'b']">
  <component :is="view"></component>
</keep-alive>
```
## 误解

1.直接添加在router-view上：
将所有页面全部缓存，这样的内存不会卡顿？如何制定那些缓存。无法使用过滤。
```
<keep-alive>
  <router-view></router-view>
</keep-alive>
```
1. 不适用与v-for的组件上，和没有组件的内容。
1. 

# 实例
子路由的内容缓存，切换路由，将组建缓存，通过data，判断是否重新渲染页面内容。
mounted => activated => deactivated => activated => deactivated
```
mounted () {
  // 加载数据
},
activated: function () {
  // 是否重新加载数据
  console.log('组件启动')
},
deactivated: function () {
  console.log('组件缓存')
}
```

- [关于router-view上使用keep-alive的问题](https://segmentfault.com/q/1010000006827156)
