title: Vue SSR Nuxt
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
**服务器端渲染(Server-Side Rendering)：**
Vue.js 是构建客户端应用程序的框架。默认情况下，可以在浏览器中输出 Vue 组件，进行生成 DOM 和操作 DOM。然而，也可以将同一个组件渲染为服务器端的 HTML 字符串，将它们直接发送到浏览器，最后将静态标记"混合"为客户端上完全交互的应用程序。
服务器渲染的 Vue.js 应用程序也可以被认为是"同构"或"通用"，因为应用程序的大部分代码都可以在服务器和客户端上运行。
<!-- more -->

# 工作原理
![工作原理](https://user-gold-cdn.xitu.io/2018/5/24/16390509c83aef03?imageView2/0/w/1280/h/960/format/webp/ignore-error/1 "")

# 模板生成
## 安装
### 开发环境
``` javascript
Node
vue-cli
```

### 初始化项目
``` javascript
$ vue init nuxt-community/starter-template <project-name>
```

### 安装包
``` javascript
$ cd <project-name>
$ npm install
```

### 启动项目
``` javascript
$ npm run dev
```
## 模板结构
``` javascript
.
├── build/               # Webpack 配置目录
	├── dev-client.js            # npm run dev
	├── prod.js                  # npm run build
	├── webpack.base.conf.js     # 基础文件 求取路径原型函数
	├── webpack.dev.conf.js      # dev执行
	└── webpack.prod.conf.js     # build执行
├── dist/                # build 生成的生产环境下的项目
├── src/                 # 源码目录（开发都在这里进行）
	├── assets/            # 放置需要经由 Webpack 处理的静态文件（ASSET）
		├── css/            		 # css 文件夹
		├── img/            		 # img 文件夹
		├── less/            		 # less 文件夹
		└── scss/            		 # scss 文件夹
	├── components/        # 组件（COMPONENT）
		├── App.vue                  # 根组件
		├── Breadcrumb.vue           # 面包屑
		├── Navbar.vue               # 顶部导航栏
		├── Pagination.vue           # 分页
		├── Select/                  # 下拉框选择框组件
			├── LimitSelect.vue            # “每页显示多少条记录” 下拉选择框
			└── Select2.vue                # 对 Select2 的封装
		├── Sidebar/                 # 侧边栏组件
			├── index.vue                  # 侧边栏
			└── Link.vue                   # 导航链接封装
	├── filters/           # 过滤器（FILTER）
	├── mixins/            # （MIXIN）
	├── routes/            # 路由（ROUTE）
	├── services/          # 服务（SERVICE，统一管理 XHR 请求）
	├── utils/             # 工具类（UTIL）
	├── views/             # 路由页面组件（VIEW）
		├── index.vue                # 首页
		├── auth/                    # 用户认证模块ø
			├── login.vue                  # 登录页
			└── logout.vue                 # 注销登录页
		└── msg/                     # 留言板模块
			├── index.vue                  # 对应 /msg（留言板首页，alias => /msg/list）
			├── list.vue                   # 对应 /msg/list（留言板列表）
			├── add.vue                    # 对应 /msg/add（新增留言）
			├── detail.vue                 # 对应 /msg/detail/:msgId（查看留言）
			├── update.vue                 # 对应 /msg/update/:msgId（修改留言）
			├── _components/               # 留言板模块共用组件
			│   ├── AuthorSelect.vue             # 留言发布者选择下拉框
			│   ├── MsgForm.vue                  # 留言表单
			│   └── OptBtnGroup.vue              # 留言操作按钮组
			└── _mixins/                   # 留言板模块共用 mixins
				└── autoLoadByParams.js            # 根据 $route.params.msgId 自动加载
	└── app.js             # 启动文件
├── static/              # 放置无需经由 Webpack 处理的静态文件
├── index.html           # 静态基页
├── .babelrc             # Babel 转码配置
├── .eslintignore        # （配置）ESLint 检查中需忽略的文件（夹）
├── .eslintrc            # ESLint 配置
├── .gitignore           # （配置）需被 Git 忽略的文件（夹）
└── package.json         #
```

## 
``` javascript
```