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
# 概况
**服务器端渲染(Server-Side Rendering)：**Vue.js是构建客户端应用程序的框架。默认情况下，可以在浏览器中输出Vue组件，进行生成DOM和操作DOM。然而，也可以将同一个组件渲染为服务器端的HTML字符串，将它们直接发送到浏览器，最后将静态标记"混合"为客户端上完全交互的应用程序。
服务器渲染的Vue.js应用程序也可以被认为是"同构"或"通用"，因为应用程序的大部分代码都可以在服务器和客户端上运行。
<!-- more -->
## 传统打包
`Vue项目`打包后，组件都是JS在`html`文件返回后再渲染到`<div id=app></div>`里的。这就合理的解释了`SEO`缺陷的原因。

# 安装流程
## 新手模板
> 初始化项目

``` javascript
$ vue init nuxt-community/starter-template <project-name>
```
注：如果vue-cli没有安装, 需先通过`npm install -g vue-cli`来安装。

> 安装依赖包

``` javascript
$ cd <project-name>
$ npm install
```

> 启动项目

``` javascript
$ npm run dev
```

注意：
1. 应用现在运行在 http://localhost:3000
2. Nuxt.js 会监听 pages 目录中的文件变更并自动重启， 当添加新页面时没有必要手工重启应用。
3. 运行端口占用，如何切换端口

## 模板结构
``` javascript
.
├── .nuxt/               # 自动生成的配置文件（无需配置）
	├── components/          # css 文件夹
	├── views/            	 # img 文件夹
	├── App.js             	 # css 文件夹
	├── client.js            # css 文件夹
	├── empty.js             # css 文件夹
	├── index.js             # css 文件夹
	├── loading.html         # css 文件夹
	├── middleware.js        # css 文件夹
	├── router.js            # css 文件夹
	├── server.js            # css 文件夹
	└── utils.js             # css 文件夹
├── api/                 # 资源目录 assets 用于组织未编译的静态资源如 LESS、SASS
	├── index.js         		 # API接口配置项
	├── env.js            	 # 开发配置项
	└── http.js            	 # Axios
├── assets/              # 资源目录 assets 用于组织未编译的静态资源如 LESS、SASS
	├── css/            		 # css 文件夹
	├── img/            		 # img 文件夹
	├── less/            		 # less 文件夹
	└── scss/            		 # scss 文件夹
├── components/          # 组件目录：Vue.js 组件。Nuxt.js 不会扩展增强该目录下 Vue.js 组件，即这些组件不会像页面组件那样有 asyncData 方法的特性。
├── layouts/             # 布局目录：应用的布局组件
├── middleware/          # 中间件：存放应用的中间件
├── pages/               # 页面目录：用于组织应用的路由及视图
├── plugins/             # 插件目录：用于组织那些需要在 根vue.js应用 实例化之前需要运行的 Javascript 插件
├── static/              # 静态文件目录：用于存放应用的静态文件
├── store/               # Vuex状态树
├── .eslintignore        # ESLint 检查中需忽略的文件（夹）
├── .eslintrc            # ESLint 配置
├── .gitignore           # 需被 Git 忽略的文件（夹）
├── nuxt.config.js       # 应用的个性化配置，以便覆盖默认配置
├── README.md            # README 简介
└── package.json         # 应用的依赖关系、对外暴露的脚本接口
```
# 工作原理

