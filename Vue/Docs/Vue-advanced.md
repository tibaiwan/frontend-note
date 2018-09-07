# 深入了解组件

* [组件注册](#组件注册)
* [Prop](#prop)
* [自定义事件](#自定义事件)
* [插槽](#插槽)
* [动态组件 & 异步组件](#动态组件-&-异步组件)
* [处理边界情况](#处理边界情况)

## 组件注册

### 组件名

两张定义方法：
```js
// 使用 kebab-case(短横线分隔命名) 
Vue.component('my-component-name', { /* ... */ })
// 使用 PascalCase(驼峰式命名) 
Vue.component('MyComponentName', { /* ... */ })
```

### 全局和局部注册
```js
// 全局注册
Vue.component('component-a', { /* ... */ })

// 局部注册
var ComponentA = { /* ... */ }
new Vue({
  el: '#app'
  components: {
    'component-a': ComponentA
  }
})
```

### 模块系统

创建一个 components 目录，并将每个组件放置在其各自的文件中。
```js
import ComponentA from './ComponentA'
import ComponentC from './ComponentC'

export default {
  components: {
    ComponentA,
    ComponentC
  },
  // ...
}
```

### 基础组件的自动化全局注册

使用 require.context 全局注册通用的基础组件。
全局注册的行为必须在根 Vue 实例 (通过 new Vue) 创建之前发生。
```js
import Vue from 'vue'
import upperFirst from 'lodash/upperFirst'
import camelCase from 'lodash/camelCase'

const requireComponent = require.context(
  // 其组件目录的相对路径
  './components',
  // 是否查询其子目录
  false,
  // 匹配基础组件文件名的正则表达式
  /Base[A-Z]\w+\.(vue|js)$/
)

requireComponent.keys().forEach(fileName => {
  // 获取组件配置
  const componentConfig = requireComponent(fileName)

  // 获取组件的 PascalCase 命名
  const componentName = upperFirst(
    camelCase(
      // 剥去文件名开头的 `./` 和结尾的扩展名
      fileName.replace(/^\.\/(.*)\.\w+$/, '$1')
    )
  )

  // 全局注册组件
  Vue.component(
    componentName,
    // 如果这个组件选项是通过 `export default` 导出的，
    // 那么就会优先使用 `.default`，
    // 否则回退到使用模块的根。
    componentConfig.default || componentConfig
  )
})
```

## Prop

### Prop 类型

```js
props: {
  title: String,
  likes: Number,
  isPublished: Boolean,
  commentIds: Array,
  author: Object
}
```

### 传递静态或动态 Prop

```html
<!-- 静态赋值：字符串类型独有 -->
<blog-post title="My journey with Vue"></blog-post>
<!-- 动态赋予一个变量的值 -->
<blog-post v-bind:title="post.title"></blog-post>
```

### 传入一个数字、布尔值、数组、对象、一个对象的所有属性

```html
<!-- 数字 -->
<blog-post v-bind:likes="post.likes"></blog-post>
<!-- 布尔值 -->
<blog-post v-bind:is-published="post.isPublished"></blog-post>
<!-- 数组 -->
<blog-post v-bind:comment-ids="post.commentIds"></blog-post>
<!-- 对象 -->
<blog-post v-bind:author="post.author"></blog-post>
<!-- 传入一个对象的所有属性 -->
<blog-post v-bind="post"></blog-post>
<!-- 上面等价于 -->
<blog-post v-bind:id="post.id" v-bind:title="post.title"></blog-post>
```

### 单向数据流

> 父级 prop 的更新会向下流动到子组件中，但是反过来则不行。每次父级组件发生更新时，子组件中所有的 prop 都将会刷新为最新的值。

### Prop 验证

常用验证方式：
```js
Vue.component('my-component', {
  props: {
    // 基础的类型检查 (`null` 匹配任何类型)
    propA: Number,
    // 多个可能的类型
    propB: [String, Number],
    // 必填的字符串
    propC: {
      type: String,
      required: true
    },
    // 带有默认值的数字
    propD: {
      type: Number,
      default: 100
    },
    // 带有默认值的对象
    propE: {
      type: Object,
      // 对象或数组默认值必须从一个工厂函数获取
      default: function () {
        return { message: 'hello' }
      }
    },
    // 自定义验证函数
    propF: {
      validator: function (value) {
        // 这个值必须匹配下列字符串中的一个
        return ['success', 'warning', 'danger'].indexOf(value) !== -1
      }
    }
  }
})
```

类型检查，type选项：
- String
- Number
- Boolean
- Array
- Object
- Date
- Function
- Symbol

### 非 Prop 的特性

> 组件可以接受任意的非 Prop 特性，而这些特性会被添加到这个组件的根元素上。

替换/合并已有的特性：style 和 class 特性会被合并，其他特性会被替换。
禁用特性继承：组件选项inheritAttrs: false。

## 自定义事件

### 事件名

```js
this.$emit('myEvent')
```

> 推荐使用 kebab-case 的事件名。

### 自定义组件的 v-model

> 一个组件上的 v-model 默认会利用名为 value 的 prop 和名为 input 的事件。

## 插槽

### 插槽内容

> Vue 实现了一套内容分发的 API，将 <slot> 元素作为承载分发内容的出口。

### 具名插槽

示例：  
模板`<base-layout>`

`<slot>` 元素有一个特殊的特性：name。这个特性可以用来定义额外的插槽。
```html
<div class="container">
  <header>
    <slot name="header"></slot>
  </header>
  <main>
    <!-- 默认插槽，作为所有未匹配到插槽的内容的统一出口 -->
    <slot></slot>
  </main>
  <footer>
    <slot name="footer"></slot>
  </footer>
</div>
```

使用：在一个父组件的 `<template>` 元素上使用 slot 特性。
```html
<base-layout>
  <template slot="header">
    <h1>Here might be a page title</h1>
  </template>

  <p>A paragraph for the main content.</p>
  <p>And another one.</p>

  <template slot="footer">
    <p>Here's some contact info</p>
  </template>
</base-layout>
```

渲染结果：
```html
<div class="container">
  <header>
    <h1>Here might be a page title</h1>
  </header>
  <main>
    <p>A paragraph for the main content.</p>
    <p>And another one.</p>
  </main>
  <footer>
    <p>Here's some contact info</p>
  </footer>
</div>
```

### 插槽的默认内容

> 可以在 `<slot>` 标签内部指定默认的内容

### 编译作用域

> 父组件模板的所有东西都会在父级作用域内编译；子组件模板的所有东西都会在子级作用域内编译。

### 作用域插槽

示例：
`<todo-list>`
```html
<ul>
  <li
    v-for="todo in todos"
    v-bind:key="todo.id"
  >
    <!-- 我们为每个 todo 准备了一个插槽，-->
    <!-- 将 `todo` 对象作为一个插槽的 prop 传入。-->
    <slot v-bind:todo="todo">
      <!-- 回退的内容 -->
      {{ todo.text }}
    </slot>
  </li>
</ul>
```
使用：
```html
<todo-list v-bind:todos="todos">
  <!-- 将 `slotProps` 定义为插槽作用域的名字 -->
  <template slot-scope="slotProps">
    <!-- 为待办项自定义一个模板，-->
    <!-- 通过 `slotProps` 定制每个待办项。-->
    <span v-if="slotProps.todo.isComplete">✓</span>
    {{ slotProps.todo.text }}
  </template>
</todo-list>
```

### 解构 slot-scope

```html
<todo-list v-bind:todos="todos">
  <template slot-scope="{ todo }">
    <span v-if="todo.isComplete">✓</span>
    {{ todo.text }}
  </template>
</todo-list>
```

## 动态组件 & 异步组件

### 在动态组件上使用 keep-alive

> 在组件之间切换的时候，可以保持这些组件的状态，以避免反复重渲染导致的性能问题。

```html
<!-- 失活的组件将会被缓存！-->
<keep-alive>
  <component v-bind:is="currentTabComponent"></component>
</keep-alive>
```

### 异步组件

> Vue 允许你以一个工厂函数的方式定义你的组件，这个工厂函数会异步解析你的组件定义。Vue 只有在这个组件需要被渲染的时候才会触发该工厂函数，且会把结果缓存起来供未来重渲染。




