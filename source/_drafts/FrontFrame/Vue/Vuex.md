title: Vuex
date: 2017-04-25 18:29:00
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
　　**Vuex**是一个专为 Vue.js 应用程序开发的状态管理模式。它采用集中式存储管理应用的所有组件的状态，并以相应的规则保证状态以一种可预测的方式发生变化。
# TODO

[ ] 严格模式
[ ] 测试
[ ] 插件
[ ] 热重载
[ ] 
[x] 
<!-- more -->


# 开始
简单实用
```
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

const state = {
  count: 'jljdfdf'
}
export default new Vuex.Store({
  state
})
```

## 全局注入
```
npm install vuex --save

import store from './vuex/store'
new Vue({
  el: '#app',
  router,
  store,
  template: '<App/>',
  components: { App }
})
```

## 仓库管理
```
import Vue from 'vue'
import Vuex from 'vuex'
import modules from './modules'

Vue.use(Vuex)

export default new Vuex.Store({
  modules
})
```

## 模块化
```
import api from 'API';
import * as types from 'VUEX/mutation-types';

const state = {
}

const getters = {
}

const actions = {
}

const mutations = {
}

export default {
  state,
  getters,
  actions,
  mutations
}
```

## 状态管理
```
export const COM_NAV_STATUS = 'COM_NAV_STATUS'
export const COM_HEADER_STATUS = 'COM_HEADER_STATUS'
export const COM_LOADING_STATUS = 'COM_LOADING_STATUS'
```
## 流程
通过Getters映射，控制Actions改变状态，从而控制mutations状态控制数据变化。

# 概念
状态自管理应用包含以下几个部分：
1. state，驱动应用的数据源；
1. view，以声明方式将state映射到视图；
1. actions，响应在view上的用户输入导致的状态变化。

# store 仓库
"store"基本上就是一个容器，它包含着你的应用中大部分的状态(state)。

Vuex和单纯的全局对象有以下两点不同：
1. Vuex的状态存储是响应式的。当Vue组件从store中读取状态的时候，若store中的状态发生变化，那么相应的组件也会相应地得到高效更新。
1. 你不能直接改变store中的状态。改变store 中的状态的唯一途径就是显式地提交(commit) mutations。这样使得我们可以方便地跟踪每一个状态的变化，从而让我们能够实现一些工具帮助我们更好地了解我们的应用。




## 数据传输方式
“单向数据流”理念的极简示意

多个组件共享状态的缺点：
1. 传参的方法对于多层嵌套的组件将会非常繁琐，并且对于兄弟组件间的状态传递无能为力。
1. 我们经常会采用父子组件直接引用或者通过事件来变更和同步状态的多份拷贝。以上的这些模式非常脆弱，通常会导致无法维护的代码。

### 组件仍然保有局部状态
使用 Vuex 并不意味着你需要将所有的状态放入 Vuex。虽然将所有的状态放到 Vuex 会使状态变化更显式和易调试，但也会使代码变得冗长和不直观。如果有些状态严格属于单个组件，最好还是作为组件的局部状态。你应该根据你的应用开发需要进行权衡和确定。

this.$store.state
this.$store.commit('mutationName')


# Getters
有时候我们需要从 store 中的 state 中派生出一些状态，例如对列表进行过滤并计数：
```
computed: {
  doneTodosCount () {
    return this.$store.state.todos.filter(todo => todo.done).length
  }
}
```

Vuex 允许我们在 store 中定义『getters』（可以认为是 store 的计算属性）。Getters 接受 state 作为其第一个参数：
```
const store = new Vuex.Store({
  state: {
    todos: [
      { id: 1, text: '...', done: true },
      { id: 2, text: '...', done: false }
    ]
  },
  getters: {
    doneTodos: state => {
      return state.todos.filter(todo => todo.done)
    }
  }
})
```

Getters 也可以接受其他 getters 作为第二个参数：
```
getters: {
  // ...
  doneTodosCount: (state, getters) => {
    return getters.doneTodos.length
  }
}
```

## mapGetters 辅助函数
mapGetters 辅助函数仅仅是将 store 中的 getters 映射到局部计算属性：
```
computed: {
// 使用对象展开运算符将 getters 混入 computed 对象中
  ...mapGetters([
    'doneTodosCount',
    'anotherGetter',
    // ...
  ])
}
```

