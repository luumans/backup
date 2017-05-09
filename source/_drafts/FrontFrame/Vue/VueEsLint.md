title: Vuex
date: 2017-04-25 18:29:00
description: 
categories:
- FrontFrame
tags:
- ESLint
toc: true
author:
comments:
original:
permalink: 
---
　　**Vuex**是一个专为 Vue.js 应用程序开发的状态管理模式。它采用集中式存储管理应用的所有组件的状态，并以相应的规则保证状态以一种可预测的方式发生变化。
<!-- more -->
ESLint 一个 JavaScript 代码的静态检测工具，相比之前比较流行的 JSHint，ESLint 扩展性强、配置灵活、支持 ES6 …

[ESLint 官网](http://eslint.org/docs/user-guide/configuring#using-configuration-files)
[ESLint 中文](http://eslint.cn/ )
[给vue项目添加ESLint](http://www.cnblogs.com/hahazexia/p/6393212.html)
参考官网安装 eslint 就可以在命令行检测 js 文件的语法错误

# 编辑器
在实际开发中，更多是配合编辑器（Sublime Text）一起使用，在编写代码的时候使用 eslint 实时检测代码，并且提醒错误的部分，下面展示安装使用过程

## 安装ESLint Node 模块
```
npm install -g eslint
```

## 安装 Sublime Text 插件
```
SublimeLinter 安装指南
SublimeLinter-contrib-eslint 安装指南
SublimeLinter 是一个 代码检测基础框架，当需要具体检测方案则要安装对应的库，例如需要 eslint 监测则安装 SublimeLinter-contrib-eslint
```

## 创建配置文件

在项目根目录下创建 .eslintrc 文件
```
{
  "root": true,
  "rules": {
    "eqeqeq": "error"
  }
}
```
其中的 "eqeqeq": "warn" 规则表明，如果代码中出现 == !=来比较则会出现错误提醒，建议使用 === !==

## 具体参考
[配置指南]( "")
[配置指南（中文）]( "")
这样，写代码的时候就能实时检测错误并且提醒了：

# Vue 的 ESLint

.eslintignore 文件
```
flow
dist
packages
```
表明 eslint 检测时，要忽略掉这些目录

.eslintrc 文件
```
{
  "root": true,
  "parser": "babel-eslint",
  "extends": "vue",
  "plugins": ["flow-vars"],
  "rules": {
    "flow-vars/define-flow-type": 1,
    "flow-vars/use-flow-type": 1
  }
}
```
下面逐个配置解释：
```
"root": true
```
对于某个文件使用哪个配置文件，按照以下顺序查找

在待检测文件的同一目录查找配置文件
往上逐层父级目录查找，直到发现一个有 "root": true 的
使用项目根目录配置文件
使用系统全局配置文件
```
"parser": "babel-eslint"
```
使用非默认的 babel-eslint 作为代码解析器，同时你需要安装相应 node 模块

```
npm install -save-dev babel-eslint
```
这样 eslint 就能识别 babel 语法的代码
```
"extends": "vue"
```
官方或者第三方提供了一些配置模板，你只需继承则可以使用他们的模板配置，这里继承了 vue 意味着你需要安装 eslint-config-vue 这个 node 模块

```
npm install -save-dev eslint-config-vue
"plugins": ["flow-vars"]
```
让 eslint 支持 Flow Script 的全局注解等语法，同时你也要安装对应的 node 模块

```
npm install -save-dev eslint-plugin-flow-vars
"rules":{xx}
```
一些主要的配置都在 "extends": "vue" 解决了，flow 插件部分的配置则在这里另外定义

# 总结
使用 ESLint 保持团队编码风格统一，减少低级错误，真的很赞！

