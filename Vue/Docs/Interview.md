<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

# Vue 常见面试问题

- [计算属性（computed）、方法（methods）和侦听属性（watch）的区别与使用场景](#%E8%AE%A1%E7%AE%97%E5%B1%9E%E6%80%A7computed%E6%96%B9%E6%B3%95methods%E5%92%8C%E4%BE%A6%E5%90%AC%E5%B1%9E%E6%80%A7watch%E7%9A%84%E5%8C%BA%E5%88%AB%E4%B8%8E%E4%BD%BF%E7%94%A8%E5%9C%BA%E6%99%AF)
- [Vue 生命周期的理解](#vue-%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F%E7%9A%84%E7%90%86%E8%A7%A3)
- [Vue 双向绑定，为什么不能通过修改下标来通知视图发生变化。](#vue-%E5%8F%8C%E5%90%91%E7%BB%91%E5%AE%9A%E4%B8%BA%E4%BB%80%E4%B9%88%E4%B8%8D%E8%83%BD%E9%80%9A%E8%BF%87%E4%BF%AE%E6%94%B9%E4%B8%8B%E6%A0%87%E6%9D%A5%E9%80%9A%E7%9F%A5%E8%A7%86%E5%9B%BE%E5%8F%91%E7%94%9F%E5%8F%98%E5%8C%96)
- [简述 Vue 中的 MVVM 模型](#%E7%AE%80%E8%BF%B0-vue-%E4%B8%AD%E7%9A%84-mvvm-%E6%A8%A1%E5%9E%8B)
- [Vue 路由中 hash 模式和 history 模式区别](#vue-%E8%B7%AF%E7%94%B1%E4%B8%AD-hash-%E6%A8%A1%E5%BC%8F%E5%92%8C-history-%E6%A8%A1%E5%BC%8F%E5%8C%BA%E5%88%AB)
- [Vue 路由中 $route 和 $router 的区别](#vue-%E8%B7%AF%E7%94%B1%E4%B8%AD-route-%E5%92%8C-router-%E7%9A%84%E5%8C%BA%E5%88%AB)
- [v-for 响应式 key, index 及 item.id 参数的异同](#v-for-%E5%93%8D%E5%BA%94%E5%BC%8F-key-index-%E5%8F%8A-itemid-%E5%8F%82%E6%95%B0%E7%9A%84%E5%BC%82%E5%90%8C)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## 计算属性（computed）、方法（methods）和侦听属性（watch）的区别与使用场景

> methods VS 计算属性

我们可以将同一函数定义为一个 method 而不是一个计算属性。对于最终的结果，两种方式确实是相同的。然而，不同的是**计算属性是基于它们的依赖进行缓存的**。计算属性只有在它的相关依赖发生改变时才会重新求值。这就意味着只要 message 还没有发生改变，多次访问 reversedMessage 计算属性会立即返回之前的计算结果，而不必再次执行函数。相比而言，只要发生重新渲染，method 调用总会执行该函数。总之，重新计算开销很大的话请选计算属性，不希望有缓存的请选methods。

> watch VS 计算属性

当你在模板内使用了复杂逻辑的表达式时，你应当使用计算属性。

侦听属性是一个对象，键是需要观察的表达式，值是对应回调函数。值也可以是方法名，或者包含选项的对象。  
当你有一些数据需要随着其它数据变动而变动时，或者当需要**在数据变化时执行异步或开销较大的操作时**，你可以使用 watch。

## Vue 生命周期的理解

[如何解释vue的生命周期才能令面试官满意？](https://segmentfault.com/a/1190000014376915)

- beforeCreate

在实例初始化之后，数据观测 (data observer) 和 event/watcher 事件配置之前被调用。

- created

在实例创建完成后被立即调用。在这一步，实例已完成以下的配置：数据观测 (data observer)，属性和方法的运算，watch/event 事件回调。然而，挂载阶段还没开始，$el 属性目前不可见。

- beforeMount

在挂载开始之前被调用：相关的 render 函数首次被调用。  
该钩子在服务器端渲染期间不被调用。以下周期在服务端渲染期间都不被调用。

- mounted

el 被新创建的 vm.$el 替换，并挂载到实例上去之后调用该钩子。如果 root 实例挂载了一个文档内元素，当 mounted 被调用时 vm.$el 也在文档内。注意 mounted 不会承诺所有的子组件也都一起被挂载。如果你希望等到整个视图都渲染完毕，可以用 vm.$nextTick 替换掉 mounted。  

- beforeUpdate

数据更新时调用，发生在虚拟 DOM 打补丁之前。这里适合在更新之前访问现有的 DOM，比如手动移除已添加的事件监听器。  

- updated

由于数据更改导致的虚拟 DOM 重新渲染和打补丁，在这之后会调用该钩子。

- activated

keep-alive 组件激活时调用。

- deactivated

keep-alive 组件停用时调用。

- beforeDestroy

实例销毁之前调用。在这一步，实例仍然完全可用。

- destroyed

Vue 实例销毁后调用。调用后，Vue 实例指示的所有东西都会解绑定，所有的事件监听器会被移除，所有的子实例也会被销毁。

## Vue 双向绑定，为什么不能通过修改下标来通知视图发生变化。

[Vue为什么不能检测数组变动](https://segmentfault.com/a/1190000015783546)

> 变异方法：

Vue 包含一组观察数组的变异方法，所以它们也将会触发视图更新。这些方法如下：
push(), pop(), shift() ,unshift(), splice(), sort(), reverse()

> 替换数组：

filter(), concat() 和 slice() 。这些不会改变原始数组，但总是返回一个新数组。当使用非变异方法时，可以用新数组替换旧数组：

> 注意事项:

由于 JavaScript 的限制，Vue 不能检测以下变动的数组：
- 当你利用索引直接设置一个项时，例如：vm.items[indexOfItem] = newValue
- 当你修改数组的长度时，例如：vm.items.length = newLength

变通方法：
- Vue.set(vm.items, indexOfItem, newValue)
- vm.items.splice(indexOfItem, 1, newValue)

## 简述 Vue 中的 MVVM 模型

Vue是以数据为驱动的，Vue自身将DOM和数据进行绑定，一旦创建绑定，DOM和数据将保持同步，每当数据发生变化，DOM会跟着变化。

ViewModel是Vue的核心，它是Vue的一个实例。Vue实例是作用在某个HTML元素上的，这个HTML元素可以是body，也可以是某个id所指代的元素。 DOM Listeners和Data Bindings是实现双向绑定的关键。DOM Listeners监听页面所有View层DOM元素的变化，当发生变化，Model层的数据随之变化；Data Bindings监听Model层的数据，当数据发生变化，View层的DOM元素随之变化。

## Vue 路由中 hash 模式和 history 模式区别

> hash模式

hash 出现在 URL 中，但不会被包含在 http 请求中，对后端完全没有影响，因此改变 hash 不会重新加载页面。

特点：hash虽然在URL中，但不被包括在HTTP请求中；用来指导浏览器动作，hash不会重加载页面。

> history模式

history 利用了 html5 history interface 中新增的 pushState() 和 replaceState() 方法。这两个方法应用于浏览器记录栈，在当前已有的 back、forward、go 基础之上，它们提供了对历史记录修改的功能。只是当它们执行修改时，虽然改变了当前的 URL ，但浏览器不会立即向后端发送请求。

> 原理：

hash 模式的原理是 onhashchange 事件，可以在 window 对象上监听这个事件。   
history ：hashchange 只能改变 # 后面的代码片段，history api （pushState、replaceState、go、back、forward） 则给了前端完全的自由，通过在window对象上监听popState()事件。

## Vue 路由中 $route 和 $router 的区别

$route 是“路由信息对象”，包括 path，params，hash，query，fullPath，matched，name 等路由信息参数。  
$router 是“路由实例”对象包括了路由的跳转方法，钩子函数等。

## v-for 响应式 key, index 及 item.id 参数的异同

```html
<ul>
  <li v-for="(item, key, index) in items" :key="item.id">{{item.id}}</li>
</ul>
```
使用 v-for 渲染元素时，使用元素自身的 id 属性去指定渲染元素的 key 值有利于单个元素的重新渲染，若采用 index 和 key，在改变渲染出来的 DOM 结构时，会触发所有元素的重新渲染，当数据过大时，可能会造成性能负担。

[See Also](https://github.com/TimRChen/Vue-v-for-explor#vue-20-v-for-%E5%93%8D%E5%BA%94%E5%BC%8Fkey-index%E5%8F%8Aitemid%E5%8F%82%E6%95%B0%E5%AF%B9v-bindkey%E5%80%BC%E9%80%A0%E6%88%90%E5%B7%AE%E5%BC%82%E7%A0%94%E7%A9%B6)
