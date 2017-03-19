title: Vue  资源整理
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

## 介绍

Vue.js是一套构建用户界面的 渐进式框架。与其他重量级框架不同的是，Vue 采用自底向上增量开发的设计。Vue 的核心库只关注视图层，并且非常容易学习，非常容易与其它库或已有项目整合。另一方面，Vue 完全有能力驱动采用单文件组件和 Vue 生态系统支持的库开发的复杂单页应用。

适用环境：


## 初探


### 声明式渲染
```
<div id="app">
    {{ message }}
</div>

var app = new Vue({
    // 挂载目标
    el: '#app',
    // 响应数据
    data:{
        message: 'Hello Vue!'
    }
    // 计算属性函数
    computed: {
        // a computed getter
        reversedMessage: function () {
            // `this` points to the vm instance
            return this.message.split('').reverse().join('')
        }
    }
    // 过滤器函数
    filters: {
        capitalize: function (value) {
            if (!value) return ''
            value = value.toString()
            return value.charAt(0).toUpperCase() + value.slice(1)
        }
    }
    // 
    methods: {
        reverseMessage: function () {
            this.message = this.message.split('').reverse().join('')
        }
    }
    // 观察
    watch: {
        firstName: function (val) {
            this.fullName = val + ' ' + this.lastName
        },
        lastName: function (val) {
            this.fullName = this.firstName + ' ' + val
        }
    }
})
```


### 绑定 DOM 元素属性
v-bind 属性被称为指令。指令带有前缀 v-
```
<div id="app-2">
  <span v-bind:title="message">
    Hover your mouse over me for a few seconds to see my dynamically bound title!
  </span>
</div>

var app2 = new Vue({
  el: '#app-2',
  data:{
    message: 'You loaded this page on ' + new Date()
  }
})
```
### 数据更新
```
app2.message = 'some new message';
```

### 条件

控制切换一个元素的显示也相当简单：
```
<div id="app-3">
  <p v-if="seen">Now you see me</p>
</div>
var app3 = new Vue({
  el: '#app-3',
  data:{
    seen: true
  }
})
```

### 循环
```
<div id="app-4">
  <ol>
    <li v-for="todo in todos">
      {{ todo.text }}
    </li>
  </ol>
</div>
var app4 = new Vue({
  el: '#app-4',
  data:{
    todos: [
      { text: 'Learn JavaScript' },
      { text: 'Learn Vue' },
      { text: 'Build something awesome' }
    ]
  }
})
```

```
app4.todos.push({ text: 'New item' })
```

### 事件绑定
```
<div id="app-5">
  <p>{{ message }}</p>
  <button v-on:click="reverseMessage">Reverse Message</button>
</div>
var app5 = new Vue({
  el: '#app-5',
  data:{
    message: 'Hello Vue.js!'
  },
  methods: {
    reverseMessage: function () {
      this.message = this.message.split('').reverse().join('')
    }
  }
})
```


### 数据绑定v-model
```
<div id="app-6">
  <p>{{ message }}</p>
  <input v-model="message">
</div>
var app6 = new Vue({
  el: '#app-6',
  data:{
    message: 'Hello Vue!'
  }
})
```

### 用组件构建（应用）

组件实例：
```
<div id="app-7">
    <ol>
        // 使用组件
        <todo-item v-for="items in groceryList" v-bind:todo="items" :key="items.id"></todo-item>
    </ol>
</div>
// 数据
var app7 = new Vue({
    el: '#app-7',
    data:{
        groceryList: [
            { text: 'Vegetables' },
            { text: 'Cheese' },
            { text: 'Whatever else humans are supposed to eat' }
        ]
    }
})

// todo-item组件
Vue.component('todo-item', {
    props: ['todo'],
    template: '<li>{{ todo.text }}</li>'
})
```

```
<div id="app">
    <app-nav></app-nav>
    <app-view>
        <app-sidebar v-bind:todo="props"></app-sidebar>
        <app-content v-bind:todo="props"></app-content>
    </app-view>
</div>
```

## 模板语法

### 传值

#### 文本
数据绑定最常见的形式就是使用 “Mustache” 语法（双大括号）的文本插值：{{ msg }}，内容自动更新
```
<span>Message: {{ msg }}</span>
```

> v-once

内容不会自动更新
```
<span v-once>This will never change: {{ msg }}</span>
```
```
<div id="app" >
    <p>Original message : {{message}}</p>
    <p>Reversed message : {{ReversedMessage}}</p>
</div>

var app = new Vue({
    el: '#app',
    data:{
        message: 'hello world!',
    },
    computed:{
        ReversedMessage:function(){
            return  this.message.split('').reverse().join('')
        }
    }
})
```
#### 纯 HTML