![工作原理](https://user-gold-cdn.xitu.io/2018/5/24/16390509c83aef03?imageView2/0/w/1280/h/960/format/webp/ignore-error/1 "")

# 使用方法
## 绝对路径
| 绝对路径    | 路径        |
| ----------- | ----------- |
| ~           | /           |
| ~api        | /api        |
| ~assets     | /assets     |
| ~components | /components |
| ~layouts    | /layouts    |
| ~middleware | /middleware |
| ~pages      | /pages      |
| ~plugins    | /plugins    |
| ~static     | /static     |
| ~store      | /store      |

## 路由
Nuxt.js 根据 pages 目录结构去生成 vue-router 配置，也就是说 pages 目录的结构直接影响路由结构

``` javascript
|-- pages
    |-- posts
        |-- index.vue
        |-- welcome.vue
    |-- about.vue
    |-- index.vue

=> 生成

routes: [
  {
    path: '/posts',
    component: '~pages/posts/index.vue'
  }, {
    path: '/posts/welcome',
    component: '~pages/posts/welcome.vue'
  }, {
    path: '/about',
    component: '~pages/about.vue'
  }, {
    path: '/',
    component: '~pages/index.vue'
  }
]
```

### 隐藏路由
在文件名前加 _

``` javascript
|-- pages
    |-- _about.vue
    |-- index.vue

=> 生成
routes: [
	{
		path: '/',
		component: '~pages/index.vue'
	},
	{
		path: '/:about',
		component: '~pages/_about.vue'
	},
]
```

## 错误页面
默认访问：layouts/error.vue

``` javascript
<template>
  <div class="container">
    <h1 v-if="error.statusCode === 404">页面不存在</h1>
    <h1 v-else>应用发生错误异常</h1>
    <nuxt-link to="/">首 页</nuxt-link>
  </div>
</template>

<script>
export default {
  props: ['error'],
  layout: 'blog' // 你可以为错误页面指定自定义的布局
}
</script>
```

# 页面方法
## asyncData 异步获取数据
你可能想要在服务器端获取并渲染数据。Nuxt.js添加了asyncData方法使得你能够在渲染组件之前异步获取数据。

``` javascript
export default {
	data () {
		return { project: 'default' }
	},
	asyncData (context) {
		return { project: 'nuxt' }
	}
}
```

asyncData方法：会在组件`（限于页面组件）`每次加载之前被调用。它可以在服务端或路由更新之前被调用。在这个方法被调用的时候，第一个参数被设定为当前页面的上下文对象，你可以利用 asyncData方法来获取数据并返回给当前组件。

| 属性字段 | 类型            | 可用            | 描述                                                                                                                   |
| -------- | --------------- | --------------- | ---------------------------------------------------------------------------------------------------------------------- |
| isClient | Boolean         | 客户端 & 服务端 | 是否来自客户端渲染                                                                                                     |
| isServer | Boolean         | 客户端 & 服务端 | 是否来自服务端渲染                                                                                                     |
| isDev    | Boolean         | 客户端 & 服务端 | 是否是开发(dev) 模式，在生产环境的数据缓存中用到                                                                       |
| route    | vue-router 路由 | 客户端 & 服务端 | vue-router 路由实例。                                                                                                  |
| store    | vuex 数据流     | 客户端 & 服务端 | Vuex.Store 实例。只有vuex 数据流存在相关配置时可用。                                                                   |
| env      | Object          | 客户端 & 服务端 | nuxt.config.js 中配置的环境变量, 见 环境变量 api                                                                       |
| params   | Object          | 客户端 & 服务端 | route.params 的别名                                                                                                    |
| query    | Object          | 客户端 & 服务端 | route.query 的别名                                                                                                     |
| req      | http.Request    | 服务端          | Node.js API 的 Request 对象。如果 nuxt 以中间件形式使用的话，这个对象就根据你所使用的框架而定。nuxt generate 不可用。  |
| res      | http.Response   | 服务端          | Node.js API 的 Response 对象。如果 nuxt 以中间件形式使用的话，这个对象就根据你所使用的框架而定。nuxt generate 不可用。 |
| redirect | Function        | 客户端 & 服务端 | 用这个方法重定向用户请求到另一个路由。状态码在服务端被使用，默认 302。redirect([status,] path [, query])               |
| error    | Function        | 客户端 & 服务端 | 用这个方法展示错误页：error(params)。params 参数应该包含 statusCode 和 message 字段。                                  |

### 错误处理

> error

Nuxt.js 在上下文对象context中提供了一个 error(params) 方法，你可以通过调用该方法来显示错误信息页面。params.statusCode 可用于指定服务端返回的请求状态码。

``` javascript
export default {
	asyncData ({ params, error }) {
		return axios.get(`https://my-api/posts/${params.id}`)
		.then((res) => {
			return { title: res.data.title }
		})
		.catch((e) => {
			error({ statusCode: 404, message: 'Post not found' })
		})
	}
}
```

> callback

## metaInfo
Nuxt.js 使用了 vue-meta 更新应用的 头部标签(Head) 和 html 属性。

``` javascript
head () {
  return {
    title: 'title', // 标题
    meta: [
      {
        hid: 'Keywords', // 唯一标识
        name: 'Keywords1', // 名称
        content: 'Keywords2' // 内容
      }
    ],
    link: [
      {
	      rel: 'icon',
	      type: 'image/x-icon',
	      href: '/favicon.ico'
      }
    ]
  }
},
```

注意：为了避免子组件中的meta标签不能正确覆盖父组件中相同的标签而产生重复的现象，建议利用 hid 键为meta标签配一个唯一的标识编号。

## fetch
fetch 方法用于在渲染页面前填充应用的状态树（store）数据， 与 asyncData 方法类似，不同的是它不会设置组件的数据。

``` javascript
fetch ({ store, params }) {
  return axios.get('http://my-api/stars')
  .then((res) => {
    store.commit('setStars', res.data)
  })
}
```

你也可以使用 async 或 await 的模式简化代码如下

``` javascript
async fetch ({ store, params }) {
  let { data } = await axios.get('http://my-api/stars')
  store.commit('setStars', data)
}
```

## layout
根目录下的所有文件都属于个性化布局文件，可以在页面组件中利用 layout 属性来引用。

``` javascript
export default {
  layout: 'blog',
  // 或
  layout (context) {
    return 'blog'
  }
}
```

``` javascript
```

# 配置文件
目录下的 nuxt.config.js 是我们唯一的配置入口，默认的给力我们三个配置 ·head·css·loading· 分别是头部设置，全局css，loading进度条

| 属性       | 名称     |
| ---------- | -------- |
| build      | 模块管理 |
| cache      | 组件缓存 |
| css        | 全局样式 |
| dev        | 开发配置 |
| env        | 环境配置 |
| generate   | 静态配置 |
| head       |  全局头部配置  |
| loading    | 加载配置 |
| plugins    | 插件配置 |
| rootDir    | 配置     |
| router     | 路由配置 |
| srcDir     | 配置     |
| transition | 动画配置 |

## build
Nuxt.js 允许你在自动生成的 vendor.bundle.j 文件中添加一些模块，以减少应用 bundle的体积。如果你的应用依赖第三方模块，这个配置项是十分实用的。

### analyze
Nuxt.js 使用 webpack-bundle-analyzer 分析并可视化构建后的打包文件，你可以基于分析结果来决定如何优化它。

``` javascript
默认值： false

