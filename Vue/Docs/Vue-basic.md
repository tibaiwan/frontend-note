# Vue 笔记（基础部分）

* [Vue 实例](#vue-实例)
* [模板语法](#模板语法)
* [计算属性和侦听器](#计算属性和侦听器)
* [Class 与 Style 绑定](#class-与-sstyle绑定)
* [条件渲染](#条件渲染)
* [列表渲染](#列表渲染)
* [事件处理](#事件处理)
* [表单输入绑定](#表单输入绑定)
* [组件基础](#组件基础)

## Vue 实例

### 创建一个 Vue 实例
```js
var vm = new Vue({
  // 选项
})
```

### 数据与方法

> 当一个 Vue 实例被创建时，它向 Vue 的响应式系统中加入了其 data 对象中能找到的所有的属性。当这些属性的值发生改变时，视图将会产生“响应”，即匹配更新为新的值。

```js
// 我们的数据对象
var data = { a: 1 }

// 该对象被加入到一个 Vue 实例中
var vm = new Vue({
  data: data
})

// 获得这个实例上的属性
// 返回源数据中对应的字段
vm.a == data.a // => true

// 设置属性也会影响到原始数据
vm.a = 2
data.a // => 2

// ……反之亦然
data.a = 3
vm.a // => 3

```
**Note:** Object.freeze() 会阻止修改现有的属性，意味着响应系统无法再追踪变化。

> 除了数据属性，Vue 实例还暴露了一些有用的实例属性与方法。它们都有前缀 $，以便与用户定义的属性区分开来。

```js
var data = { a: 1 }
var vm = new Vue({
  el: '#example',
  data: data
})

vm.$data === data // => true
vm.$el === document.getElementById('example') // => true

// $watch 是一个实例方法
vm.$watch('a', function (newValue, oldValue) {
  // 这个回调将在 `vm.a` 改变后调用
})
```

### 实例生命周期钩子

生命周期图示（滚动图片）：

<div>
  <img src="../Image/lifecycle.png" width=650 />
</div>

## 模板语法

### 插值

文本：
```html
<span>Message: {{msg}}</span>
```
纯HTML：
```html
<div v-html="rawHtml"></div>
```

特性：
```html
<div v-bind:id="dynamicId"></div>
<!-- 在布尔特性的情况下，它们的存在即暗示为 true -->
<button v-bind:disabled="isButtonDisabled">Button</button>
```
使用 js 表达式：
```html
{{ number + 1 }}
{{ ok ? 'YES' : 'NO' }}
```

### 指令

> 指令（Directives）是带有v-前缀的特殊属性。指令特性的值预期是单个 JavaScript 表达式。

参数：
> 一些指令能够接收一个“参数”，在指令名称之后以冒号表示。
```html
<p v-if="seen">现在你看到我了</p>
<a v-bind:href="url"></a>
<a v-on:click="doSomethind"></a>
```

修饰符：
> 修饰符 (Modifiers) 是以半角句号 . 指明的特殊后缀，用于指出一个指令应该以特殊方式绑定。
```html
<form v-on:submit.prevent="onSubmit"></form>
```

### 缩写

```html
<a v-bind:href="url"></a>
<a v-on:click="doSomething"></a>
<!-- 缩写 -->
<a :href="url"></a>
<a @click="doSomethind"></a>
```

## 计算属性和侦听器

### 计算属性

基础例子
```html
<div id="example">
  <p>Original message: "{{ message }}"</p>
  <!-- 计算属性 -->
  <p>Computed reversed message: "{{ reversedMessage }}"</p>
  <!-- 方法 -->
  <p>Computed reversed message: "{{ reversedMessage2() }}"</p>
</div>
```
```js
var vm = new Vue({
  el: '#example',
  data: {
    message: 'Hello'
  },
  computed: {
    // 计算属性的 getter
    reversedMessage: function () {
      // `this` 指向 vm 实例
      return this.message.split('').reverse().join('')
    }
  },
  methods: {
    reversedMessage2: function () {
      return this.message.split('').reverse().join('')
    }
  }
})
```

计算属性缓存 vs 方法

> 两种方式的最终结果是完全相同的。不同的是计算属性是基于它们的依赖进行缓存的。只在相关依赖发生改变时它们才会重新求值。相比之下，每当触发重新渲染时，调用方法将总会再次执行函数。

计算属性 vs 侦听属性

    尽量使用侦听属性。

### 侦听器

> 当需要在数据变化时执行异步或开销较大的操作时，这个方式是最有用的。

## Class 与 Style 绑定

### 绑定 HTML Class

对象语法：
```html
<div v-bind:class="{ active: isActive }"></div>
<div class="static" v-bind:class="{ active: isActive, 'text-danger': hasError }"></div>
<div v-bind:class="classObject"></div>
```
```js
data: {
  isActive: true,
  hasError: false,
  classObject: {
    active: true,
    'text-danger': false
  }
}
```
**Note:** classObject 也可以是一个返回对象的计算属性。

数组语法：
```html
<div v-bind:class="[activeClass, errorClass]"></div>
<div v-bind:class="[isActive ? activeClass : '', errorClass]"></div>
<div v-bind:class="[{ active: isActive }, errorClass]"></div>
```
```js
data: {
  activeClass: 'active',
  errorClass: 'text-danger',
  isActive: true
}
```

### 绑定内联样式

对象语法：
```html
<div v-bind:style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>
<div v-bind:style="styleObject"></div>
```
```js
data: {
  activeColor: 'red',
  fontSize: 30,
  styleObject: {
    color: 'red',
    fontSize: '13px'
  }
}
```

数组语法：
```html
<div v-bind:style="[baseStyles, overridingStyles]"></div>
```

## 条件渲染

  - v-if
  - v-else-if
  - v-else
  - v-show

实例：
```html
<div v-if="type === 'A'">
  A
</div>
<div v-else-if="type === 'B'">
  B
</div>
<div v-else-if="type === 'C'">
  C
</div>
<div v-else>
  Not A/B/C
</div>
```

**Note:**  
1、 v-show 不支持 `<template>` 元素，也不支持 v-else.  
2、 可以使用 key 管理可复用的元素。  

v-if vs v-show
- v-if 是“真正”的条件渲染，因为它会确保在切换过程中条件块内的事件监听器和子组件适当地被销毁和重建。
- v-if 也是惰性的：如果在初始渲染时条件为假，则什么也不做——直到条件第一次变为真时，才会开始渲染条件块。
- v-show 不管初始条件是什么，元素总是会被渲染，并且只是简单地基于 CSS 进行切换。
- 一般来说，v-if 有更高的切换开销，而 v-show 有更高的初始渲染开销。因此，如果需要非常频繁地切换，则使用 v-show 较好；如果在运行时条件很少改变，则使用 v-if 较好。

## 列表渲染

### v-for 遍历数组和对象

```html
<!-- 遍历数组 -->
<div v-for="item in items" :key="item.id">
  <!-- 内容 -->
</div>
</ul>
<!-- 遍历对象 -->
<div v-for="(value, key, index) in object">
  {{ index }}. {{ key }}: {{ value }}
</div>
```
```js
var example1 = new Vue({
  el: '#example-1',
  data: {
    items: [
      { message: 'Foo' },
      { message: 'Bar' }
    ],
    object: {
      firstName: 'John',
      lastName: 'Doe',
      age: 30
    }
  }
})
```
**Note:** 建议尽可能在使用 v-for 时提供 key。

### 数组更新检测：
变异方法：
    
    改变原数组：
    push(), pop(), shift(), unshift(),splice(),sort(),reverse()

替换数组：

    返回新的数组：
    filter(), concat(), slice()

注意事项：

由于 JavaScript 的限制，Vue 不能检测以下变动的数组：

- 当你利用索引直接设置一个项时，例如：vm.items[indexOfItem] = newValue
- 当你修改数组的长度时，例如：vm.items.length = newLength

### 对象更改检测注意事项

    由于 JavaScript 的限制，Vue 不能检测对象属性的添加或删除
    可以使用 Vue.set(object, key, value) 方法向嵌套对象添加响应式属性
```js
var vm = new Vue({
  data: {
    userProfile: {
      name: 'Anika'
    }
  }
})
// 单个属性
Vue.set(vm.userProfile, 'age', 27)
// 多个属性
vm.userProfile = Object.assign({}, vm.userProfile, {
  age: 27,
  favoriteColor: 'Vue Green'
})
```

v-for with v-if

> 当它们处于同一节点，v-for 的优先级比 v-if 更高，这意味着 v-if 将分别重复运行于每个 v-for 循环中。

## 事件处理

### 监听事件

> 可以用 v-on 指令监听 DOM 事件
```html
<button v-on:click="counter += 1">Add 1</button>
<button v-on:click="greet">Greet</button>
<!-- 内联处理器中的方法 -->
<button v-on:click="say('hi', $event)">Say hi</button>
```
```js
var vm = new Vue({
  el: '#example',
  data: {
    counter: 0
  },
  // 在 `methods` 对象中定义方法
  methods: {
    greet: function (event) {
      // `this` 在方法里指向当前 Vue 实例
      alert('Hello ' + this.name + '!')
      // `event` 是原生 DOM 事件
      if (event) {
        alert(event.target.tagName)
      },
    },
    say: function (message, event) {
      if (event) event.preventDefault()
      alert(message)
    }
  }
})
```
**Note:** 原始的 DOM 事件，可以用特殊变量 $event 把它传入方法。

### 事件修饰符

- .stop
- .prevent
- .capture
- .self
- .once
- .passive

```html
<!-- 阻止单击事件继续传播 -->
<a v-on:click.stop="doThis"></a>

<!-- 提交事件不再重载页面 -->
<form v-on:submit.prevent="onSubmit"></form>

<!-- 修饰符可以串联 -->
<a v-on:click.stop.prevent="doThat"></a>

<!-- 只有修饰符 -->
<form v-on:submit.prevent></form>

<!-- 添加事件监听器时使用事件捕获模式 -->
<!-- 即元素自身触发的事件先在此处理，然后才交由内部元素进行处理 -->
<div v-on:click.capture="doThis">...</div>

<!-- 只当在 event.target 是当前元素自身时触发处理函数 -->
<!-- 即事件不是从内部元素触发的 -->
<div v-on:click.self="doThat">...</div>

<!-- 点击事件将只会触发一次 -->
<a v-on:click.once="doThis"></a>

<!-- 滚动事件的默认行为 (即滚动行为) 将会立即触发 -->
<!-- 而不会等待 `onScroll` 完成  -->
<!-- 这其中包含 `event.preventDefault()` 的情况 -->
<!-- 这个 .passive 修饰符尤其能够提升移动端的性能。 -->
<div v-on:scroll.passive="onScroll">...</div>
```

### 按键修饰符

```html
<!-- 只有在 `keyCode` 是 13 时调用 `vm.submit()` -->
<input v-on:keyup.13="submit">

<!-- 同上 -->
<input v-on:keyup.enter="submit">
```

全部的按键别名：

- .enter
- .tab
- .delete (捕获“删除”和“退格”键)
- .esc
- .space
- .up
- .down
- .left
- .right

系统修饰键

- .ctrl
- .alt
- .shift
- .meta

.exact 修饰符

> .exact 修饰符允许你控制由精确的系统修饰符组合触发的事件。
```html
<!-- 即使 Alt 或 Shift 被一同按下时也会触发 -->
<button @click.ctrl="onClick">A</button>

<!-- 有且只有 Ctrl 被按下的时候才触发 -->
<button @click.ctrl.exact="onCtrlClick">A</button>

<!-- 没有任何系统修饰符被按下的时候才触发 -->
<button @click.exact="onClick">A</button>
```

鼠标按钮修饰符

- .left
- .right
- .middle

## 表单输入绑定

### 基础用法
> 你可以用 v-model 指令在表单 `<input>`、`<textarea>` 及 `<select>` 元素上创建双向数据绑定。它会根据控件类型自动选取正确的方法来更新元素。

```html
<p>Message is: {{ message }}</p>
<!-- 文本 -->
<input v-model="message" placeholder="edit me">

<!-- 多行文本 -->
<textarea v-model="message" placeholder="add multiple lines"></textarea>

<!-- 单个复选框（多个复选框类型是数组） -->
<input type="checkbox" id="checkbox" v-model="checked">

<!-- 单选按钮 -->
<input type="radio" id="one" value="One" v-model="picked">
<label for="one">One</label>
<br>
<input type="radio" id="two" value="Two" v-model="picked">
<label for="two">Two</label>

<!-- 选择框 -->
<select v-model="selected">
  <option disabled value="">请选择</option>
  <option>A</option>
  <option>B</option>
</select>
```

### 值绑定

```html
<!-- 当选中时，`picked` 为字符串 "a" -->
<input type="radio" v-model="picked" value="a">

<!-- 复选框，当选中时：vm.toggle === 'yes'，当没有选中时：vm.toggle === 'no' -->
<input type="checkbox" v-model="toggle" true-value="yes" false-value="no">

<!-- 单选按钮,当选中时：vm.pick === vm.a -->
<input type="radio" v-model="pick" v-bind:value="a">

<!-- 选择框的选项，当选中时：vm.selected.number => 123 -->
<select v-model="selected">
  <option v-bind:value="{ number: 123 }">123</option>
</select>
```

### 修饰符

> .lazy

    在默认情况下，v-model 在每次 input 事件触发后将输入框的值与数据进行同步 (除了上述输入法组合文字时)。你可以添加 lazy 修饰符，从而转变为使用 change 事件进行同步

```html
<!-- 在“change”时而非“input”时更新 -->
<input v-model.lazy="msg" >
```

> .number

    如果想自动将用户的输入值转为数值类型，可以给 v-model 添加 number 修饰符
    这通常很有用，因为即使在 type="number" 时，HTML 输入元素的值也总会返回字符串。

```html
<input v-model.number="age" type="number">
```

> .trim

    如果要自动过滤用户输入的首尾空白字符，可以给 v-model 添加 trim 修饰符

```html
<input v-model.trim="msg">
```

## 组件基础

### 基本示例

```js
// 定义一个名为 button-counter 的新组件
Vue.component('button-counter', {
  data: function () {
    return {
      count: 0
    }
  },
  template: '<button v-on:click="count++">You clicked me {{ count }} times.</button>'
})
```

### 组件的复用

```html
<div id="components-demo">
  <button-counter></button-counter>
  <button-counter></button-counter>
  <button-counter></button-counter>
</div>
```

**Notice:**

一个组件的 data 选项必须是一个函数，因此每个实例可以维护一份被返回对象的独立的拷贝。  
当点击按钮时，每个组件都会各自独立维护它的 count。  

### 组件的组织

> 通常一个应用会以一棵嵌套的组件树的形式来组织：

### 通过 Prop 向子组件传递数据

1、可以把数据作为一个自定义特性传递  
2、可以使用 v-bind 来动态传递 prop

```html
<blog-post title="My journey with Vue"></blog-post>
<blog-post :title="post.title"></blog-post>
```

### 单个根元素

> 每个组件必须只有一个根元素。

### 通过事件向父级组件发送消息

使用事件抛出一个值

    $emit('method', data)

在组件上使用 v-model

```html
<input v-model="searchText">
<!-- 等价于 -->
<input
  v-bind:value="searchText"
  v-on:input="searchText = $event.target.value"
>
<!-- 当用在组件上时 -->
<custom-input
  v-bind:value="searchText"
  v-on:input="searchText = $event"
></custom-input>
<!-- 定义组件后使用方式 -->
<custom-input v-model="searchText"></custom-input>
```
```js
Vue.component('custom-input', {
  props: ['value'],
  template: `
    <input
      v-bind:value="value"
      v-on:input="$emit('input', $event.target.value)"
    >
  `
})
```

### 通过插槽分发内容

```html
<alert-box>
  Something bad happened.
</alert-box>
```
```js
Vue.component('alert-box', {
  template: `
    <div class="demo-alert-box">
      <strong>Error!</strong>
      <slot></slot>
    </div>
  `
})
```
渲染后：
```html
<div class="demo-alert-box">
  <strong>Error!</strong> 
  Something bad happened.
</div>
```

### 动态组件

```html
<!-- 组件会在 `currentTabComponent` 改变时改变 -->
<component v-bind:is="currentTabComponent"></component
```

### 解析 DOM 模板时的注意事项

> 有些 HTML 元素，诸如 `<ul>`、`<ol>`、`<table>` 和 `<select>`，对于哪些元素可以出现在其内部是有严格限制的。而有些元素，诸如 `<li>`、`<tr>` 和 `<option>`，只能出现在其它某些特定的元素内部。
