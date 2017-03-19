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

### 过滤器

Vue.js 允许你自定义过滤器，被用作一些常见的文本格式化。过滤器应该被添加在 mustache 插值的尾部，由“管道符”指示：
messages为传送的数据，capitalize作为过滤的方法。
```
{{ message | capitalize }}
<!-- in mustaches -->
{{ message | capitalize }}
<!-- in v-bind -->
<div v-bind:id="rawId | formatId"></div>
```

Vue 2.x 中，过滤器只能在 mustache 绑定和 v-bind 表达式（从 2.1.0 开始支持）中使用，因为过滤器设计目的就是用于文本转换。为了在其他指令中实现更复杂的数据变换，你应该使用计算属性。
```
<div id="app" >
    <p v-if='seen'>{{message | capitalize}}</p>
</div>

var app = new Vue({
    el: '#app',
    data:{
        message: 'hello world!',
        seen:true,
    },
    filters:{
        // 过滤器函数总接受表达式的值作为第一个参数。value = message
        capitalize:function(value){
            if(!value) return ''
            value = value.toString()
            return value.charAt(0).toUpperCase() + value.slice(1)
        }
    }
})
```
```
{{ message | filterA | filterB }}
过滤器是 JavaScript 函数，因此可以接受参数：
{{ message | filterA('arg1', arg2) }}
这里，字符串 'arg1' 将传给过滤器作为第二个参数， arg2 表达式的值将被求值然后传给过滤器作为第三个参数。
```

### 缩写

### v-bind 缩写
```
<!-- 完整语法 -->
<a v-bind:href="url"></a>
<!-- 缩写 -->
<a :href="url"></a>
```


### v-on 缩写
```
<!-- 完整语法 -->
<a v-on:click="doSomething"></a>
<!-- 缩写 -->
<a @click="doSomething"></a>
```

## 事件处理器

### 事件处理操作
```
<div id="example-1">
    <button @click="counter += 1">增加 1</button>
    <p>这个按钮被点击了 {{ counter }} 次。</p>
</div>

var example1 = new Vue({
    el: '#example-1',
    data:{
        counter: 0
    }
})
```

### 事件绑定
```
<div id="example-2">
    <!-- `greet` 是在下面定义的方法名 -->
    <button v-on:click="greet">Greet</button>
</div>

var example2 = new Vue({
    el: '#example-2',
    data:{
        name: 'Vue.js'
    },
    // 在 `methods` 对象中定义方法
    methods: {
        greet: function (event) {
            // `this` 在方法里指当前 Vue 实例
            alert('Hello ' + this.name + '!')
            // `event` 是原生 DOM 事件
            alert(event.target.tagName)
        }
    }
})

// 也可以用 JavaScript 直接调用方法
example2.greet() //
```

### 内联处理器方法
```
<div id="example-3">
    <button v-on:click="say('hi')">Say hi</button>
    <button v-on:click="say('what')">Say what</button>
</div>

new Vue({
    el: '#example-3',
    methods: {
        say: function (message) {
            alert(message)
        }
    }
})
```
特殊变量 $event
```
<button v-on:click="warn('Form cannot be submitted yet.', $event)">Submit</button>
// ...
methods: {
    warn: function (message, event) {
        // 现在我们可以访问原生事件对象
        if (event) event.preventDefault()
        alert(message)
    }
}
```

### 事件修饰符
methods 只有纯粹的数据逻辑，而不是去处理 DOM 事件细节
```
<!-- 阻止单击事件冒泡 -->
<a v-on:click.stop="doThis"></a>
<div id="example-3" v-on:click="alert('hi2')">
    <a href="https://www.baidu.com/" v-on:click.stop="alert('hi1')">Say hi</a>
</div>
new Vue({
    el: '#example-3',
})

<!-- 提交事件不再重载页面 -->
<form v-on:submit.prevent="onSubmit"></form>
<!-- 修饰符可以串联  -->
<a v-on:click.stop.prevent="doThat"></a>
<!-- 只有修饰符 -->
<form v-on:submit.prevent></form>
<!-- 添加事件侦听器时使用事件捕获模式 -->
<div v-on:click.capture="doThis">...</div>
<!-- 只当事件在该元素本身（而不是子元素）触发时触发回调 -->
<div v-on:click.self="doThat">...</div>

<!-- the click event will be triggered at most once -->
<a v-on:click.once="doThis"></a>
```
### 按键修饰符
```
<!-- 只有在 keyCode 是 13 时调用 vm.submit() -->
<input v-on:keyup.13="submit">
```

