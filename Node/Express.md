# Express 官网笔记

[express 官网](http://www.expressjs.com.cn/)

* [express 应用生成器](express-应用生成器)
* [静态文件托管](#静态文件托管)
* [路由]()
    - [路由](#路由)
    - [路由路径](#路由路径)
    - [路由句柄](#路由句柄)
    - [响应方法](#响应方法)
    - [app.route()](#app.route())
    - [express.Router](#express.Router)

# express 应用生成器

```bash
npm install express-generator -g
```

生成的目录结构如下：
```bash
.
├── app.js
├── bin
│   └── www
├── package.json
├── public
│   ├── images
│   ├── javascripts
│   └── stylesheets
│       └── style.css
├── routes
│   ├── index.js
│   └── users.js
└── views
    ├── error.jade
    ├── index.jade
    └── layout.jade
```

# 静态文件托管

> 通过 Express 内置的 express.static 可以方便地托管静态文件。

示例：
```js
app.use(express.static('public'));
app.use('/static', express.static('public')); // 使用虚拟目录 static
```

访问地址：
```bash
http://localhost:3000/images/kitten.jpg
http://localhost:3000/static/images/kitten.jpg
```
**注意：** 所有文件的路径都是相对于存放目录的，因此，存放静态文件的目录名不会出现在 URL 中。

# 路由

## 路由定义

> 路由是指如何定义应用的端点（URIs）以及如何响应客户端的请求。

```bash
var express = require('express');
var app = express();
app.METHOD(path, [callback...], callback)
```
释义：

    app 是一个 express 实例；
    METHOD 是某个 HTTP 请求方式中的一个；
    path 是服务器端的路径；
    callback 是当路由匹配到时需要执行的函数。

示例：
```js
// 对网站首页的访问返回 "Hello World!" 字样
app.get('/', function (req, res) {
  res.send('Hello World!');
});

// 网站首页接受 POST 请求
app.post('/', function (req, res) {
  res.send('Got a POST request');
});

// /user 节点接受 PUT 请求
app.put('/user', function (req, res) {
  res.send('Got a PUT request at /user');
});

// /user 节点接受 DELETE 请求
app.delete('/user', function (req, res) {
  res.send('Got a DELETE request at /user');
});
```

## 路由路径

> 路由路径和请求方法一起定义了请求的端点，它可以是字符串、字符串模式或者正则表达式。

示例：
```js
// 匹配 abcd、abxcd、abRABDOMcd、ab123cd等
app.get('/ab*cd', function(req, res) {
  res.send('ab*cd');
});

// 匹配任何路径中含有 a 的路径：
app.get(/a/, function(req, res) {
  res.send('/a/');
});

// 匹配 butterfly、dragonfly
app.get(/.*fly$/, function(req, res) {
  res.send('/.*fly$/');
});
```

## 路由句柄

> 可以为请求处理提供多个回调函数，其行为类似 中间件。唯一的区别是这些回调函数有可能调用 next('route') 方法而略过其他路由回调函数。

使用多个回调函数处理路由（记得指定 next 对象）：
```js
app.get('/example/b', function (req, res, next) {
  console.log('response will be sent by the next function ...');
  next();
}, function (req, res) {
  res.send('Hello from B!');
});
```

使用回调函数数组处理路由：
```js
var cb0 = function (req, res, next) {
  console.log('CB0');
  next();
}

var cb1 = function (req, res, next) {
  console.log('CB1');
  next();
}

var cb2 = function (req, res) {
  res.send('Hello from C!');
}

app.get('/example/c', [cb0, cb1, cb2]);
```

## 响应方法

> 下表中响应对象（res）的方法向客户端返回响应，终结请求响应的循环。如果在路由句柄中一个方法也不调用，来自客户端的请求会一直挂起。

方法 | 描述
- | -
res.download() | 提示下载文件
res.end() | 终结响应处理流程。
res.json() | 发送一个 JSON 格式的响应。
res.jsonp()	| 发送一个支持 JSONP 的 JSON 格式的响应。
res.redirect() | 重定向请求。
res.render() | 渲染视图模板。
res.send() | 发送各种类型的响应。
res.sendFile | 以八位字节流的形式发送文件。
res.sendStatus() | 设置响应状态代码，并将其以字符串形式作为响应体的一部分发送。

## app.route()

>  创建路由路径的链式路由句柄。

示例：
```js
app.route('/book')
  .get(function(req, res) {
    res.send('Get a random book');
  })
  .post(function(req, res) {
    res.send('Add a book');
  })
  .put(function(req, res) {
    res.send('Update the book');
  });
```

## express.Router

> 可使用 express.Router 类创建模块化、可挂载的路由句柄。Router 实例是一个完整的中间件和路由系统，因此常称其为一个 “mini-app”。

在 app 目录下创建名为 birds.js 的文件，内容如下：
```js
var express = require('express');
var router = express.Router();

// 该路由使用的中间件
router.use(function timeLog(req, res, next) {
  console.log('Time: ', Date.now());
  next();
});
// 定义网站主页的路由
router.get('/', function(req, res) {
  res.send('Birds home page');
});
// 定义 about 页面的路由
router.get('/about', function(req, res) {
  res.send('About birds');
});

module.exports = router;
```
然后在应用中加载路由模块：
```js
var birds = require('./birds');
...
app.use('/birds', birds);
```
应用即可处理发自 /birds 和 /birds/about 的请求，并且调用为该路由指定的 timeLog 中间件。

