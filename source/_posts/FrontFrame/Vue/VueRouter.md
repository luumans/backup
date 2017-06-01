title: Vue Router
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

# TODO

[ ] history的后退配置
[ ] 路由懒加载
[ ] 滚动行为
[ ] router.beforeEach
<!-- more -->

# 基础
## 路由
### 使用
```
<div id="app">
  <h1>Hello App!</h1>
  <p>
    <!-- 使用 router-link 组件来导航. -->
    <!-- 通过传入 `to` 属性指定链接. -->
    <!-- <router-link> 默认会被渲染成一个 `<a>` 标签 -->
    <router-link to="/foo">Go to Foo</router-link>
    <router-link to="/bar">Go to Bar</router-link>
  </p>
  <!-- 路由出口 -->
  <!-- 路由匹配到的组件将渲染在这里 -->
  <router-view></router-view>
</div>
```

### 配置

```
import Vue from 'vue'
import Router from 'vue-router'

import Active from 'VIEW/active/active'

Vue.use(Router)

// 通过这个这个属性（是个函数），可以让应用像浏览器的原生表现那样，在按下 后退/前进 按钮时，简单地让页面滚动到顶部或原来的位置。
const scrollBehavior = (to, from, savedPosition) => {
  if (savedPosition) {
    return savedPosition
  } else {
    return { x: 0, y: 0 }
  }
}

export default new Router({
	mode: 'history',
	// history: 依赖 HTML5 History API 和服务器配置。
  base: __dirname,
  // 默认值: “/”，应用的基路径，一般就是项目的根目录，webpack中有配置好。
  linkActiveClass:'link-active',
  scrollBehavior,
  routes: [
    {
      path: '/',
      name: 'Active',
      component: Active
    }
  ]
})
```

```
new Vue({
  el: '#app',
  router,
  template: '<App/>',
  components: { App }
})
```

## 动态路由匹配
在 vue-router 的路由路径中使用『动态路径参数』（dynamic segment）来达到这个效果：

### 路径参数
```
{ path: '/user/:username', component: User }
/user/evan
{ path: '/user/:username', component: User }
/user/evan/post/123

参数不能不传
```

### 响应路由参数的变化
例如从 /user/foo 导航到 user/bar，原来的组件实例会被复用。因为两个路由都渲染同个组件，比起销毁再创建，复用则显得更加高效。不过，这也意味着组件的生命周期钩子不会再被调用。
```
watch: {
  '$route' (to, form) {
  }
},
```