module.exports = {
	build: {
		analyze: true
		// or
		analyze: {
			analyzerMode: 'static'
		}
	}
}
```

提示： 可通过 nuxt build --analyze 或 nuxt build -a 命令来启用该分析器进行编译构建，分析结果可在 http://localhost:8888 上查看。

### babel
为 JS 和 Vue 文件设定自定义的 babel 配置。
``` javascript
默认值：

{
	presets: ['vue-app']
}
```

``` javascript
module.exports = {
	build: {
		babel: {
			presets: ['es2015', 'stage-0']
		}
	}
}
```

### extend
为客户端和服务端的构建配置进行手工的扩展处理。

该扩展方法会被调用两次，一次在服务端打包构建的时候，另外一次是在客户端打包构建的时候。该方法的参数如下：

Webpack 配置对象
构建环境对象，包括这些属性（全部为布尔类型）： isDev， isClient， isServer

``` javascript
module.exports = {
	build: {
		extend (config, { isClient }) {
			// 为 客户端打包 进行扩展配置
			if (isClient) {
				config.devtool = 'eval-source-map'
			}
		}
	}
}
```
如果你想了解更多关于webpack的配置，可以移步 Nuxt.js 源码的 webpack 目录。

### filenames 自定义打包文件名
``` javascript
默认值：

