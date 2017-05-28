title: Vue Github实战项目搭建总结
date: 2017-05-25 18:29:00
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
# 环境搭建

## 技术栈
> vue
vue-router
vuex
axios
progressive-image
es6
webpack
script-loader
Eslint

<!-- more -->
## 项目结构
```
.
├── build/               # Webpack 配置目录
	├── dev-client.js            # npm run dev
	├── prod.js                  # npm run build
	├── webpack.base.conf.js     # 基础配置 全局路径原型函数
	├── webpack.dev.conf.js      # dev执行
	└── webpack.prod.conf.js     # build执行
├── config/               # Webpack 配置目录
	├── index.js                   # 端口与路径配置
├── dist/                # build 生成的生产环境下的项目
├── src/                 # 源码目录（开发都在这里进行）
	├── assets/            # （ASSET）放置需要经由 Webpack 处理的静态文件
		├── css/            		 # css 文件夹
		├── img/            		 # img 文件夹
		├── less/            		 # less 文件夹
		└── scss/            		 # scss 文件夹
	├── components/        # （COMPONENT）组件
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
	├── filters/           # （FILTER）过滤器
	├── mixins/            # （MIXIN）
	├── routes/            # （ROUTE）路由
	├── apis/              # （API，统一管理 XHR 请求）服务
	├── utils/             # （UTIL）工具类
      ├── flexible.js                # flexible 布局
      ├── common.js                  # 通用方法
      ├── tool.js                    # 工具方法
  ├── vuexs/             # （VUEX）状态管理
	├── views/             # （VIEW）路由页面
		├── index.vue                # 首页
		├── auth/                    # 用户认证模块
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
				└── autoLoadByParams.js          # 根据 $route.params.msgId 自动加载
	├── App.vue            # 根页面
	└── main.js            # 启动文件
├── static/              # 放置无需经由 Webpack 处理的静态文件
├── index.html           # 静态基页
├── .babelrc             # Babel 转码配置
├── .eslintignore        # （配置）ESLint 检查中需忽略的文件（夹）
├── .eslintrc            # ESLint 配置
├── .gitignore           # （配置）需被 Git 忽略的文件（夹）
└── package.json         #
```
### 全局路径
浏览很多项目的项目结构，配置全局路径，无需关注目录结构直接写。
```
resolve: {
  extensions: ['.js', '.vue', '.json'],
  alias: {
    'vue$': 'vue/dist/vue.esm.js',
    '@': resolve('src'),
    'SRC': path.resolve(__dirname, '../src'),
    'UTIL': path.resolve(__dirname, '../src/utils'),
    'API': path.resolve(__dirname, '../src/apis'),
    'ROUTE': path.resolve(__dirname, '../src/routes'),
    'FILTER': path.resolve(__dirname, '../src/filters'),
    'MIXIN': path.resolve(__dirname, '../src/mixins'),
    'ASSET': path.resolve(__dirname, '../src/assets'),
    'VIEW': path.resolve(__dirname, '../src/views'),
    'VUEX': path.resolve(__dirname, '../src/vuexs'),
    'COMPONENT': path.resolve(__dirname, '../src/components')
  }
},
```

## index.html
```
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <title>Vue</title>
    <meta charset="utf-8">
    <!-- 介绍 -->
    <meta name="Keywords" content="" />
    <meta name="description" content=""/>
    <meta name="author" content="">
    <!-- 清除cache -->
    <meta http-equiv="Expires" content="-1">
    <meta http-equiv="Cache-Control" content="no-cache">
    <meta http-equiv="Pragma" content="no-cache">
    <!-- icon -->
    <link rel="icon" href="favicon.ico">
    <!-- 移动端 -->
    <!-- <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no, minimum-scale=1.0, maximum-scale=1.0"> -->
    <meta name="format-detection" content="email=no">
    <meta name="format-detection" content="telephone=no">
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black">
    <link rel="apple-touch-icon" href="">
</head>
<body>
    <div id="app"></div>
<script>
    var _hmt = _hmt || [];
    (function() {
        var hm = document.createElement("script");
        hm.src = "//hm.baidu.com/hm.js?819b1c6493df653afb8c7846bc4b8db6";
        var s = document.getElementsByTagName("script")[0]; 
        s.parentNode.insertBefore(hm, s);
    })();
</script>
</body>
</html>
```

## main.js
引入IconFont、Flexible、vue-lazyload、vuex
```
require('!!script-loader!ASSET/fonts/iconfont')
import Vue from 'vue'
import App from './app'
import router from './routers'
import 'UTIL/flexible'

import VueLazyload from 'vue-lazyload'
import loading from 'ASSET/img/loading.png'
import error from 'ASSET/img/error.png'
Vue.use(VueLazyload, {
  preLoad: 1.3,
  error: error,
  loading: loading,
  attempt: 1,
  listenEvents: [ 'scroll', 'mousewheel' ]
})

import store from './vuex/store'

/* eslint-disable no-new */
new Vue({
  el: '#app',
  router,
  store,
  template: '<App/>',
  components: { App }
})
```

