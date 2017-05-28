title: Vue Transition过渡效果
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

[ ] 简单罗列些方法
<!-- more -->
## 基本使用
Vue 提供了 transition 的封装组件，在下列情形中，可以给任何元素和组件添加 entering/leaving 过渡

### 组件封装
```
<transition name="fade"></transition>

设置时间
.fade-enter-active, .fade-leave-active {
  transition: opacity .5s
}
.fade-enter, .fade-leave-active {
  opacity: 0
}
```

### 过渡的-CSS-类名

```

v-enter: 定义进入过渡的开始状态。在元素被插入时生效，在下一个帧移除。

v-enter-active: 定义进入过渡的结束状态。在元素被插入时生效，在 transition/animation 完成之后移除。

v-leave: 
定义离开过渡的开始状态。在离开过渡被触发时生效，在下一个帧移除。

v-leave-active: 定义离开过渡的结束状态。在离开过渡被触发时生效，在 transition/animation 完成之后移除。
```

## 过渡

### 
```
.router-slid-enter-active, .router-slid-leave-active {
  transition: all .4s;
}
.router-slid-enter, .router-slid-leave-active {
	transform: translate3d(2rem, 0, 0);
	opacity: 0;
}
```

## 动效
### 右侧淡入elm
```
<transition name="router-ali" mode="out-in">
  <router-view></router-view>
</transition>

.router-slid-enter-active, .router-slid-leave-active {
  transition: all .4s;
}
.router-slid-enter, .router-slid-leave-active {
  transform: translate3d(2rem, 0, 0);
  opacity: 0;
}
```

### 左右移动Vux
使用vuex控制进出的状态
```
<transition :name="'vux-pop-' + (direction === 'forward' ? 'in' : 'out')">
  <router-view class="router-view"></router-view>
</transition>

/**
* vue-router transition
*/
.router-view {
  width: 100%;
  animation-duration: 0.5s;
  animation-fill-mode: both;
  backface-visibility: hidden;
}
.vux-pop-out-enter-active,
.vux-pop-out-leave-active,
.vux-pop-in-enter-active,
.vux-pop-in-leave-active {
  will-change: transform;
  height: 100%;
  position: absolute;
  left: 0;
}
.vux-pop-out-enter-active {
  animation-name: popInLeft;
}
.vux-pop-out-leave-active {
  animation-name: popOutRight;
}
.vux-pop-in-enter-active {
  perspective: 1000;
  animation-name: popInRight;
}
.vux-pop-in-leave-active {
  animation-name: popOutLeft;
}
@keyframes popInLeft {
  from {
    opacity: 0;
    transform: translate3d(-100%, 0, 0);
  }
  to {
    opacity: 1;
    transform: translate3d(0, 0, 0);
  }
}
@keyframes popOutLeft {
  from {
    opacity: 1;
  }
  to {
    opacity: 0;
    transform: translate3d(-100%, 0, 0);
  }
}
@keyframes popInRight {
  from {
    opacity: 0;
    transform: translate3d(100%, 0, 0);
  }
  to {
    opacity: 1;
    transform: translate3d(0, 0, 0);
  }
}
@keyframes popOutRight {
  from {
    opacity: 1;
  }
  to {
    opacity: 0;
    transform: translate3d(100%, 0, 0);
  }
}
```

### 
```
```

## 概括
Vue.js 是用于构建交互式的 Web  界面的库。它提供了 MVVM 数据绑定和一个可组合的组件系统，具有简单、灵活的 API。从技术上讲， Vue.js 集中在 MVVM 模式上的视图模型层，并通过双向数据绑定连接视图和模型。实际的 DOM 操作和输出格式被抽象出来成指令和过滤器。相比其它的 MVVM 框架，Vue.js 更容易上手。
## 官方文档
- [vuejs中文官网](http://cn.vuejs.org/ "") 
- [vuejs英文官网](http://vuejs.org/ "") 
- [vuejs组织](https://github.com/vuejs "")
- [Vuejs2.0 文档攻略](http://larabase.com/ "")
- [Vue.2.0.5-过渡效果](http://www.cnblogs.com/jiangxiaobo/p/6076652.html "")
- [Vue.js每天必学之过渡与动画](http://www.jb51.net/article/92039.htm "")
- [初学vue2.0做一个很简单的过](http://www.jianshu.com/p/fe012c4ead3b "")
- []( "")
- []( "")