```
<!-- 同上 -->
<input v-on:keyup.enter="submit">
<!-- 缩写语法 -->
<input @keyup.enter="submit">
<div id="example-3">
    <input v-model="Inputs" v-on:keyup.enter="submit">
    <div>{{Inputs}}</div>
    <div>{{todo}}</div>
</div>
new Vue({
    el: '#example-3',
    data:{
        Inputs: '',
        todo: 'dfdsf'
    },
    methods: {
        submit: function (value) {
            console.log(this.Inputs)
            this.todo = this.Inputs
        }
    }
})
```

```
.enter
.tab
.delete (捕获 “删除” 和 “退格” 键)
.esc
.space
.up
.down
.left
.right

// 可以使用 v-on:keyup.f1
Vue.config.keyCodes.f1 = 112
```

### 按键修饰符
```
.ctrl
.alt
.shift
.meta
<!-- Alt + C -->
<input @keyup.alt.67="clear">
<!-- Ctrl + Click -->
<div @click.ctrl="doSomething">Do something</div>
```

## 绑定 HTML Class

### 对象语法
```
<div v-bind:class="{ active: isActive }"></div>
```

```
<div class="static" v-bind:class="{ active: isActive, 'text-danger': hasError }"></div>
```
isActive控制Class是否写入
```
data:{
  isActive: true,
  hasError: false
}
<div class="static active"></div>
```

> 对象

```
<div v-bind:class="classObject"></div>

data:{
  classObject: {
    active: true,
    'text-danger': false
  }
}
```

> 计算属性

```
<div v-bind:class="classObject"></div>

data:{
  isActive: true,
  error: null
},
computed: {
  classObject: function () {
    return {
      active: this.isActive && !this.error,
      'text-danger': this.error && this.error.type === 'fatal',
    }
  }
}
```

### 数组语法

> 数组赋值

```
<div v-bind:class="[activeClass, errorClass]">

data:{
    activeClass: 'active',
    errorClass: 'text-danger'
}

<div class="active text-danger"></div>
```

> 三元表达式

```
<div v-bind:class="[isActive ? activeClass : '', errorClass]">

data:{
    isActive: false,
    activeClass: 'active',
    errorClass: 'text-danger'
}

<div class="text-danger"></div>
```

```
<div v-bind:class="[{ active: isActive }, errorClass]">

data:{
    isActive: false,
    activeClass: 'active',
    errorClass: 'text-danger'
}

<div class="text-danger"></div>
```
### 用在组件上
```
<my-component class="baz boo"></my-component>
Vue.component('my-component', {
  template: '<p class="foo bar">Hi</p>'
})

<p class="foo bar baz boo">Hi</p>
```
```
<my-component class="baz boo"></my-component>
<my-component v-bind:class="{ active: isActive }"></my-component>
data:{
    isActive: false,
}

<p class="foo bar active"></p>
```

## 绑定内联样式

### 对象语法
CSS 属性名可以用驼峰式（camelCase）或短横分隔命名（kebab-case）
```
<div v-bind:style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>

data:{
    activeColor: 'red',
    fontSize: 30
}
```

```
<div v-bind:style="styleObject"></div>
data:{
    styleObject: {
        color: 'red',
        fontSize: '13px'
    }
}
```

### 数组语法
```
<div v-bind:style="[baseStyles, overridingStyles]">
```