如果你想将一个 getter 属性另取一个名字，使用对象形式：
```
mapGetters({
  // 映射 this.doneCount 为 store.getters.doneTodosCount
  doneCount: 'doneTodosCount'
})
```

# Mutations
更改 Vuex 的 store 中的状态的唯一方法是提交 mutation。
> Vuex 中的 mutations 非常类似于事件：

每个 mutation 都有一个字符串的 事件类型 (type) 和 一个 回调函数 (handler)。
这个回调函数就是我们实际进行状态更改的地方，并且它会接受 state 作为第一个参数：
```
const store = new Vuex.Store({
  state: {
    count: 1
  },
  mutations: {
    increment (state) {
      // 变更状态
      state.count++
    }
  }
})
```

## 提交 mutation

### Action
```
store.commit('increment')
```

>提交载荷（Payload）

你可以向 store.commit 传入额外的参数，即 mutation 的 载荷（payload）
```
store.commit('increment', 10)

store.commit('increment', {
  amount: 10
})

store.commit({
  type: 'increment',
  amount: 10
})
```

### mapMutations 组件中提交
```
import { mapMutations } from 'vuex'

export default {
  methods: {
    ...mapMutations([
      'increment'
      // 映射 this.increment() 为 this.$store.commit('increment')
    ]),
    ...mapMutations({
      add: 'increment'
      // 映射 this.add() 为 this.$store.commit('increment')
    })
  }
}
```

## 状态操作
```
const mutations = {
  [types.Increment] (state, params) {
    state.searchKey = params
  },
  increment (state, params) {
    state.searchKey = params
  }
}
```

现在想象，我们正在 debug 一个 app 并且观察 devtool 中的 mutation 日志。每一条 mutation 被记录，devtools 都需要捕捉到前一状态和后一状态的快照。然而，在上面的例子中 mutation 中的异步函数中的回调让这不可能完成：因为当 mutation 触发的时候，回调函数还没有被调用，devtools 不知道什么时候回调函数实际上被调用 —— 实质上任何在回调函数中进行的的状态的改变都是不可追踪的。

## 下一步：Actions
在 mutation 中混合异步调用会导致你的程序很难调试。
例如，当你能调用了两个包含异步回调的 mutation 来改变状态，你怎么知道什么时候回调和哪个先回调呢？这就是为什么我们要区分这两个概念。在 Vuex 中，mutation 都是同步事务：
```
store.commit('increment')
// 任何由 "increment" 导致的状态变更都应该在此刻完成。
```


# Actions
Action 类似于 mutation，不同在于：
1. Action 提交的是 mutation，而不是直接变更状态。
1. Action 可以包含任意异步操作。

Action 函数接受一个与 store 实例具有相同方法和属性的 context 对象，因此你可以调用 context.commit 提交一个 mutation，或者通过 context.state 和 context.getters 来获取 state 和 getters。当我们在之后介绍到 Modules 时，你就知道 context 对象为什么不是 store 实例本身了。
```
actions: {
  increment (context) {
    context.commit('increment')
  }
}

actions: {
  increment ({ commit }) {
    commit('increment')
  }
}
```

## dispatch 分发 Action
```
this.$store.dispatch('getTravelsList')
```

## 载荷分发 Action
```
// 以载荷形式分发
store.dispatch('incrementAsync', {
  amount: 10
})

// 以对象形式分发
store.dispatch({
  type: 'incrementAsync',
  amount: 10
})
```

## mapActions 组件分发
```
methods: {
  ...mapActions([
    'increment' // 映射 this.increment() 为 this.$store.dispatch('increment')
  ]),
  ...mapActions({
    add: 'increment' // 映射 this.add() 为 this.$store.dispatch('increment')
  })
}
```
## 组合 Actions
一个 store.dispatch 在不同模块中可以触发多个 action 函数。在这种情况下，只有当所有触发函数完成后，返回的 Promise 才会执行。
```
actions: {
  actionA ({ commit }) {
    return new Promise((resolve, reject) => {
      setTimeout(() => {
        commit('someMutation')
        resolve()
      }, 1000)
    })
  }
}


store.dispatch('actionA').then(() => {
  // ...
})

actions: {
  // ...
  actionB ({ dispatch, commit }) {
    return dispatch('actionA').then(() => {
      commit('someOtherMutation')
    })
  }
}
```