module.exports = {
  build: {
    filenames: {
      vendor: 'vendor.[hash].js',
      app: 'app.[chunkhash].js'
    }
  }
}
```

### loaders 加载器
自定义 webpack 加载器

``` javascript
默认值：

[
	{
		test: /\.(png|jpe?g|gif|svg)$/,
		loader: 'url-loader',
		query: {
			limit: 1000, // 1KO
			name: 'img/[name].[hash:7].[ext]'
		}
	},
	{
		test: /\.(woff2?|eot|ttf|otf)(\?.*)?$/,
		loader: 'url-loader',
		query: {
			limit: 1000, // 1 KO
			name: 'fonts/[name].[hash:7].[ext]'
		}
	}
]
```

``` javascript
module.exports = {
	build: {
		loaders: [
			{
				test: /\.(png|jpe?g|gif|svg)$/,
				loader: 'url-loader',
				query: {
					limit: 10000, // 10KO
					name: 'img/[name].[hash].[ext]'
				}
			}
		]
	}
}
```
当 nuxt.config.js 里有自定义的 loaders 配置时，将会覆盖默认的配置。

### plugins 插件

``` javascript
const webpack = require('webpack')

module.exports = {
	build: {
		plugins: [
			new webpack.DefinePlugin({
				'process.VERSION': require('./package.json').version
			})
		]
	}
}
```

### postcss
``` javascript
默认值：

[
	require('autoprefixer')({
		browsers: ['last 3 versions']
	})
]
```

``` javascript
module.exports = {
	build: {
		postcss: [
			require('postcss-nested')(),
			require('postcss-responsive-type')(),
			require('postcss-hexrgba')(),
			require('autoprefixer')({
				browsers: ['last 3 versions']
			})
		]
	}
}
```
### publicPath CDN地址
Nuxt.js 允许你将待发布的文件直接上传至 CDN 以获得最佳访问性能，只需设置 publicPath 为你的 CDN 地址即可。

``` javascript
默认值: '/_nuxt/'

module.exports = {
	build: {
		publicPath: 'https://cdn.nuxtjs.org'
	}
}
```
通过以上配置，当运行 nuxt build 时，再将.nuxt/dist/目录的内容上传到您的CDN，然后瞧！

### vendor 第三方模块
Nuxt.js 允许你在自动生成的 vendor.bundle.js 文件中添加一些模块，以减少应用 bundle 的体积。这里说的是一些你所依赖的第三方模块 (比如 axios)
``` javascript
module.exports = {
	build: {
		vendor: ['axios']
	}
}
```

``` javascript
module.exports = {
	build: {
		vendor: [
			'axios',
			'~plugins/my-lib.js'
		]
	}
}
```

## cache 组件缓存
该配置项让你开启组件缓存策略以提升渲染性能。
如 cache 设定的值为 true，那么相当于应用了下面的默认配置：
``` javascript
module.exports = {
  cache: true
  // or
  cache: {
    max: 1000,
    maxAge: 900000
  }
}
```

| 属性名 | 是否可选? | 类型 | 默认值 | 描述                                                                                      |
| ------ | --------- | ---- | ------ | ----------------------------------------------------------------------------------------- |
| max    | 是        | 整型 | 1000   | 缓存组件的最大数目，当第 1001 个组件被添加至缓存中时， 第一个被缓存的组件会从缓存中移除。 |
| maxAge | 是        | 整型 | 900000 | 缓存时间，单位毫秒, 默认是 15 分钟。                                                      |

## css
该配置项用于定义应用的全局（所有页面均需引用的）样式文件、模块或第三方库。
1. src: String (文件路径)
1. lang: String (所需的预处理器)

``` javascript
module.exports = {
	css: [
		// 加载一个 node.js 模块
		'hover.css/css/hover-min.css',
		// 同样加载一个 node.js 模块，不过我们定义所需的预处理器
		{ src: 'bulma', lang: 'sass' },
		// 项目中的 CSS 文件
		'~assets/css/main.css',
		// 项目中的 Sass 文件
		{ src: '~assets/css/main.scss', lang: 'scss' } // 指定 scss 而非 sass
	]
}
```

## dev
dev 属性的值会被 nuxt 命令 覆盖
1. 当使用 nuxt 命令时，dev 会被强制设置成 true
1. 当使用 nuxt build， nuxt start 或 nuxt generate 命令时，dev 会被强制设置成 false

``` javascript
module.exports = {
  dev: (process.env.NODE_ENV !== 'production')
}

