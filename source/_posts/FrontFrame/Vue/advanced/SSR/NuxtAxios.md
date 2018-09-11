title: Vue SSR Nuxt axios封装
date: 2018-08-25 18:29:00
description:
categories:
- FrontFrame
tags:
- Nuxt
toc: true
author:
comments:
original:
permalink:
---
<!-- more -->

# 安装

``` node
$ npm install axios --save
```

# 使用
## ~/nuxt.config.js
引入插件，启动中间件

``` javascript
plugins: ['~/plugins/api.js'],
```

## ~/plugins/api.js
``` javascript
import Vue from 'vue'
import API from '~/api/index.js'

Vue.prototype.$API = API
Vue.use(API)
```

## ~/api/index.js
``` javascript
/**
 * api接口统一管理
 * import API from 'API'
 * Vue.prototype.$API = API
 * Vue.use(API)
 *
 * this.$API.Login()
 */
import {get, post} from './http'

export default {
  POST (link) {
    return post(link)
  },
  GET (link) {
    return get(link)
  }
}
```

## ~/api/http.js
创建本地语言库

``` javascript
import axios from 'axios' // 引入axios
import qs from 'qs' // 引入qs模块，用来序列化post类型的数据，后面会提到
import {
  baseUrl
} from './env.js'

const TOKEN = '7bf2b13020e1ed2278db4bba3f5e7a53102cbc37'

// vuex
// import * as Tool from 'UTIL/vuex'

// axios 配置
axios.defaults.timeout = 5000 // 设置请求超时
axios.defaults.baseURL = baseUrl // 默认请求地址
axios.defaults.headers.common['Authorization'] = `token ${TOKEN}` // Authorization
axios.defaults.headers.post['Content-Type'] = 'application/x-www-form-urlencoded' // 请求头的设置

// 请求
axios.interceptors.request.use((config) => {
  if (config.method === 'post') {
    config.data = qs.stringify(config.data)
  }
  let URL = config.url.split(config.baseURL)
  // Tool.open(URL[1], config.showLoading)
  return config
}, (error) => {
  // Tool.toast('错误的传参', 'fail')
  return Promise.reject(error)
})

// 返回
axios.interceptors.response.use((res) => {
  // console.log(res)
  // 拦截器配置
  // if (res.data.success) {
  //   Tool.toast(res.data.msg)
  //   Tool.close()
  //   return Promise.reject(res)
  // }
  // Tool.close()
  // return res // 全部数据
  return res.data // data数据
}, (error) => {
  // 请求失败
  // Tool.toast('网络异常', 'fail')
  // Tool.close()
  return Promise.reject(error)
})

export const get = (url, showLoading) => axios.get(url, {
  showLoading: showLoading
})

export const post = (url, params, showLoading) => axios.post(url, params, {
  showLoading: showLoading
})
```

## ~/api/env.js
更加不同页面添加不用的语言

``` javascript
/**
 * 配置编译环境和线上环境之间的切换
 *
 * baseUrl: 域名地址
 * routerMode: 路由模式
 * imgBaseUrl: 图片所在域名地址
 *
 */

let baseUrl
let routerMode
const imgBaseUrl = 'https://fuss10.elemecdn.com'

if (process.env.NODE_ENV === 'development') {
  baseUrl = 'https://api.github.com/'
  routerMode = 'hash'
} else {
  baseUrl = 'https://api.github.com/'
  routerMode = 'hash'
}

export {
  baseUrl,
  routerMode,
  imgBaseUrl
}

```

## ~/store/index.js
``` javascript
import Locale from '~/locales'

export const state = () => ({
  locales: Locale(),
  locale: Locale()[0]
})

export const mutations = {
  SET_LANG(state, locale) {
    if (state.locales.indexOf(locale) !== -1) {
	  	console.log(locale)
      state.locale = locale
    }
  }
}
```

# 方法

## POST
``` javascript
this.$API.POST('').then((res) => {
  console.log(res)
})
```

## GET
``` javascript
this.$API.GET('').then((res) => {
  console.log(res)
})
```