### 高级匹配模式
vue-router 使用 [path-to-regexp](https://github.com/pillarjs/path-to-regexp#parameters "") 作为路径匹配引擎，所以支持很多高级的匹配模式，例如：可选的动态路径参数、匹配零个或多个、一个或多个，甚至是自定义正则匹配。查看它的 文档 学习高阶的路径匹配，还有 这个例子 展示 vue-router 怎么使用这类匹配
```
{ path: '/' },
// 参数前面使用“：”表示
{ path: '/params/:foo/:bar' },
// 添加“？”使参数作为可选择
{ path: '/optional-params/:foo?' },
// 匹配id为数字的链接
{ path: '/params-with-regex/:id(\\d+)' },
// * 可以匹配任何东西
{ path: '/asterisk/*' },
// 使用括号包裹，用？让其可选择
make part of th path optional by wrapping with parens and add "?"
{ path: '/optional-group/(foo/)?bar' }

<li><router-link to="/">/</router-link></li>
<li><router-link to="/params/foo/bar">/params/foo/bar</router-link></li>
<li><router-link to="/optional-params">/optional-params</router-link></li>
<li><router-link to="/optional-params/foo">/optional-params/foo</router-link></li>
<li><router-link to="/params-with-regex/123">/params-with-regex/123</router-link></li>
<li><router-link to="/params-with-regex/abc">/params-with-regex/abc</router-link></li>
<li><router-link to="/asterisk/foo">/asterisk/foo</router-link></li>
<li><router-link to="/asterisk/foo/bar">/asterisk/foo/bar</router-link></li>
<li><router-link to="/optional-group/bar">/optional-group/bar</router-link></li>
<li><router-link to="/optional-group/foo/bar">/optional-group/foo/bar</router-link></li>
```

### 匹配优先级
有时候，同一个路径可以匹配多个路由，此时，匹配的优先级就按照路由的定义顺序：谁先定义的，谁的优先级就最高。

## [嵌套路由](http://jsfiddle.net/yyx990803/L7hscd8h/ "")
这里的 <router-view> 是最顶层的出口，渲染最高级路由匹配到的组件。同样地，一个被渲染组件同样可以包含自己的嵌套 <router-view>。例如，在 User 组件的模板添加一个 <router-view>：
children 配置就是像 routes 配置一样的路由配置数组，所以呢，你可以嵌套多层路由。
```
{ path: '/user/:id', component: User,
  children: [
  	// 空的 子路由
  	{ path: '', component: UserHome },
    {
      // 当 /user/:id/profile 匹配成功，
      // UserProfile 会被渲染在 User 的 <router-view> 中
      path: 'profile',
      component: UserProfile
    },
    {
      // 当 /user/:id/posts 匹配成功
      // UserPosts 会被渲染在 User 的 <router-view> 中
      path: 'posts',
      component: UserPosts
    }
  ]
}
```
注意：以 / 开头的嵌套路径会被当作根路径。 这让你充分的使用嵌套组件而无须设置嵌套的路径。

## 编程式的导航

### router.push(location)
> router.push(location, onComplete?, onAbort?)

想要导航到不同的 URL，则使用 router.push 方法。这个方法会向 history 栈添加一个新的记录，所以，当用户点击浏览器后退按钮时，则回到之前的 URL。

|  声明式  | 编程式 |
| :----: | :----: |
|  `<router-link :to="...">`  |  router.push(...)  |

```
// 字符串
this.$router.push('home')

// 对象
this.$router.push({ path: 'home' })

// 命名的路由
this.$router.push({ name: 'user', params: { userId: 123 }})
this.$route.params.userId

// 带查询参数，变成 /register?plan=private
this.$router.push({ path: 'register', query: { plan: 'private' }})
this.$route.query.plan
```

### router.replace(location)
> router.replace(location, onComplete?, onAbort?)

跟 router.push 很像，唯一的不同就是，它不会向 history 添加新记录，而是跟它的方法名一样 —— 替换掉当前的 history 记录。

|  声明式  | 编程式 |
| :----: | :----: |
|  `<router-link :to="..." replace>` |  router.replace(...)  |

### router.go(n)
这个方法的参数是一个整数，意思是在 history 记录中向前或者后退多少步，类似 window.history.go(n)。

```
// 在浏览器记录中前进一步，等同于 history.forward()
this.$router.go(1)

// 后退一步记录，等同于 history.back()
this.$router.go(-1)

// 前进 3 步记录
this.$router.go(3)

// 如果 history 记录不够用，那就默默地失败呗
this.$router.go(-100)
this.$router.go(100)
```

## 命名路由
通过一个名称来标识一个路由显得更方便一些
```
/user/123
<router-link :to="{ name: 'user', params: { userId: 123 }}">User</router-link>
<router-link :to="{ name: 'user', params: { userId: 123 }}">User</router-link>
this.$router.push({ name: 'user', params: { userId: 123 }})
```

## [命名视图](https://jsfiddle.net/posva/6du90epg/)
有时候想同时（同级）展示多个视图，而不是嵌套展示，例如创建一个布局，有 sidebar（侧导航） 和 main（主内容） 两个视图，这个时候命名视图就派上用场了。你可以在界面中拥有多个单独命名的视图，而不是只有一个单独的出口。如果 router-view 没有设置名字，那么默认为 default。

```
<router-view class="view one"></router-view>
<router-view class="view two" name="a"></router-view>
<router-view class="view three" name="b"></router-view>

const router = new VueRouter({
  routes: [
    {
      path: '/',
      components: {
        default: Foo,
        a: Bar,
        b: Baz
      }
    }
  ]
})
```

## 重定向 和 别名

### 重定向

#### 路径
```
{ path: '/a', redirect: '/b' }
```
#### 命名
```
{ path: '/a', redirect: { name: 'foo' }}
```

#### 动态返回重定向
```
{ path: '/a', redirect: to => {
  // 方法接收 目标路由 作为参数
  // return 重定向的 字符串路径/路径对象
}}
```

### 别名
别名：的功能让你可以自由地将 UI 结构映射到任意的 URL，而不是受限于配置的嵌套路由结构。
```
{ path: '/a', component: A, alias: '/b' }
```

## HTML5 History 模式 ****
vue-router 默认 hash 模式 —— 使用 URL 的 hash 来模拟一个完整的 URL，于是当 URL 改变时，页面不会重新加载。
http://localhost:8680/#/Tap/btn/Github
```
const router = new VueRouter({
  mode: 'history',
  routes: [...]
})
```

/user/:id
不过这种模式要玩好，还需要后台配置支持。因为我们的应用是个单页客户端应用，如果后台没有正确的配置，当用户在浏览器直接访问 http://oursite.com/user/id 就会返回 404，这就不好看了。


# 进阶
正如其名，vue-router 提供的导航钩子主要用来拦截导航，让它完成跳转或取消。有多种方式可以在路由导航发生时执行钩子：全局的, 单个路由独享的, 或者组件级的。
## 导航钩子
### router.beforeEach
```
to: Route, from: Route, next: Function
router.beforeEach((to, from, next) => {
  // to 和 from 都是 路由信息对象
})
```

#### to: Route: 
即将要进入的目标 路由对象

#### from: Route: 
当前导航正要离开的路由

#### next: Function: 
一定要调用该方法来 resolve 这个钩子。执行效果依赖 next 方法的调用参数。

next(): 进行管道中的下一个钩子。如果全部钩子执行完了，则导航的状态就是 confirmed （确认的）。

next(false): 中断当前的导航。如果浏览器的 URL 改变了（可能是用户手动或者浏览器后退按钮），那么 URL 地址会重置到 from 路由对应的地址。

next('/') 或者 next({ path: '/' }): 跳转到一个不同的地址。当前的导航被中断，然后进行一个新的导航。

### router.afterEach
```
after 钩子没有 next 方法，不能改变导航：
router.afterEach(route => {
  // ...
})
```

## 某个路由独享的钩子
这些钩子与全局 before 钩子的方法参数是一样的。
```
const router = new VueRouter({
  routes: [
    {
      path: '/foo',
      component: Foo,
      beforeEnter: (to, from, next) => {
        // ...
      }
    }
  ]
})
```

## 组件内的钩子
```
const Foo = {
  template: `...`,
  beforeRouteEnter (to, from, next) {
    // 在渲染该组件的对应路由被 confirm 前调用
    // 不！能！获取组件实例 `this`
    // 因为当钩子执行前，组件实例还没被创建
  },
  beforeRouteUpdate (to, from, next) {
    // 在当前路由改变，但是该组件被复用时调用
    // 举例来说，对于一个带有动态参数的路径 /foo/:id，在 /foo/1 和 /foo/2 之间跳转的时候，
    // 由于会渲染同样的 Foo 组件，因此组件实例会被复用。而这个钩子就会在这个情况下被调用。
    // 可以访问组件实例 `this`
  },
  beforeRouteLeave (to, from, next) {
    // 导航离开该组件的对应路由时调用
    // 可以访问组件实例 `this`
  }
}
```
## 数据获取

> 导航完成之后获取：

先完成导航，然后在接下来的组件生命周期钩子中获取数据。在数据获取期间显示『加载中』之类的指示。

> 导航完成之前获取：

导航完成前，在路由的 enter 钩子中获取数据，在数据获取成功后执行导航。

### 导航完成后获取数据
```
<template>
  <div class="post">
    <div class="loading" v-if="loading">
      Loading...
    </div>

    <div v-if="error" class="error">

    </div>

    <div v-if="post" class="content">
      <h2></h2>
      <p></p>
    </div>
  </div>
</template>
export default {
  data () {
    return {
      loading: false,
      post: null,
      error: null
    }
  },
  created () {
    // 组件创建完后获取数据，
    // 此时 data 已经被 observed 了
    this.fetchData()
  },
  watch: {
    // 如果路由有变化，会再次执行该方法
    '$route': 'fetchData'
  },
  methods: {
    fetchData () {
      this.error = this.post = null
      this.loading = true
      // replace getPost with your data fetching util / API wrapper
      getPost(this.$route.params.id, (err, post) => {
        this.loading = false
        if (err) {
          this.error = err.toString()
        } else {
          this.post = post
        }
      })
    }
  }
}
```

### 在导航完成前获取数据
我们在导航转入新的路由前获取数据。我们可以在接下来的组件的 beforeRouteEnter 钩子中获取数据，当数据获取成功后只调用 next 方法。
```
export default {
  data () {
    return {
      post: null,
      error: null
    }
  },
  beforeRouteEnter (to, from, next) {
    getPost(to.params.id, (err, post) => 
      if (err) {
        // display some global error message
        next(false)
      } else {
        next(vm => {
          vm.post = post
        })
      }
    })
    axios.get(api).then((response) => {
      next(vm => {
        vm.votes = response.data.items
        vm.jsons = 1
      })
    }, (response) => {
      next(false)
    })
  },
  // 路由改变前，组件就已经渲染完了
  // 逻辑稍稍不同
  watch: {
	  $route () {}
    '$route': () {
      this.post = null
      getPost(this.$route.params.id, (err, post) => {
        if (err) {
          this.error = err.toString()
        } else {
          this.post = post
        }
      })
    }
  }
}
```

## 滚动行为
注意: 这个功能只在 HTML5 history 模式下可用。
使用前端路由，当切换到新路由时，想要页面滚到顶部，或者是保持原先的滚动位置，就像重新加载页面那样。 vue-router 能做到，而且更好，它让你可以自定义路由切换时页面如何滚动。

```
const router = new VueRouter({
  routes: [...],
  scrollBehavior (to, from, savedPosition) {
    // return 期望滚动到哪个的位置
  }
})
```

## 路由懒加载


# API文档

## router-link
`<router-link>` 组件支持用户在具有路由功能的应用中（点击）导航。 通过 to 属性指定目标地址，默认渲染成带有正确链接的 `<a>` 标签，可以通过配置 tag 属性生成别的标签.。另外，当目标路由成功激活时，链接元素自动设置一个表示激活的 CSS 类名。
也可以使用：`<a v-link="{name: 'user', params: {userId: 1}">This is a user whose id is 1</a>`

## Props参数
### to
表示目标路由的链接。当被点击后，内部会立刻把 to 的值传到 router.push()，所以这个值可以是一个字符串或者是描述目标位置的对象。

> 字符串

```
<!-- 字符串 -->
<router-link to="home">Home</router-link>
<!-- 渲染结果 -->
<a href="home">Home</a>
```

> 表达式

```
<!-- 使用 v-bind 的 JS 表达式'home' -->
<router-link v-bind:to="'home'">Home</router-link>

<!-- 不写 v-bind 也可以，就像绑定别的属性一样 -->
<router-link :to="'home'">Home</router-link>

<!-- 同上 -->
<router-link :to="{ path: 'home' }">Home</router-link>

<!-- 命名的路由 -->
<router-link :to="{ name: 'user', params: { userId: 123 }}">User</router-link>

<!-- 带查询参数，下面的结果为 /register?plan=private -->
<router-link :to="{ path: 'register', query: { plan: 'private' }}">Register</router-link>
```

### replace
设置 replace 属性的话，当点击时，会调用 router.replace() 而不是 router.push()，于是导航后不会留下 history 记录。
```
<router-link :to="{ path: '/abc'}" replace></router-link>
```

### append 相对路径
设置 append 属性后，则在当前（相对）路径前添加基路径。例如，我们从 /a 导航到一个相对路径 b，如果没有配置 append，则路径为 /b，如果配了，则为 /a/b

```
<router-link :to="{ path: 'relative/path'}" append></router-link>
```

### tag 渲染标签
有时候想要 <router-link> 渲染成某种标签，例如 <li>。 于是我们使用 tag prop 类指定何种标签，同样它还是会监听点击，触发导航。
```
<router-link to="/foo" tag="li">foo</router-link>
<!-- 渲染结果 -->
<li>foo</li>
```

### active-class
设置链接激活时使用的 CSS 类名。默认值可以通过路由的构造选项 linkActiveClass 来全局配置。

```
<router-link active-class replace to="/Tap/btn">proImg</router-link>

const router = new VueRouter({
	mode: 'history',
	linkActiveClass:'link-active',
	routes: []
});

.link-active{}
```

### exact
"是否激活"默认类名的依据是inclusive match （全包含匹配）。 举个例子，如果当前的路径是 /a 开头的，那么 `<router-link to="/a">` 也会被设置 CSS 类名。
[Active Links](https://jsfiddle.net/8xrk1n9f/ "")

### events
声明可以用来触发导航的事件。可以是一个字符串或是一个包含字符串的数组。
默认值: 'click'


## router-view
`<router-view>` 组件是一个 functional 组件，渲染路径匹配到的视图组件。`<router-view>` 渲染的组件还可以内嵌自己的 `<router-view>`，根据嵌套路径，渲染嵌套组件。

### name
```
<router-view class="view one"></router-view>
<router-view class="view two" name="a"></router-view>
<router-view class="view three" name="b"></router-view>
```

> keep-alive

```
<transition>
  <keep-alive>
    <router-view></router-view>
  </keep-alive>
</transition>
```

## 路由信息对象
一个 `route object`（路由信息对象） 表示当前激活的路由的状态信息，包含了当前 URL 解析得到的信息，还有 URL 匹配到的 route records（路由记录）。

### $route Watcher
```
watch: {
  '$route' (to, from) {
    const toDepth = to.path.split('/')
    const fromDepth = from.path.split('/')
    if (toDepth.length === fromDepth.length) {
      if (toDepth[toDepth.length - 1] === '') {
        this.transitionName = 'vux-pop-in'
      } else {
        this.transitionName = 'vux-pop-out'
      }
    } else {
      this.transitionName = toDepth < fromDepth ? 'vux-pop-in' : 'vux-pop-out'
    }
  }
}
```

```
router.beforeEach((to, from, next) => {
  // to 和 from 都是 路由信息对象
})
```

### this.$route
#### $route.path 绝对路径

#### $route.params 路由参数
关于动态片段（如/user/:username)的键值对信息,如{username: 'paolino'}

#### $route.query URL查询参数
请求参数，如/foo?user=1获取到query.user = 1

#### $route.hash
当前路由的 hash 值 (带 #) ，如果没有 hash 值，则为空字符串。

#### $route.fullPath
完成解析后的 URL，包含查询参数和 hash 的完整路径。

#### $route.matched
数组，包含当前匹配的路径中所包含的所有片段所对应的配置参数对象。
```
const router = new VueRouter({
  routes: [
    // 下面的对象就是 route record
    { path: '/foo', component: Foo,
      children: [
        // 这也是个 route record
        { path: 'bar', component: Bar }
      ]
    }
  ]
})
```
当 URL 为 /foo/bar，$route.matched 将会是一个包含从上到下的所有对象（副本）。

#### $route.name
当前路由的名称，如果有的话
```
{
  path: '/user/:userId',
  name: 'user',
  component: User
}
```

## Router 构造配置

### routes

```
{
	<!-- 路径 -->
  path: string;
  path: '',
  <!-- 组件 -->
  component?: Component,
  <!-- 命名路由 -->
  name?: string;
  name?: '',
  <!-- 命名视图组件 -->
  components?: { [name: string]: Component },
  redirect?: string | Location | Function,
  alias?: string | Array<string>,
  <!-- 组件 -->
  children?: Array<RouteConfig>;
  children?: [],
  <!-- 某个路由独享的钩子 -->
  beforeEnter?: (to: Route, from: Route, next: Function) => void,
  beforeEnter: (to, from, next) => {
    // ...
  }
  <!-- 路由元信息 -->
  meta?: any
  meta: { requiresAuth: true }
}
```

### mode
```
mode: 'history',
```
> hash: 

使用 URL hash 值来作路由。支持所有浏览器，包括不支持 HTML5 History Api 的浏览器。

> history: 

依赖 HTML5 History API 和服务器配置。查看 HTML5 History 模式.

> abstract: 

支持所有 JavaScript 运行环境，如 Node.js 服务器端。如果发现没有浏览器的 API，路由会自动强制进入这个模式。

### base
应用的基路径。例如，如果整个单页应用服务在 /app/ 下，然后 base 就应该设为 "/app/"。

### linkActiveClass
```
linkActiveClass:'link-active',
```

### scrollBehavior
```
const router = new VueRouter({
  scrollBehavior (to, from, savedPosition) {
    // to 和 from 都是 路由信息对象
  }
})
```

```
const scrollBehavior = (to, from, savedPosition) => {
  if (savedPosition) {
    // savedPosition is only available for popstate navigations.
    return savedPosition
  } else {
    const position = {}
    // new navigation.
    // scroll to anchor by returning the selector
    if (to.hash) {
      position.selector = to.hash
    }
    // check if any matched route config has meta that requires scrolling to top
    if (to.matched.some(m => m.meta.scrollToTop)) {
      // cords will be used if no selector is provided,
      // or if the selector didn't match any element.
      position.x = 0
      position.y = 0
    }
    // if the returned position is falsy or an empty object,
    // will retain current scroll position.
    return position
  }
}
```

## 方法

###  导航钩子
增加全局的导航钩子
#### router.beforeEach(guard)
#### router.afterEach(hook)


### 编程式导航
动态的导航到一个新 url
#### router.push(location)
#### router.replace(location)
#### router.go(n) 到达
#### router.back() 后退
#### router.forward() 前进

```
router.getMatchedComponents(location?)

返回目标位置或是当前路由匹配的组件数组（是数组的定义/构造类，不是实例）。通常在服务端渲染的数据预加载时时候。

router.resolve(location, current?, append?)

2.1.0+

解析目标位置（格式和 <router-link> 的 to prop 一样），返回包含如下属性的对象：

{
  location: Location;
  route: Route;
  href: string;
}
router.addRoutes(routes)

2.2.0+

动态添加更多的路由规则。参数必须是一个符合 routes 选项要求的数组。

router.onReady(callback)

2.2.0+

添加一个会在第一次路由跳转完成时被调用的回调函数。此方法通常用于等待异步的导航钩子完成，比如在进行服务端渲染的时候。
```


- [router](https://router.vuejs.org/en/# "")
- []( "")

- []( "")
- []( "")
- []( "")