title: 如何将 Nuxt 应用部署至 Heroku？
date: 2018-08-25 18:29:00
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
**服务器端渲染(Server-Side Rendering)：**Vue.js是构建客户端应用程序的框架。默认情况下，可以在浏览器中输出Vue组件，进行生成DOM和操作DOM。然而，也可以将同一个组件渲染为服务器端的HTML字符串，将它们直接发送到浏览器，最后将静态标记"混合"为客户端上完全交互的应用程序。
服务器渲染的Vue.js应用程序也可以被认为是"同构"或"通用"，因为应用程序的大部分代码都可以在服务器和客户端上运行。
<!-- more -->

# [注册](https://signup.heroku.com/dc "")
注意：注册不支持QQ邮箱

# 安装
[set-up](https://devcenter.heroku.com/articles/getting-started-with-nodejs#set-up )

## Windows

## macOS

``` node
$ brew install heroku/brew/heroku
```

## Ubuntu 16+

``` node
$ sudo snap install heroku --classic
```

# 登录

``` node
$ heroku login
	Enter your Heroku credentials.
	Email: <user@example.com>
	Password: <Password>
```

## 查看版本

``` node
$ node -v
$ npm -v
```

## 配置package.json

``` node
"scripts": {
  "heroku-postbuild": "npm run build"
},
"engines": {
  "node": "8.9.0",
  "npm": "5.5.1"
},
```

## 部署应用程序

### 创建APP

``` node
$ heroku create
Creating sharp-rain-871... done, stack is cedar-14
http://sharp-rain-871.herokuapp.com/ | https://git.heroku.com/sharp-rain-871.git
Git remote heroku added
```
注意：默认会自动创建`sharp-rain-`开头的名称，也可以指定名称

``` node
$ heroku create <name>
Creating <name>... done, stack is cedar-14
http://<name>.herokuapp.com/ | https://git.heroku.com/<name>.git
Git remote heroku added
```

### 设置

``` node
$ heroku config:set NPM_CONFIG_PRODUCTION=false
```

### 主机IP

``` node
$ heroku config:set HOST=0.0.0.0
$ heroku config:set NODE_ENV=production
```

## 部署代码

``` node
$ git push heroku master
```

## 打开部署页面

``` node
$ heroku open
```