=> server.js

const {Nuxt, Builder} = require('nuxt')
const app = require('express')()
const port = process.env.PORT || 3000

// 传入配置初始化 Nuxt.js 实例
let config = require('./nuxt.config.js')
const nuxt = new Nuxt(config)
app.use(nuxt.render)

// 在开发模式下进行编译
if (config.dev) {
  new Builder(nuxt).build()
  .catch((error) => {
    console.error(error)
    process.exit(1)
  })
}

// 监听指定端口
app.listen(port, '0.0.0.0')
console.log('服务器运行于 localhost:' + port)
```


## env
Nuxt.js 让你可以配置在客户端和服务端共享的环境变量。
``` javascript
module.exports = {
  env: {
    baseUrl: process.env.BASE_URL || 'http://localhost:3000'
  }
}
```

我们可以通过以下两种方式来使用 baseUrl 变量：

1. 通过 process.env.baseUrl
1. 通过 context.baseUrl，请参考 context api

举个例子， 我们可以利用它来配置 axios 的自定义实例。

``` javascript
plugins/axios.js

import axios from 'axios'

export default axios.create({
  baseURL: process.env.baseUrl
})
```

## generate
配置 Nuxt.js 应用生成静态站点的具体方式。
``` javascript
```
该配置项用于定义每个动态路由的参数，Nuxt.js 依据这些路由配置生成对应目录结构的静态文件。

关于 generate 配置项的详细文档

## head
借助 head 属性，Nuxt.js 让你可以在 nuxt.config.js 中配置应用的 meta 信息。
``` javascript
module.exports = {
  head: {
    titleTemplate: '%s - Nuxt.js',
    meta: [
      { charset: 'utf-8' },
      { name: 'viewport', content: 'width=device-width, initial-scale=1' },
      { hid: 'description', name: 'description', content: 'Meta description' }
    ]
  }
}
```

[关于 head 配置项的详细文档](https://github.com/declandewet/vue-meta#recognized-metainfo-properties "")

## loading
在页面切换的时候，Nuxt.js 使用内置的加载组件显示加载进度条。你可以定制它的样式，禁用或者创建自己的加载组件。

### 禁用加载进度条
``` javascript
module.exports = {
	loading: false
}
```

### 个性化加载进度条
| 键          | 类型   | 默认值  | 描述                                                                 |
| ----------- | ------ | ------- | -------------------------------------------------------------------- |
| color       | String | 'black' | 进度条的颜色                                                         |
| failedColor | String | 'red'   | 页面加载失败时的颜色 （当 data 或 fetch 方法返回错误时）。           |
| height      | String | '2px'   | 进度条的高度 (在进度条元素的 style 属性上体现)。                     |
| duration    | Number | 5000    | 进度条的最大显示时长，单位毫秒。Nuxt.js 假设页面在该时长内加载完毕。 |

``` javascript
module.exports = {
  loading: {
    color: 'blue',
    height: '5px'
  }
}
```

### 自定义加载组件
| 方法          | 是否必须 | 描述                                                                              |
| ------------- | -------- | --------------------------------------------------------------------------------- |
| start()       | 是       | 路由更新（即浏览器地址变化）时调用, 请在该方法内显示组件。                        |
| finish()      | 是       | 路由更新完毕（即asyncData方法调用完成且页面加载完）时调用，请在该方法内隐藏组件。 |
| fail()        | 否       | 路由更新失败时调用（如asyncData方法返回异常）。                                   |
| increase(num) | 否       | 页面加载过程中调用, num 是小于 100 的整数。                                       |

``` javascript
module.exports = {
	loading: '~components/loading.vue'
}
```

``` javascript
components/loading.vue