最后，如果我们利用 async / await 这个 JavaScript 即将到来的新特性，我们可以像这样组合 action：
```
actions: {
  async actionA ({ commit }) {
    commit('gotData', await getData())
  },
  async actionB ({ dispatch, commit }) {
    // 等待 actionA 完成
    await dispatch('actionA')
    commit('gotOtherData', await getOtherData())
  }
}
```

# Modules
使用单一状态树，导致应用的所有状态集中到一个很大的对象。但是，当应用变得很大时，store 对象会变得臃肿不堪。
为了解决以上问题，Vuex 允许我们将 store 分割到模块（module）。每个模块拥有自己的 state、mutation、action、getters、甚至是嵌套子模块——从上至下进行类似的分割：
```
const moduleA = {
  state: { ... },
  mutations: { ... },
  actions: { ... },
  getters: { ... }
}

const moduleB = {
  state: { ... },
  mutations: { ... },
  actions: { ... }
}

const store = new Vuex.Store({
  modules: {
    a: moduleA,
    b: moduleB
  }
})

store.state.a // -> moduleA 的状态
store.state.b // -> moduleB 的状态
```

## 模块的局部状态
对于模块内部的 mutation 和 getter，接收的第一个参数是模块的局部状态。
```
const moduleA = {
  state: { count: 0 },
  mutations: {
    increment (state) {
      // state 模块的局部状态
      state.count++
    }
  },
  getters: {
    doubleCount (state) {
      return state.count * 2
    }
  }
}
```

同样，对于模块内部的 action，context.state 是局部状态，根节点的状态是 context.rootState:
```
const moduleA = {
  // ...
  actions: {
    incrementIfOddOnRootSum ({ state, commit, rootState }) {
      if ((state.count + rootState.count) % 2 === 1) {
        commit('increment')
      }
    }
  }
}
```

对于模块内部的 getter，根节点状态会作为第三个参数：
```
const moduleA = {
  // ...
  getters: {
    sumWithRootCount (state, getters, rootState) {
      return state.count + rootState.count
    }
  }
}
```

## 命名空间
模块内部的 action、mutation、和 getter 现在仍然注册在全局命名空间——这样保证了多个模块能够响应同一 mutation 或 action。你可以通过添加前缀或后缀的方式隔离各模块，以避免名称冲突。你也可能希望写出一个可复用的模块，其使用环境不可控。例如，我们想创建一个 todos 模块：
```
// types.js

// 定义 getter、action、和 mutation 的名称为常量，以模块名 `todos` 为前缀
export const DONE_COUNT = 'todos/DONE_COUNT'
export const FETCH_ALL = 'todos/FETCH_ALL'
export const TOGGLE_DONE = 'todos/TOGGLE_DONE'
// modules/todos.js
import * as types from '../types'

// 使用添加了前缀的名称定义 getter、action 和 mutation
const todosModule = {
  state: { todos: [] },
  getters: {
    [types.DONE_COUNT] (state) {
      // ...
    }
  },
  actions: {
    [types.FETCH_ALL] (context, payload) {
      // ...
    }
  },
  mutations: {
    [types.TOGGLE_DONE] (state, payload) {
      // ...
    }
  }
}
```

## 模块动态注册
在 store 创建之后，你可以使用 store.registerModule 方法注册模块
```
store.registerModule('myModule', {
  // ...
})
```

# 模块化

# 问题
1. 问题：loading加载
最近浏览到篇文章，普通loading只能通过控制单一的显示，而且多次条用很容易报错。[vue-vuex-loading](https://github.com/deboyblog/vue-vuex-loading "结合Vuex制作一个完美的Loading组件")


# 案例
[shopping-cart](https://github.com/vuejs/vuex/tree/dev/examples/shopping-cart "")
[Counter](https://github.com/vuejs/vuex/tree/dev/examples/counter "")
[Counter with Hot Reload](https://github.com/vuejs/vuex/tree/dev/examples/counter-hot "")
[TodoMVC](https://github.com/vuejs/vuex/tree/dev/examples/todomvc "")
[Flux Chat](https://github.com/vuejs/vuex/tree/dev/examples/chat "")

[Vuex下Store的模块化拆分实践](https://segmentfault.com/a/1190000007667542 "模块拆分多余零碎")
