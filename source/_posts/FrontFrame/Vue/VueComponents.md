title: Vue组件探秘
date: 2017-08-18 18:29:00
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
　　****文章适合有一定vue经验，只是简单介绍项目中的搭建与开发的优化之处。知识点，请自行查阅！
<!-- more -->

# 基础

## 属性Props


# 探秘

## Props验证
```
Vue.component('example', {
  props: {
    // 基础类型检测 (`null` 意思是任何类型都可以)
    propA: Number,
    // 多种类型
    propB: [String, Number],
    // 必传且是字符串
    propC: {
      type: String,
      required: true
    },
    // 数字，有默认值
    propD: {
      type: Number,
      default: 100
    },
    // 数组/对象的默认值应当由一个工厂函数返回
    propE: {
      type: Object,
      default: function () {
        return { message: 'hello' }
      }
    },
    // 自定义验证函数
    propF: {
      validator: function (value) {
        return value > 10
      }
    }
  }
})
```

### type
可以是下面原生构造器
```
String
Number
Boolean
Function
Object
Array
Symbol
```

## v-on:

### 绑定原生事件
如果想在某个组件的根元素上监听一个原生事件。可以使用 .native 修饰 v-on
```
<my-component v-on:click.native="doTheThing"></my-component>
```

### 自定义事件
this.$emit分发事件
```
<div id="app3">
    <my-component2 v-on:myclick="onClick"></my-component2>
</div>
<script>
  Vue.component('my-component2', {
    template: `<div>
    <button type="button" @click="childClick">点击我触发自定义事件</button>
    </div>`,
    methods: {
      childClick () {
        this.$emit('myclick', '这是我暴露出去的数据', '这是我暴露出去的数据2')
      }
    }
  })
  new Vue({
    el: '#app3',
    methods: {
      onClick () {
        console.log(arguments)
      }
    }
  })
</script>
```
1. 组件内部方法，触发外部自定义方法

```
this.$emit('myclick', 'data1', 'data2')
<!-- 第一个参数是自定义事件的名字 -->
<!-- 后面的参数是依次想要发送出去的数据 -->
```

1. 父组件利用v-on为事件绑定处理器

```
<my-component2 v-on:myclick="onClick"></my-component2>
```
在使用v-on绑定事件处理方法时，不应该传进任何参数，而是直接写v-on:myclick="onClick",不然，子组件暴露出来的数据就无法获取到了

## v-model
v-model是一个十分强大的指令,它可以自动让原生表单组件的值自动和你选择的值绑定

```
<input v-model="message" placeholder="edit me">
<p>Message is: {{ message }}</p>
```

### 使用自定义事件的表单输入组件

```
<input v-model="something">

<input
  v-bind:value="something"
  v-on:input="something = $event.target.value">
```

非常简单的货币输入的自定义控件：
```
<currency-input v-model="price"></currency-input>

Vue.component('currency-input', {
  template: '\
    <span>\
      $\
      <input\
        ref="input"\
        v-bind:value="value"\
        v-on:input="updateValue($event.target.value)"\
      >\
    </span>\
  ',
  props: ['value'],
  methods: {
    // 不是直接更新值，而是使用此方法来对输入值进行格式化和位数限制
    updateValue: function (value) {
      var formattedValue = value
        // 删除两侧的空格符
        .trim()
        // 保留 2 小数位
        .slice(
          0,
          value.indexOf('.') === -1
            ? value.length
            : value.indexOf('.') + 3
        )
      // 如果值不统一，手动覆盖以保持一致
      if (formattedValue !== value) {
        this.$refs.input.value = formattedValue
      }
      // 通过 input 事件发出数值
      this.$emit('input', Number(formattedValue))
    }
  }
})
```

### 修饰符
```
<!-- 在 "change" 而不是 "input" 事件中更新 -->
<input v-model.lazy="msg" >

<!-- 自动将用户的输入值转为 Number 类型 -->
<input v-model.number="age" type="number">

<!-- 自动过滤用户输入的首尾空格 -->
<input v-model.trim="msg">
```





```
<div id="app4">
	<input type="text" v-bind:value="text" v-on:input="changeValue($event.target.value)">
	{{text}}
</div>
<script>
  new Vue({
    el: '#app4',
    data: {
      text: '444'
    },
    methods: {
      changeValue (value) {
        this.text = value
      }
    }
  })
</script>
```

```
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8" />
    <title>vue.js component</title>
</head>
<body>
<!-- 子组件模板 -->
<template id="child-template">
    <input v-model='msg' />
    <button v-on:click="notify">Dispatch Event</button>
</template>
<!-- 父组件模板 -->
<div id="events-example">
    <p>Messages: {{messages | json}}</p>
    <child></child>
</div>
<div id="example">
</div>
<script type="text/javascript" src="../js/vue.js"></script>
<script type="text/javascript">
	Vue.component('child', {
	    template: '#child-template',
	    data: function () {
	        return {
	            msg: 'hello'
	        }
	    },
	    methods: {
	        notify: function () {
	            if (this.msg.trim()) {
	                this.$dispatch('child-msg', this.msg);
	                this.msg = '';
	            }
	        }
	    }
	});

	var parent = new Vue({
	    el: '#events-example',
	    data: {
	        messages: [],
	    },
	    events: {
	        'child-msg': function (msg) {
	            this.messages.push(msg);
	        }
	    }
	});
</script>
</body>
</html>
```

```
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8" />
    <title>vue.js component</title>
</head>
<body>
<!-- 子组件模板 -->
<template id="child-template">
    <input v-model='msg' />
    <button v-on:click="notify">Dispatch Event</button>
</template>
<!-- 父组件模板 -->
<div id="events-example">
    <p>Messages: {{messages | json}}</p>
    <child v-on:child-msg="handleIt"></child>
</div>
<div id="example">
</div>
<script type="text/javascript" src="../js/vue.js"></script>
<script type="text/javascript">
	Vue.component('child', {
	    template: '#child-template',
	    data: function () {
	        return {
	            msg: 'hello'
	        }
	    },
	    methods: {
	        notify: function () {
	            if (this.msg.trim()) {
	                this.$dispatch('child-msg', this.msg);
	                this.msg = '';
	            }
	        }
	    }
	});

	var parent = new Vue({
	    el: '#events-example',
	    data: {
	        messages: [],
	    },
	    methods: {
	        'handleIt': function () {
	            console.log('a');
	        }
	    }
	});
</script>
</body>
</html>
```

- [深刻理解Vue中的组件](https://segmentfault.com/a/1190000010527064 "")
- [表单控件绑定](https://cn.vuejs.org/v2/guide/forms.html#绑定-value "")
- []( "")
- []( "")
- []( "")
- []( "")