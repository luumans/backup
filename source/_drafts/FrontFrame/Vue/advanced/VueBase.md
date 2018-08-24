title: Vue 资源整理
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
　　**自用笔记：**Vue.js通过简洁的API提供高效的数据绑定和灵活的组件系统。最近在Github上看到了不少Vue的项目，很好奇，决定尝试尝试。
<!-- more -->

# 组件通信
## 父子组件通信
### 自定义事件$on、$emit

``` javascript
// 父组件
<single-address @edit-address="editAddress"></single-address>
```

``` javascript
// 子组件
methods: {
 editAddress () {
  this.$emit('edit-address', false)
 }
}
```

### props

``` javascript
// 父组件
<one-address :addressitems="addressitems"></one-address>

// 子组件
<div>{{ addressitems.partment }}{{ addressitems.address }}</div>
export default {
  props: {
    addressitems: Object
  }
}
```

## 非父子组件的通信
### EventBus

``` javascript
window.bus = new Vue()

// 组件A 触发
window.bus.$emit('id-selected', 1)

// 组件B 监听
window.bus.$on('id-selected', function (id) {
 console.log(id)
})
```

问题：如何组建来回切换会触发多次。
这个$on事件是不会自动清楚销毁的，需要我们手动来销毁。
bus.$off('id-selected')

问题：生命周期
下一个组建，渲染后注销组建，装载新组建。
created2 -> destory1 -> mounted2

## 复杂情况vuex

## 路由
params、query

``` javascript
this.$router.push({
	path: '',
	query: {
		index: 1
	},
	params: {
		index: 1
	}
})

```

# 怎么动态添加组件
## 



``` javascript
```