双大括号会将数据解释为纯文本，而非 HTML 。为了输出真正的 HTML ，你需要使用 v-html 指令：
```
<div v-html="rawHtml"></div>
```
被插入的内容都会被当做 HTML —— 数据绑定会被忽略。注意，你不能使用 v-html 来复合局部模板，因为 Vue 不是基于字符串的模板引擎。组件更适合担任 UI 重用与复合的基本单元。
你的站点上动态渲染的任意 HTML 可能会非常危险，因为它很容易导致 XSS 攻击。请只对可信内容使用 HTML 插值，绝不要对用户提供的内容插值。
```
<div id = "app">
    {{msg}}
    <div v-html="hi"></div>
</div>
var app  = new Vue({
    el: '#app',
    data:{
        msg: 'hello world!',
        hi:'<h1>hi</h1>'
  }
})
```
#### 属性

Mustache 不能在 HTML 属性中使用，应使用 v-bind 指令：
```
<div v-bind:id="dynamicId"></div>
```

这对布尔值的属性也有效 —— 如果条件被求值为 false 的话该属性会被移除：
```
<button v-bind:disabled="someDynamicCondition">Button</button>
// <button disabled="disabled">Button</button>
```

```
<div id="app-2">
    <span v-bind:title="message">
        Hover your mouse over me for a few seconds to see my dynamically bound title!
    </span>
</div>

var app2 = new Vue({
    el: '#app-2',
    data:{
        message: 'You loaded this page on ' + new Date()
    }
})
```

>使用 JavaScript 表达式

迄今为止，在我们的模板中，我们一直都只绑定简单的属性键值。但实际上，对于所有的数据绑定， Vue.js 都提供了完全的 JavaScript 表达式支持。

```
{{ number + 1 }}
{{ ok ? 'YES' : 'NO' }}
{{ message.split('').reverse().join('') }}
<div v-bind:id="'list-' + id"></div>
这些表达式会在所属 Vue 实例的数据作用域下作为 JavaScript 被解析。有个限制就是，每个绑定都只能包含单个表达式，所以下面的例子都不会生效。

false
<!-- 这是语句，不是表达式 -->
{{ var a = 1 }}
<!-- 流控制也不会生效，请使用三元表达式 -->
{{ if (ok) { return message } }}
```

模板表达式都被放在沙盒中，只能访问全局变量的一个白名单，如 Math 和 Date 。你不应该在模板表达式中试图访问用户定义的全局变量。

```
<!-- 绑定一个属性 -->
<img v-bind:src="imageSrc">
<!-- 缩写 -->
<img :src="imageSrc">
<!-- with inline string concatenation -->
<img :src="'/path/to/images/' + fileName">
<!-- class 绑定 -->
<div :class="{ red: isRed }"></div>
<div :class="[classA, classB]"></div>
<div :class="[classA, { classB: isB, classC: isC }]">
<!-- style 绑定 -->
<div :style="{ fontSize: size + 'px' }"></div>
<div :style="[styleObjectA, styleObjectB]"></div>
<!-- 绑定一个有属性的对象 -->
<div v-bind="{ id: someProp, 'other-attr': otherProp }"></div>
// <div id="someProp" other-attr="otherProp"></div>
<!-- 通过 prop 修饰符绑定 DOM 属性 -->
<div v-bind:text-content.prop="text"></div>
<!-- prop绑定. “prop” 必须在my-component组件中声明。 someThing为数据-->
<my-component :prop="someThing"></my-component>
<!-- XLink -->
<svg><a :xlink:special="foo"></a></svg>
```

### 指令Directives
```
```
带有 v- 前缀的特殊属性。
指令属性的值预期是单一 JavaScript 表达式（除了v-for，之后再讨论）。
指令的职责就是当其表达式的值改变时相应地将某些行为应用到DOM上。让我们回顾一下在介绍里的例子：
```
<p v-if="seen">Now you see me</p>
```
根据表达式seen的值的真假来移除/插入<p>元素。


#### 参数
> bind

一些指令能接受一个“参数”，在指令后以冒号指明。例如， v-bind 指令被用来响应地更新 HTML 属性：
```
<a v-bind:href="url"></a>
```
在这里 href 是参数，告知 v-bind 指令将该元素的 href 属性与表达式 url 的值绑定。
> on

另一个例子是 v-on 指令，它用于监听 DOM 事件：
```
<a v-on:click="doSomething">
```
在这里参数是监听的事件名。我们也会更详细地讨论事件处理。

#### 修饰符
修饰符（Modifiers）是以半角句号 . 指明的特殊后缀，用于指出一个指定应该以特殊方式绑定。例如，.prevent 修饰符告诉 v-on 指令对于触发的事件调用 event.preventDefault()：
```
<form v-on:submit.prevent="onSubmit"></form>
```
