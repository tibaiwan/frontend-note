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