## routers
路由使用H5 history，
```
import Vue from 'vue'
import Router from 'vue-router'

import Hello from 'VIEW/hello/hello'
import Active from 'VIEW/active/active'

Vue.use(Router)

const scrollBehavior = (to, from, savedPosition) => {
  if (savedPosition) {
    return savedPosition
  } else {
    return { x: 0, y: 0 }
  }
}

export default new Router({
  mode: 'history',
  linkActiveClass: 'active',
  scrollBehavior,
  routes: [
    {
      path: '/',
      name: 'Active',
      component: Active,
      children: [
      ]
    },
    {
      path: '/Tap/:id',
      name: 'Tap',
      component: Tap,
      children: [
        {
          path: 'proImg',
          name: 'proImg',
          component: proImg
        },
        {
          path: 'Button',
          name: 'Button',
          component: Button
        },
        {
          path: 'Github',
          name: 'Github',
          component: Github
        }
      ]
    },
    {
      path: '/404',
      name: 'Error',
      component: Error
    },
    {
      path: '*',
      redirect: '/404'
    }
  ]
})
```

## API
```
import API from 'API'

API.Login(username, reponame).then(res => {
  console.log(res)
}, err => {
  console.log(err)
})
```

### axios
```
import axios from 'axios'

const TOKEN = 'f92b04fe9414ee3a91d0e1a697fd8ce12fc9878f'

import qs from 'qs'
import * as Tool from 'UTIL/tool'
// axios 配置
axios.defaults.timeout = 5000
axios.defaults.baseURL = 'https://api.github.com'
axios.defaults.headers.common['Authorization'] = `token ${TOKEN}`
axios.defaults.headers.post['Content-Type'] = 'application/x-www-form-urlencoded'

// POST传参序列化
axios.interceptors.request.use((config) => {
  if (config.method === 'post') {
    config.data = qs.stringify(config.data)
  }
  console.log(config.data)
  return config
}, (error) => {
  Tool.toast('错误的传参', 'fail')
  return Promise.reject(error)
})

// 返回状态判断
axios.interceptors.response.use((res) => {
  console.log(res)
  if (!res.data.success) {
    Tool.toast(res.data.msg)
    return res
  }
  return res
}, (error) => {
  Tool.toast('网络异常', 'fail')
  return Promise.reject(error)
})

export const oGet = (url, params) => {
  return new Promise((resolve, reject) => {
    axios.get(url, params)
      .then(res => {
        resolve(res.data)
      }, err => {
        reject(err)
      })
      .catch(err => {
        reject(err)
      })
  })
}

export const oPost = (url, params) => {
  return new Promise((resolve, reject) => {
    axios.post(url, params)
      .then(res => {
        resolve(res.data)
      }, err => {
        reject(err)
      })
      .catch(err => {
        reject(err)
      })
  })
}

export default {
  ReposList (username) {
    // return oPost('/activity/vote/activityVote', {cityId: 1, appType: 17600662355})
    // return oGet('/activity/vote/activityVote', {cityId: 1, appType: 17600662355})
    return oGet(`/users/${username}/repos`)
  },
  Login (username, reponame) {
    return oGet(`/repos/${username}/${reponame}`)
  }
}
```

### 伪数据
```
import * as repos from './tempdata/repos'
export const setpromise = data => {
  return new Promise((resolve, reject) => {
    resolve(data)
  })
}
var Login = (username) => setpromise(repos.List)
var ReposList = (username) => setpromise(repos.List)

export default {
  Login,
  ReposList
}
```

# 性能优化

## webpack-bundle-analyzer
最新`Vue-cli`还帮着注入了`webpack-bundle-analyzer`插件（Webpack插件和CLI实用程序），她可以将内容束展示为方便交互的直观树状图，让你明白你所构建包中真正引入的内容；我们可以借助她，发现它大体有哪些模块组成，找到错误的模块，然后优化它。我们可以在`package.json`中注入如下命令去方便运行她`npm run analyz`，默认会打开`http://127.0.0.1:8888`作为展示。
```
"analyz": "NODE_ENV=production npm_config_report=true npm run build"
```

# 开发

## nav
```
{
  path: '/Github',
  name: 'Github',
  component: Github,
  children: [
    {
      path: 'Novelty',
      name: 'Novelty',
      component: Novelty
    },
    {
      path: 'Repos',
      name: 'Repos',
      component: Repos
    },
    {
      path: 'Owner',
      name: 'Owner',
      component: Owner
    },
    {
      path: 'Follow',
      name: 'Follow',
      component: Follow
    }
  ]
},
```
使用router-link的路由状态获取active

# 问题解决

## favicon.ico
把favicon.ico放在项目的根目录下（不是src中，是最外面） 
然后在build/webpack.dev.conf.js中找到：
```
new HtmlWebpackPlugin({
  filename: config.build.index,
  template: 'index.html',
  favicon: 'favicon.ico', 在此处添加一行这个，用于webpack生成index.html时，自动把favicon.ico加入HTML中
  inject: true,
  minify: {
    removeComments: true,
    collapseWhitespace: true,
    removeAttributeQuotes: true
    chunksSortMode: 'dependency'
}),
```
同理，在build/webpack.prod.conf.js中也这么干。

# 拓展
- [如何写一手漂亮的 Vue](http://jeffjade.com/2017/03/11/120-how-to-write-vue-better/?from=jianshu# "")
- [Vue.js 插件开发详解](https://zhuanlan.zhihu.com/p/26057542 "")

## vue eslint
- [vue eslint](http://www.cnblogs.com/hahazexia/p/6393212.html "")

## vue axios
- [axios](https://www.npmjs.com/package/axios "")
- []( "")
- []( "")
- []( "")
- []( "")
- []( "")