### 自动添加前缀
当 v-bind:style 使用需要特定前缀的 CSS 属性时，如 transform，"moz "Vue.js会自动侦测并添加相应的前缀。

## 条件渲染

### v-if
```
<h1 v-if="ok">Yes</h1>
<h1 v-else>No</h1>
```

> <template> 中 v-if 条件组

```
<template v-if="ok">
  <h1>Title</h1>
  <p>Paragraph 1</p>
  <p>Paragraph 2</p>
</template>
```

> v-else-if

```
<div v-if="type === 'A'">A</div>
<div v-else-if="type === 'B'">B</div>
<div v-else-if="type === 'C'">C</div>
<div v-else>Not A/B/C</div>
```

> 使用 key 控制元素的可重用

```
<div id="keyDiv-demo">
    <div>
        <template v-if="loginType === 'username'">
            <label>Username</label>
            <input  placeholder="Enter your username">
        </template>
        <template v-else>
            <label>Email</label>
            <input placeholder="Enter your email address">
        </template>
    </div>
    <button v-on:click="changeName">Toggle login type</button>
</div>
var vueIfKey = new Vue({
    el: '#keyDiv-demo',
    data: {
        loginType: 'username',
        n: 0
    },
    methods: {
        changeName: function(){
            this.n += 1;
            this.loginType = this.n%2 ? 'email' : 'username';
        }
    }
})
```
这样调用的是同一个input！
```
<div id="keyDiv-demo">
    <div>
        <template v-if="loginType === 'username'">
            <label>Username</label>
            <input placeholder="Enter your username" key="username-input">
        </template>
        <template v-else>
            <label>Email</label>
            <input placeholder="Enter your email address" key="email-input">
        </template>
    </div>
    <button v-on:click="changeName">Toggle login type</button>
</div>
var vueIfKey = new Vue({
    el: '#keyDiv-demo',
    data: {
        loginType: 'username',
        n: 0
    },
    methods: {
        changeName: function(){
            this.n += 1;
            this.loginType = this.n%2 ? 'email' : 'username';
        }
    }
})
```

```
<div id="keyDiv-demo">
    <div>
        <template v-if="loginType === 'username'">
            <label>Username</label>
            <input v-model="userName" placeholder="Enter your username">
        </template>
        <template v-else>
            <label>Email</label>
            <input v-model="eMail" placeholder="Enter your email address">
        </template>
    </div>
    <button v-on:click="changeName">Toggle login type</button>
</div>
var vueIfKey = new Vue({
    el: '#keyDiv-demo',
    data: {
        loginType: 'username',
        userName: '',
        eMail: '',
        n: 0
    },
    methods: {
        changeName: function(){
            this.n += 1;
            this.loginType = this.n%2 ? 'email' : 'username';
        }
    }
})
```

```
```
### v-show

```
<h1 v-show="ok">Hello!</h1>
```
不同的是有 v-show 的元素会始终渲染并保持在 DOM 中。v-show 是简单的切换元素的 CSS 属性 display 。
##### 注意 v-show 不支持 <template> 语法。

### v-if vs v-show

v-if 是真实的条件渲染，因为它会确保条件块在切换当中适当地销毁与重建条件块内的事件监听器和子组件。
v-if 也是惰性的：如果在初始渲染时条件为假，则什么也不做——在条件第一次变为真时才开始局部编译（编译会被缓存起来）。
相比之下， v-show 简单得多——元素始终被编译并保留，只是简单地基于 CSS 切换。
一般来说， v-if 有更高的切换消耗而 v-show 有更高的初始渲染消耗。
因此，如果需要频繁切换使用 v-show 较好，如果在运行时条件不大可能改变则使用 v-if 较好。


## 列表渲染

### v-for

