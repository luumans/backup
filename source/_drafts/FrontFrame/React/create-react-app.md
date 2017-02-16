title: React单页面应用环境
date: 2017-02-15 18:29:00
description: 
categories:
- React
tags:
- React
toc: true
author: 
comments:
original:
permalink: 
---
### 创建react app
facebook 退出的单页面react环境，但是还是有些喽，没有sass等等的插件。不过还是很好的，很适合初学者。
<!-- more -->
[create-react-app](https://github.com/facebookincubator/create-react-app "npm page")

### 安装插件

```
$ npm install -g create-react-app
```

### 创建Demo

```
$ create-react-app my-app
$ cd my-app
```

### 项目结构
```
my-app/
  README.md
  node_modules/
  package.json
  .gitignore
  public/
    favicon.ico
    index.html
  src/
    App.css
    App.js
    App.test.js
    index.css
    index.js
    logo.svg
```

### 命名

npm run start 运行开发环境

npm run build 打包项目

npm run test 运行测试监测环境
npm test -- --coverage 生成覆盖报告

npm run eject 编辑环境配置项


### 构建项目
- [react-project](https://github.com/wdjungst/react-project#lazy "")
- [面向React+Redux+Webpack的单项目多应用脚手架](https://github.com/wxyyxc1992/Webpack2-React-Redux-Boilerplate "")
- []( "")
- []( "")
- []( "")