<template lang="html">
  <div class="loading-page" v-if="loading">
    <p>Loading...</p>
  </div>
</template>

<script>
export default {
  data: () => ({
    loading: false
  }),
  methods: {
    start () {
      this.loading = true
    },
    finish () {
      this.loading = false
    }
  }
}
</script>

<style scoped>
.loading-page {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(255, 255, 255, 0.8);
  text-align: center;
  padding-top: 200px;
  font-size: 30px;
  font-family: sans-serif;
}
</style>
```

## plugins
该配置项用于配置那些需要在 根vue.js应用 实例化之前需要运行的 Javascript 插件。
``` javascript
plugins: [
	{src: '~plugins/extra.js', ssr: false},
	{src: '~plugins/share.js', ssc: true}
],
```

1. src: String (文件的路径)
1. ssr: Boolean (默认为 true) 如果值为 false，该文件只会在客户端被打包引入。有些插件可能只是在浏览器里使用，所以你可以用 ssr: false 变量来配置插件只从客户端还是服务端运行。

## rootDir
``` javascript
```
该配置项用于配置 Nuxt.js 应用的根目录。

关于 rootDir 配置项的详细文档

## router
### base
应用的根URL。举个例子，如果整个单页面应用的所有资源可以通过 /app/ 来访问，那么 base 配置项的值需要设置为 '/app/'。

``` javascript
默认值： '/'

module.exports = {
	router: {
		base: '/app/'
	}
}
```
base 被设置后，Nuxt.js 会自动将它添加至页面中： <base href="{{ router.base }}"/>。该配置项的值会被直接传给 vue-router 的构造器。

### mode
默认值：'history'，配置路由的模式，鉴于服务端渲染的特性，不建议修改该配置。该配置项的值会被直接传给 vue-router 的构造器。

``` javascript
module.exports = {
	router: {
		mode: 'hash'
	}
}
```

### linkActiveClass
默认值： 'nuxt-link-active'，全局配置 <nuxt-link> 组件默认的激活类名。该配置项的值会被直接传给 vue-router 的构造器。

``` javascript
module.exports = {
	router: {
		linkActiveClass: 'active-link'
	}
}
```

### scrollBehavior
scrollBehavior 配置项用于个性化配置跳转至目标页面后的页面滚动位置。每次页面渲染后都会调用 scrollBehavior 配置的方法。该配置项的值会被直接传给 vue-router 的构造器。

scrollBehavior 的默认配置为：
``` javascript
const scrollBehavior = (to, from, savedPosition) => {
	// savedPosition 只有在 popstate 导航（如按浏览器的返回按钮）时可以获取。
	if (savedPosition) {
		return savedPosition
	} else {
		let position = {}
		// 目标页面子组件少于两个
		if (to.matched.length < 2) {
			// 滚动至页面顶部
			position = { x: 0, y: 0 }
		}
		else if (to.matched.some((r) => r.components.default.options.scrollToTop)) {
			// 如果目标页面子组件中存在配置了scrollToTop为true
			position = { x: 0, y: 0 }
		}
		// 如果目标页面的url有锚点,  则滚动至锚点所在的位置
		if (to.hash) {
			position = { selector: to.hash }
		}
		return position
	}
}
```

举个例子，我们可以配置所有页面渲染后滚动至顶部：
``` javascript
module.exports = {
	router: {
		scrollBehavior: function (to, from, savedPosition) {
			return { x: 0, y: 0 }
		}
	}
}
```


### middleware
为应用的每个页面设置默认的中间件。

``` javascript
module.exports = {
	router: {
		// 在每页渲染前运行 middleware/user-agent.js 中间件的逻辑
		middleware: 'user-agent'
	}
}
middleware/user-agent.js