我们用 v-for 指令根据一组数组的选项列表进行渲染。 v-for 指令需要以 item in items 形式的特殊语法， items 是源数据数组并且 item 是数组元素迭代的别名。
```
<ul id="example-1">
    <li v-for="item in items">
        {{ item }}
    </li>
</ul>
var example1 = new Vue({
    el: '#example-1',
    data: {
        items: ['Foo','Foo','Foo']
    }
})

<ul id="example-1">
    <li v-for="item in items">
        {{ item.message }}
    </li>
</ul>
var example1 = new Vue({
    el: '#example-1',
    data: {
        items: [
            {message: 'Foo' },
            {message: 'Bar' }
        ]
    }
})
// Foo
// Bar
```

对父作用域属性的完全访问权限。 v-for 还支持一个可选的第二个参数为当前项的索引key。
```
<ul id="example-2">
    <li v-for="(item, index) in items">
        {{ parentMessage }} - {{ index }} - {{ item.message }}
    </li>
</ul>
var example2 = new Vue({
    el: '#example-2',
    data: {
        parentMessage: 'Parent',
        items: [
            { message: 'Foo' },
            { message: 'Bar' }
        ]
    }
})
// Parent - 0 - Foo
// Parent - 1 - Bar
```
```
<ul id="repeat-object" class="demo">
    <li v-for="value in object">
        <div>{{ value.FirstName }}</div>
        <div>{{ value.LastName }}</div>
        <div>{{ value.Age }}</div>
    </li>
</ul>
new Vue({
    el: '#repeat-object',
    data: {
        object: [
            {FirstName: 'John',LastName: 'Doe',Age: 30},
            {FirstName: 'John',LastName: 'Doe',Age: 30},
            {FirstName: 'John',LastName: 'Doe',Age: 30},
        ]
    }
})
// John
// Doe
// 30
```

### of 替代 in
```
<div v-for="item of items"></div>
```

### Template v-for

```
<ul id="example-2">
    <template v-for="item in items">
        <li>{{ item.msg }}</li>
        <li class="divider">divider</li>
    </template>
</ul>

var example2 = new Vue({
    el: '#example-2',
    data: {
        items: [
            { msg: 'Foo' },
            { msg: 'Bar' }
        ]
    }
})
```

### 对象迭代 v-for

```
<ul id="repeat-object" class="demo">
    <li v-for="value in object">
        {{ value }}
    </li>
</ul>
new Vue({
    el: '#repeat-object',
    data: {
        object: {
            FirstName: 'John',
            LastName: 'Doe',
            Age: 30
        }
    }
})
// John
// Doe
// 30
```

```
你也可以提供第二个的参数为键名：
<div v-for="(value, key) in object">
    {{ key }} : {{ value }}
</div>

第三个参数为索引：
<div v-for="(value, key, index) in object">
    {{ index }}. {{ key }} : {{ value }}
</div>
```

### 整数迭代 v-for
v-for 也可以取整数。在这种情况下，它将重复多次模板。
```
<div>
    <span v-for="n in 10">{{ n }}</span>
</div>
// 1 2 3 4 5 6 7 8 9 10
```

### 组件 和 v-for
然而他不能自件动传递数据到组里，因为组件有自己独立的作用域。为了传递迭代数据到组件里，我们要用 props ：
```
<my-component v-for="item in items"></my-component>

<my-component v-for="(item, index) in items" v-bind:item="item" v-bind:index="index"></my-component>
```

#### todo list
```
<div id="todo-list-example">
    <input v-model="newTodoText" @keyup.enter="addNewTodo" placeholder="Add a todo">
    <ul>
        <li is="todo-item" v-for="(todo,index) in todos" :title="todo" @remove="todo.splice(index,1)"></li>
    </ul>
</div>

Vue.component('todo-item',{
    template: '\
        <li>\
            {{ title }}\
            <button v-on:click="$emit(\'remove\')">X</button>\
        </li>\
    ',
    props: ['title']
})
new Vue({
    el: '#todo-list-example',
    data: {
        newTodoText: '',
        todos: [
            'Do the dishes',
            'Take out the trash',
            'Mow the lawn'
        ]
    },
    methods: {
        addNewTodo: function(){
            if(this.newTodoText){
                this.todos.push(this.newTodoText)
                this.newTodoText = ''
            }else{
                popup();
            }
        },
        popup: function(){
        }
    }
})
// Do the dishes  X
// Take out the trash  X
// Mow the lawn  X
```

