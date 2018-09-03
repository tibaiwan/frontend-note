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

