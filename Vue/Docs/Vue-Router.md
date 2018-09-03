# Vue-Router 笔记

基础

* [起步](#起步)
* [动态路由匹配](#动态路由匹配)
* [嵌套路由](#嵌套路由)
* [编程式的导航](#编程式的导航)
* [命名路由](#命名路由)
* [命名视图](#命名视图)
* [重定向和别名](#重定向和别名)
* [路由组件传参](#路由组件传参)
* [HTML5 History 模式](#html5-history-模式)

进阶

* [导航守卫](#导航守卫)
* [路由元信息](#路由元信息)
* [过渡动效](#过渡动效)
* [数据获取](#数据获取)
* [滚动行为](#滚动行为)
* [路由懒加载](#路由懒加载)

## 起步

```js
const router = new VueRouter({
  routes // (缩写) 相当于 routes: routes
})

// 记得要通过 router 配置参数注入路由，
// 从而让整个应用都有路由功能
const app = new Vue({
  router
}).$mount('#app')
```

Note：

    1、使用 router-link 组件来导航，使用 router-view 渲染匹配到的组件
    2、通过注入路由器，我们可以在任何组件内通过 this.$router 访问路由器，也可以通过 this.$route 访问当前路由：
    this.$route.params.username
    this.$router.push('/')
    this.$router.go(-1)

## 动态路由匹配

| 模式 | 匹配路径 | $route.params |
| :- | :- | :- |
| /user/:username | /user/evan | { username: 'evan' } |
| /user/:username/post/:post_id | /user/evan/post/123 | { username: 'evan', post_id: 123 } |

### 响应路由参数的变化

使用 watch 监测变化
```js
const User = {
  template: '...',
  watch: {
    '$route' (to, from) {
      // 对路由变化作出响应...
    }
  }
}
```

使用导航守卫 beforeRouteUpdate
```
const User = {
  template: '...',
  beforeRouteUpdate (to, from, next) {
    // react to route changes...
    // don't forget to call next()
  }
}
```

### 高级匹配模式

    vue-router 使用 [path-to-regexp](https://github.com/pillarjs/path-to-regexp) 作为路径匹配引擎，所以支持很多高级的匹配模式

### 匹配优先级

    匹配的优先级就按照路由的定义顺序

## 嵌套路由

示例：
```js
/user/foo/profile                     /user/foo/posts
+------------------+                  +-----------------+
| User             |                  | User            |
| +--------------+ |                  | +-------------+ |
| | Profile      | |  +------------>  | | Posts       | |
| |              | |                  | |             | |
| +--------------+ |                  | +-------------+ |
+------------------+                  +-----------------+
```

**Notice:** 以 / 开头的嵌套路径会被当作根路径。 这让你充分的使用嵌套组件而无须设置嵌套的路径。

## 编程式的导航

语法：
```js
router.push(location, onComplete?, onAbort?)
```

| 声明式 | 编程式 |
| :- | :- |
| `<router-link :to="...">` | `router.push(...)` |

编程式导航方法：
```js
// 字符串
router.push('home')

// 对象
router.push({ path: 'home' })

// 命名的路由
router.push({ name: 'user', params: { userId: 123 }})
router.push({ path: `/user/${userId}` })

// 带查询参数，变成 /register?plan=private
router.push({ path: 'register', query: { plan: 'private' }})
```
**注意：** 如果提供了 path，params 会被忽略，需要提供完整的带有参数的 path。

其他API：
```js
// 替换当前路由
router.replace(location, onComplete?, onAbort?)
// 在 history 记录中向前或者后退多少步
router.go(n)
```

## 命名路由

> 有时候，通过一个名称来标识一个路由显得更方便一些，特别是在链接一个路由，或者是执行一些跳转的时候。

示例：
```js
const router = new VueRouter({
  routes: [
    {
      path: '/user/:userId',
      name: 'user',
      component: User
    }
  ]
})
```
使用：
```html
<router-link :to="{ name: 'user', params: { userId: 123 }}">User</router-link>
```
```js
router.push({ name: 'user', params: { userId: 123 }})
```

## 命名视图

同时 (同级) 展示多个视图：
```html
<router-view class="view one"></router-view>
<router-view class="view two" name="a"></router-view>
<router-view class="view three" name="b"></router-view>
```
```js
const router = new VueRouter({
  routes: [
    {
      path: '/',
      components: {
        default: Foo,
        a: Bar,
        b: Baz
      }
    }
  ]
})
```
**Notice：** 一个视图使用一个组件渲染，因此对于同个路由，多个视图就需要多个组件。确保正确使用 components 配置 (带上 s)。

## 重定向和别名

### 重定向
```js
const router = new VueRouter({
  routes: [
    { path: '/a', redirect: '/b' },
    { path: '/a', redirect: { name: 'foo' }},
    { path: '/a', redirect: to => {
      // 方法接收 目标路由 作为参数
      // return 重定向的 字符串路径/路径对象
    }}
  ]
})
```

### 别名

```js
const router = new VueRouter({
  routes: [
    { path: '/a', component: A, alias: '/b' }
  ]
})
```

    “重定向”是，当用户访问 /a时，URL 将会被替换成 /b，然后匹配路由为 /b
    “别名”是，/a 的别名是 /b，意味着，当用户访问 /b 时，URL 会保持为 /b，但是路由匹配则为 /a

## 路由组件传参

组件中使用 $route 会使之与其对应路由形成高度耦合，使用 props 将组件和路由解耦。

### 布尔模式

```js
const User = {
  props: ['id'],
  template: '<div>User {{ id }}</div>'
}
const router = new VueRouter({
  routes: [
    { path: '/user/:id', component: User, props: true },

    // 对于包含命名视图的路由，你必须分别为每个命名视图添加 `props` 选项：
    {
      path: '/user/:id',
      components: { default: User, sidebar: Sidebar },
      props: { default: true, sidebar: false }
    }
  ]
})
```

### 对象模式

```js
const router = new VueRouter({
  routes: [
    { path: '/promotion/from-newsletter', component: Promotion, props: { newsletterPopup: false } }
  ]
})
```

### 函数模式

```js
const router = new VueRouter({
  routes: [
    { path: '/search', component: SearchUser, props: (route) => ({ query: route.query.q }) }
  ]
})
```

## HTML5 History 模式

    vue-router 默认 hash 模式。
    使用 history 模式时，需要后台配置支持。（如果后台没有正确的配置，当用户在浏览器直接访问 http://oursite.com/user/id 就会返回 404）

## 导航守卫

> “导航”表示路由正在发生改变。

### 全局守卫

```js
const router = new VueRouter({ ... })

router.beforeEach((to, from, next) => {
  // ...
})
```

每个守卫方法接收三个参数：
- to: Route: 即将要进入的目标 路由对象
- from: Route: 当前导航正要离开的路由
- next: Function: 一定要调用该方法来 resolve 这个钩子。执行效果依赖 next 方法的调用参数。
  - next(): 进行管道中的下一个钩子。
  - next(false): 中断当前的导航。
  - next('/') 或者 next({ path: '/' }): 跳转到一个不同的地址。
  - next(error)：导航会被终止且该错误会被传递给 router.onError() 注册过的回调。

### 全局解析守卫

    可以用 router.beforeResolve 注册一个全局守卫。
    这和 router.beforeEach 类似，区别是在导航被确认之前，同时在所有组件内守卫和异步路由组件被解析之后，解析守卫就被调用。

### 全局后置钩子

```js
router.afterEach((to, from) => {
  // ...
})
```

### 路由独享的守卫

```js
const router = new VueRouter({
  routes: [
    {
      path: '/foo',
      component: Foo,
      beforeEnter: (to, from, next) => {
        // ...
      }
    }
  ]
})
```

### 组件内的守卫

```js
const Foo = {
  template: `...`,
  beforeRouteEnter (to, from, next) {
    // 在渲染该组件的对应路由被 confirm 前调用
    // 不！能！获取组件实例 `this`
    // 因为当守卫执行前，组件实例还没被创建
  },
  beforeRouteUpdate (to, from, next) {
    // 在当前路由改变，但是该组件被复用时调用
    // 举例来说，对于一个带有动态参数的路径 /foo/:id，在 /foo/1 和 /foo/2 之间跳转的时候，
    // 由于会渲染同样的 Foo 组件，因此组件实例会被复用。而这个钩子就会在这个情况下被调用。
    // 可以访问组件实例 `this`
  },
  beforeRouteLeave (to, from, next) {
    // 导航离开该组件的对应路由时调用
    // 可以访问组件实例 `this`
  }
}
```

### 完整的导航解析流程

    导航被触发。
    在失活的组件里调用离开守卫。
    调用全局的 beforeEach 守卫。
    在重用的组件里调用 beforeRouteUpdate 守卫 (2.2+)。
    在路由配置里调用 beforeEnter。
    解析异步路由组件。
    在被激活的组件里调用 beforeRouteEnter。
    调用全局的 beforeResolve 守卫 (2.5+)。
    导航被确认。
    调用全局的 afterEach 钩子。
    触发 DOM 更新。
    用创建好的实例调用 beforeRouteEnter 守卫中传给 next 的回调函数。

## 路由元信息

定义路由的时候可以配置 meta 字段：
```js
const router = new VueRouter({
  routes: [
    {
      path: '/foo',
      component: Foo,
      children: [
        {
          path: 'bar',
          component: Bar,
          // a meta field
          meta: { requiresAuth: true }
        }
      ]
    }
  ]
})
```

> 一个路由匹配到的所有路由记录会暴露为 $route 对象 (还有在导航守卫中的路由对象) 的 $route.matched 数组。  

在全局导航守卫中检查元字段：
```js
router.beforeEach((to, from, next) => {
  if (to.matched.some(record => record.meta.requiresAuth)) {
    // this route requires auth, check if logged in
    // if not, redirect to login page.
    if (!auth.loggedIn()) {
      next({
        path: '/login',
        query: { redirect: to.fullPath }
      })
    } else {
      next()
    }
  } else {
    next() // 确保一定要调用 next()
  }
})
```

## 过渡动效

<router-view> 是基本的动态组件，所以我们可以用 <transition> 组件给它添加一些过渡效果：

```html
<transition>
  <router-view></router-view>
</transition>
```

### 单个路由的过渡

```js
const Foo = {
  template: `
    <transition name="slide">
      <div class="foo">...</div>
    </transition>
  `
}

const Bar = {
  template: `
    <transition name="fade">
      <div class="bar">...</div>
    </transition>
  `
}
```

### 基于路由的动态过渡

```html
<!-- 使用动态的 transition name -->
<transition :name="transitionName">
  <router-view></router-view>
</transition>
```
```js
// 接着在父组件内
// watch $route 决定使用哪种过渡
watch: {
  '$route' (to, from) {
    const toDepth = to.path.split('/').length
    const fromDepth = from.path.split('/').length
    this.transitionName = toDepth < fromDepth ? 'slide-right' : 'slide-left'
  }
}
```

## 数据获取

两种实现方式：

- 导航完成之后获取：先完成导航，然后在接下来的组件生命周期钩子中获取数据。在数据获取期间显示“加载中”之类的指示。
- 导航完成之前获取：导航完成前，在路由进入的守卫中获取数据，在数据获取成功后执行导航。

## 滚动行为

```js
const router = new VueRouter({
  routes: [...],
  scrollBehavior (to, from, savedPosition) {
    // return 期望滚动到哪个的位置
  }
})
```

返回滚动位置的对象信息：
- { x: number, y: number }
- { selector: string, offset? : { x: number, y: number }} (offset 只在 2.6.0+ 支持)

## 路由懒加载

> 把不同路由对应的组件分割成不同的代码块，然后当路由被访问的时候才加载对应组件。
> 结合 Vue 的异步组件和 Webpack 的代码分割功能，实现路由组件的懒加载。

```js
const Foo = () => import('./Foo.vue')
```
**Notice:** 如果使用的是 Babel，需要添加 syntax-dynamic-import 插件，才能使 Babel 可以正确地解析语法。

其他方式：
```js
// 报表
const Report = resolve => require(['@/components/pages/Report'], resolve)
```

### 组件按组分块

```js
const Foo = () => import(/* webpackChunkName: "group-foo" */ './Foo.vue')
const Bar = () => import(/* webpackChunkName: "group-foo" */ './Bar.vue')
const Baz = () => import(/* webpackChunkName: "group-foo" */ './Baz.vue')
```
