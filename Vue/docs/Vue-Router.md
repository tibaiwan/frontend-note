# Vuex 笔记

* [state](#state)
* [Getter](#getter)
* [Mutation](#mutation)
* [Action](#action)
* [Module](#module)

> 每一个 Vuex 应用的核心就是 store（仓库）。“store”基本上就是一个容器，它包含着你的应用中大部分的状态 (state)。

每一个Vuex应用的核心就是store。它和单纯的全局变量有两点不同：

    1、Vuex 的状态存储是响应式的。当 Vue 组件从 store 中读取状态的时候，若 store 中的状态发生变化，那么相应的组件也会相应地得到高效更新。
    2、你不能直接改变 store 中的状态。改变 store 中的状态的唯一途径就是显式地提交 (commit) mutation。

实例：
```js
// 如果在模块化构建系统中，请确保在开头调用了 Vue.use(Vuex)

const store = new Vuex.Store({
  state: {
    count: 0
  },
  mutations: {
    increment (state) {
      state.count++
    }
  }
})
```

## state

### 单一状态树

> Vuex 使用单一状态树 —— 用一个对象就包含了全部的应用层级状态。

### 在 Vue 组件中获得 Vuex 状态

1、在计算属性中返回某个状态（需要使用 state 的组件中需要导入）。
```js
// 创建一个 Counter 组件
const Counter = {
  template: `<div>{{ count }}</div>`,
  computed: {
    count () {
      return store.state.count
    }
  }
}
```

2、将状态从根组件“注入”到每一个子组件中，子组件能通过 this.$store 访问到。
```js
const app = new Vue({
  el: '#app',
  // 把 store 对象提供给 “store” 选项，这可以把 store 的实例注入所有的子组件
  store,
  components: { Counter },
  template: `
    <div class="app">
      <counter></counter>
    </div>
  `
})
```
```js
const Counter = {
  template: `<div>{{ count }}</div>`,
  computed: {
    count () {
      return this.$store.state.count
    }
  }
}
```

3、使用 mapState 辅助函数

```js
// 在单独构建的版本中辅助函数为 Vuex.mapState
import { mapState } from 'vuex'

export default {
  // ...
  computed: mapState({
    // 箭头函数可使代码更简练
    count: state => state.count,

    // 传字符串参数 'count' 等同于 `state => state.count`
    countAlias: 'count',

    // 为了能够使用 `this` 获取局部状态，必须使用常规函数
    countPlusLocalState (state) {
      return state.count + this.localCount
    }
  })
}
```

> 当映射的计算属性的名称与 state 的子节点名称相同时，我们也可以给 mapState 传一个字符串数组
```js
computed: mapState([
  // 映射 this.count 为 store.state.count
  'count'
])
```

### 对象展开运算符

  mapState 函数返回的是一个对象。使用对象展开运算符使 mapState 与局部计算属性混合使用。
```js
computed: {
  localComputed () { /* ... */ },
  // 使用对象展开运算符将此对象混入到外部对象中
  ...mapState({
    // ...
  })
}
```

## Getter

有时候我们需要从 store 中的 state 中派生出一些状态，如果有多个组件需要用到此属性，可以使用 Getter。

> Vuex 允许我们在 store 中定义“getter”（可以认为是 store 的计算属性）。就像计算属性一样，getter 的返回值会根据它的依赖被缓存起来，且只有当它的依赖值发生了改变才会被重新计算。

- Getter 接受 state 作为其第一个参数  
- Getter 也可以接受其他 getter 作为第二个参数
```js
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
    },
    doneTodosCount: (state, getters) => {
      return getters.doneTodos.length
    }
  }
})
```

### 通过属性访问

```js
// 方式一
store.getters.doneTodos // -> [{ id: 1, text: '...', done: true }]
// 方式二
computed: {
  doneTodosCount () {
    return this.$store.getters.doneTodosCount
  }
}
```

### 通过方法访问

```js
getters: {
  // ...
  getTodoById: (state) => (id) => {
    return state.todos.find(todo => todo.id === id)
  }
}
```
```js
store.getters.getTodoById(2) // -> { id: 2, text: '...', done: false }
```
**Notice:** getter 在通过方法访问时，每次都会去进行调用，而不会缓存结果。

### mapGetters 辅助函数

> mapGetters 辅助函数仅仅是将 store 中的 getter 映射到局部计算属性

```js
import { mapGetters } from 'vuex'

export default {
  // ...
  computed: {
  // 使用对象展开运算符将 getter 混入 computed 对象中
    ...mapGetters([
      doneCount: 'doneTodosCount'
      'anotherGetter',
      // ...
    ])
  }
}
```

## Mutation

    更改 Vuex 的 store 中的状态的唯一方法是提交 mutation。Vuex 中的 mutation 非常类似于事件：每个 mutation 都有一个字符串的事件类型 (type) 和 一个回调函数 (handler)。Mutation 必须是同步函数。

定义：
```js
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

调用：
```js
store.commit('increment')
```

### 提交载荷（Payload）

```js
// ...
mutations: {
  increment (state, payload) {
    state.count += payload.amount
  }
}
```
提交：
```js
store.commit('increment', {
  amount: 10
})
```

对象风格的提交方式：
```js
store.commit({
  type: 'increment',
  amount: 10
})
```

### Mutation 需遵守 Vue 的响应规则

1、最好提前在你的 store 中初始化好所有所需属性。  
2、当需要在对象上添加新属性时，你应该
  - 使用 Vue.set(obj, 'newProp', 123), 或者
  - 以新对象替换老对象。（state.obj = { ...state.obj, newProp: 123 }）

### 使用常量替代 Mutation 事件类型

```js
// mutation-types.js
export const SOME_MUTATION = 'SOME_MUTATION'
```
```js
// store.js
import Vuex from 'vuex'
import { SOME_MUTATION } from './mutation-types'

const store = new Vuex.Store({
  state: { ... },
  mutations: {
    // 我们可以使用 ES2015 风格的计算属性命名功能来使用一个常量作为函数名
    [SOME_MUTATION] (state) {
      // mutate state
    }
  }
})
```

### 在组件中提交 Mutation

    你可以在组件中使用 this.$store.commit('xxx') 提交 mutation，或者使用 mapMutations 辅助函数将组件中的 methods 映射为 store.commit 调用（需要在根节点注入 store）。

```js
import { mapMutations } from 'vuex'

export default {
  // ...
  methods: {
    ...mapMutations([
      'increment', // 将 `this.increment()` 映射为 `this.$store.commit('increment')`

      // `mapMutations` 也支持载荷：
      'incrementBy' // 将 `this.incrementBy(amount)` 映射为 `this.$store.commit('incrementBy', amount)`
    ]),
    ...mapMutations({
      add: 'increment' // 将 `this.add()` 映射为 `this.$store.commit('increment')`
    })
  }
}
```

## Action

Action 类似于 mutation，不同在于：
* Action 提交的是 mutation，而不是直接变更状态。
* Action 可以包含任意异步操作。

定义：
```js
const store = new Vuex.Store({
  state: {
    count: 0
  },
  mutations: {
    increment (state) {
      state.count++
    }
  },
  actions: {
    increment1 (context) {
      context.commit('increment')
    },
    increment2 ({ commit }) {
      commit('increment')
    }
  }
})
```
Action 函数接受一个与 store 实例具有相同方法和属性的 context 对象

### 分发 Action

```js
store.dispatch('increment')
```

### 在组件中分发 Action

    在组件中使用 this.$store.dispatch('xxx') 分发 action，或者使用 mapActions 辅助函数将组件的 methods 映射为 store.dispatch 调用（需要先在根节点注入 store）

```js
import { mapActions } from 'vuex'

export default {
  // ...
  methods: {
    ...mapActions([
      'increment', // 将 `this.increment()` 映射为 `this.$store.dispatch('increment')`

      // `mapActions` 也支持载荷：
      'incrementBy' // 将 `this.incrementBy(amount)` 映射为 `this.$store.dispatch('incrementBy', amount)`
    ]),
    ...mapActions({
      add: 'increment' // 将 `this.add()` 映射为 `this.$store.dispatch('increment')`
    })
  }
}
```

### 组合 Action

定义：
```js
actions: {
  actionA ({ commit }) {
    return new Promise((resolve, reject) => {
      setTimeout(() => {
        commit('someMutation')
        resolve()
      }, 1000)
    })
  },
  actionB ({ dispatch, commit }) {
    return dispatch('actionA').then(() => {
      commit('someOtherMutation')
    })
  }
}
```
调用：
```js
store.dispatch('actionA').then(() => {
  // ...
})
```