当 Vue.js 用 v-for 正在更新已渲染过的元素列表时，它默认用 “就地复用” 策略。如果数据项的顺序被改变，Vue将不是移动 DOM 元素来匹配数据项的顺序， 而是简单复用此处每个元素，并且确保它在特定索引下显示已被渲染过的每个元素。这个类似 Vue 1.x 的 track-by="$index" 。
这个默认的模式是有效的，但是只适用于不依赖子组件状态或临时 DOM 状态（例如：表单输入值）的列表渲染输出。
为了给 Vue 一个提示，以便它能跟踪每个节点的身份，从而重用和重新排序现有元素，你需要为每项提供一个唯一 key 属性。理想的 key 值是每项都有唯一 id。这个特殊的属性相当于 Vue 1.x 的 track-by ，但它的工作方式类似于一个属性，所以你需要用 v-bind 来绑定动态值（在这里使用简写）：

```
<div v-for="item in items" :key="item.id">
  <!-- 内容 -->
</div>
```
建议尽可能使用 v-for 来提供 key ，除非迭代 DOM 内容足够简单，或者你是故意要依赖于默认行为来获得性能提升。
因为它是 Vue 识别节点的一个通用机制， key 并不特别与 v-for 关联，key 还具有其他用途，我们将在后面的指南中看到其他用途。
数组更新检测

### 
```
变异方法

Vue 包含一组观察数组的变异方法，所以它们也将会触发视图更新。这些方法如下：
push()
pop()
shift()
unshift()
splice()
sort()
reverse()
你打开控制台，然后用前面例子的 items 数组调用变异方法：example1.items.push({ message: 'Baz' }) 。
重塑数组

变异方法(mutation method)，顾名思义，会改变被这些方法调用的原始数组。相比之下，也有非变异(non-mutating method)方法，例如： filter(), concat(), slice() 。这些不会改变原始数组，但总是返回一个新数组。当使用非变异方法时，可以用新数组替换旧数组：
example1.items = example1.items.filter(function (item) {
  return item.message.match(/Foo/)
})
你可能认为这将导致 Vue 丢弃现有 DOM 并重新渲染整个列表。幸运的是，事实并非如此。 Vue 实现了一些智能启发式方法来最大化 DOM 元素重用，所以用一个含有相同元素的数组去替换原来的数组是非常高效的操作。
注意事项

由于 JavaScript 的限制， Vue 不能检测以下变动的数组：
当你利用索引直接设置一个项时，例如： vm.items[indexOfItem] = newValue
当你修改数组的长度时，例如： vm.items.length = newLength
为了避免第一种情况，以下两种方式将达到像 vm.items[indexOfItem] = newValue 的效果， 同时也将触发状态更新：
// Vue.set
Vue.set(example1.items, indexOfItem, newValue)
// Array.prototype.splice`
example1.items.splice(indexOfItem, 1, newValue)
避免第二种情况，使用 splice：
example1.items.splice(newLength)
显示过滤/排序结果

有时，我们想要显示一个数组的过滤或排序副本，而不实际改变或重置原始数据。在这种情况下，可以创建返回过滤或排序数组的计算属性。
例如：
<li v-for="n in evenNumbers">{{ n }}</li>
data: {
  numbers: [ 1, 2, 3, 4, 5 ]
},
computed: {
  evenNumbers: function () {
    return this.numbers.filter(function (number) {
      return number % 2 === 0
    })
  }
}
或者，你也可以在计算属性不适用的情况下 (例如，在嵌套 v-for 循环中) 使用 method 方法：
<li v-for="n in even(numbers)">{{ n }}</li>
data: {
  numbers: [ 1, 2, 3, 4, 5 ]
},
methods: {
  even: function (numbers) {
    return numbers.filter(function (number) {
      return number % 2 === 0
    })
  }
}


```

[]( "")