export default function (context) {
	// 给上下文对象增加 userAgent 属性（增加的属性可在 `asyncData` 和 `fetch` 方法中获取）
	context.userAgent = context.isServer ? context.req.headers['user-agent'] : navigator.userAgent
}
```
了解更多关于中间件的信息，请参考 中间件指引文档。

### extendRoutes
你可以通过 extendRoutes 配置项来扩展 Nuxt.js 生成的路由配置。

``` javascript
const resolve = require('path').resolve

module.exports = {
	router: {
		extendRoutes (routes) {
			routes.push({
				name: 'custom',
				path: '*',
				component: resolve(__dirname, 'pages/404.vue')
			})
		}
	}
}
```

## srcDir
``` javascript
```
该配置项用于配置应用的源码目录路径。

关于 srcDir 配置项的详细文档

## transition
该配置项用于个性化配置应用过渡效果属性的默认值。

``` javascript
默认值：
{
	name: 'page',
	mode: 'out-in'
}
```

``` javascript
module.exports = {
	transition: 'page'
	// or
	transition: {
		name: 'page',
		mode: 'out-in',
		beforeEnter (el) {
			console.log('Before enter...');
		}
	}
}
```

| 属性字段         | 类型    | 默认值   | 描述                                                                                                                                    |
| ---------------- | ------- | -------- | --------------------------------------------------------------------------------------------------------------------------------------- |
|                  |         |          |                                                                                                                                         |
| name             | String  | "page"   | 所有路由过渡都会用到的过渡名称。                                                                                                        |
| mode             | String  | "out-in" | 所有路由都用到的过渡模式，见 Vue.js transition 使用文档。                                                                               |
| css              | Boolean | true     | 是否给页面组件根元素添加 CSS 过渡类名。如果值为 false，路由过渡时将触发页面组件事件注册的 Javascript 钩子方法。                         |
| type             | String  | n/a      | 指定过滤动效事件的类型，用于判断过渡结束的时间点。值可以是 "transition" 或 "animation"。 默认情况下, Nuxt.js 会自动侦测动效事件的类型。 |
| enterClass       | String  | n/a      | 目标路由动效开始时的类名。 详情请参考 Vue.js transition 使用文档 。                                                                     |
| enterToClass     | String  | n/a      | 目标路由动效结束时的类名。 详情请参考 Vue.js transition 使用文档 。                                                                     |
| enterActiveClass | String  | n/a      | 目标路由过渡过程中的类名。详情请参考 Vue.js transition 使用文档 。                                                                      |
| leaveClass       | String  | n/a      | 当前路由动效开始时的类名。 详情请参考 Vue.js transition 使用文档 。                                                                     |
| leaveToClass     | String  | n/a      | 当前路由动效结束时的类名。 详情请参考 Vue.js transition 使用文档 。                                                                     |
| leaveActiveClass | String  | n/a      | 当前路由动效过程中的类名。详情请参考 Vue.js transition 使用文档 。                                                                      |

# 安装插件
## SAAS
``` javascript
npm install --save-dev node-sass sass-loader
```

## LESS
``` javascript
npm install --save-dev less less-loader
```

## Axios API

``` javascript
```


##
``` javascript
```


# 案例
[Nuxt 案例](https://nuxtjs.org/examples "")
[]( "")
[]( "")
[]( "")
[V2ex clone built with Nuxt.js](https://github.com/OrangeXC/n2ex "vue ssr v2ex")
[文章发布系统](https://github.com/ITCNZ/mynuxt "vue+nuxt+sass+node+express+MongoDB 实现的文章发布系统")
[A Nuxtjs Website using Storyblok](https://github.com/onefriendaday/nuxtjs-multilanguage-website "")
[vuejs, nuxtjs 搭建 新闻聚合网站](https://github.com/MontageD/nuxt-maopingshou "Node.js(6.9.1) + vue(2.0) + vuex + (NUXT)SSR + nginx反向代理")
