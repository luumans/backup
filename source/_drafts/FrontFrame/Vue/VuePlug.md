title: Vue Plug
date: 2017-03-25 18:29:00
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
　　**自用笔记：**Vue.js通过简洁的API提供高效的数据绑定和灵活的组件系统。最近在Github上看到了不少Vue的项目，很好奇，决定尝试尝试。
<!-- more -->


# [v-tap](https://github.com/MeCKodo/vue-tap "")
## To use
```
npm install v-tap --save-dev

import vueTap from 'v-tap'
Vue.use(vueTap)
```

## To
```
v-tap={ methods : xxx , paramA : a,paramB:b}
// 链接跳转
<a href="http://www.baidu.com" v-tap>链接跳转</a>

注释：现在有个bug，只要配置了函数对象，就会不可以跳转。
// 无法滑动页面
<p v-tap.prevent="{ methods : scroll }">无法滑动页面</p>

// 事件处理
<a v-tap.prevent="{ methods : cant }" href="http://www.baidu.com">无法跳转的</a>

<a v-tap.prevent="{ methods : Link, path: 'http://www.baidu.com'}">传递参数</a>


Link (params) {
	// params 可获取绑定时候带的参数
	console.log(params.event); // 原生事件
	console.log(params.tapObj); // 手指触摸的一些参数
	console.log(params.paramA); // 绑定时候传入的paramA
	console.log(params.paramB); // 绑定时候传入的paramB
}

// 跳转不处理
<a href="aaa" v-tap="{ methods : cant }">can't</a>

<!-- <a v-tap="a++">v-tap="a++" 直接执行表达式在2.0里无法使用</a> -->

<a href="javascript:window.history.go(-1);" v-tap>我可以直接在href里写js代码 如history.go(-1)</a>
```

# [axios](https://github.com/mzabriskie/axios "")
## To use
```
npm install axios --save-dev

import axios from 'axios'
```
## get
```
axios.get('https://api.github.com/search/rpositories?q=javascript&sort=stars').then((response) => {
  console.log(response.data)
}, (response) => {
  console.log(response)
})
```

# [vue-axios](https://github.com/imcvampire/vue-axios "")
## To use
```
npm install axios vue-axios --save-dev

import axios from 'axios'
import VueAxios from 'vue-axios'

Vue.use(VueAxios, axios)
```

## get
```
this.axios.get(api).then((response) => {
  console.log(response.data)
})

this.$http.get(api).then((response) => {
  console.log(response.body)
})
```

# [vue-resource](https://github.com/pagekit/vue-resource "")
## To use
```
npm install vue-resource --save-dev

import VueResource from 'vue-resource'
Vue.use(VueResource)
```

## get
```
this.$http.get(api).then((response) => {
 console.log(response.body)
}, (response) => {
  console.log(response)
})
```

## post
```
this.$http.post(api, {'cityId': 1, 'cityId': 1}, {emulateJSON: true}).then((response) => {
 console.log(response.body)
}, (response) => {
  console.log(response)
})
```

# []( "")
## To use
```
npm install progressive-image --save

import progressive from 'progressive-image/dist/vue'
Vue.use(progressive, {
  removePreview: true
})
Vue.config.productionTip = false
```

## To
```
<img class="preview" :data-srcset="dVote.owner['avatar_url']" v-progressive="dVote.owner['avatar_url'] + '&s=10'" :src="dVote.owner['avatar_url']">

data-src
src
```

# [script-loader](https://github.com/webpack-contrib/script-loader "")
## To use
```
npm install script-loader --save-dev

require('!!script-loader!../static/plugins/flexible')
```

# [proxy代理配置](https://vuejs-templates.github.io/webpack/proxy.html "")
## To use
```
// config/index.js

proxyTable: {
  '/users': {
    target: 'https://api.github.com',
    changeOrigin: true,
    pathRewrite: {
      '^/users': '/users'
    }
  }
},
```

# [mock 伪数据]( "")
- [搭建vue+webpack+mock脚手架（一）](https://segmentfault.com/a/1190000008279215 "")
- [vue-cli 本地开发mock数据使用方法](http://www.jianshu.com/p/ccd53488a61b "")
- []( "")
- []( "")
- []( "")
## To use
```
npm install v-tap --save-dev

import vueTap from 'v-tap'
Vue.use(vueTap)
```

## To
```

```


# [vue images](http://littlewin.info/vue-images/example/ "")
## To use
```
// Install using npm
npm install vue-images --save

// In ES6 module
import vueImages from 'vue-images'
```

## To
```
<vue-images :imgs="images"
  :modalclose="modalclose"
  :keyinput="keyinput"
  :mousescroll="mousescroll"
  :showclosebutton="showclosebutton"
  :showcaption="showcaption"
  :imagecountseparator="imagecountseparator"
  :showimagecount="showimagecount"
  :showthumbnails="showthumbnails">
</vue-images>

images: [
  {
    imageUrl: 'http://image.yktour.com.cn/g1/M00/04/C2/CgAMClkQTPeAG9kDAAFQJRBoAkw230.jpg',
    caption: '<a href="#">Photo by 1</a>'
  }
],
```

# [vue-core-image-upload](http://vanthink-ued.github.io/vue-core-image-upload/index.html#/cn/home "")
## To use
```
npm install v-tap --save-dev

import vueTap from 'v-tap'
Vue.use(vueTap)
```

## To
```

```


# []( "")
## To use
```
npm install v-tap --save-dev

import vueTap from 'v-tap'
Vue.use(vueTap)
```

## To
```

```


