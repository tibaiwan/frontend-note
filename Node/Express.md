# Express 官网笔记

[express 官网](http://www.expressjs.com.cn/)

* [express 应用生成器](express-应用生成器)
* [静态文件托管](#静态文件托管)
* [路由](#路由)
    - [路由定义](#路由定义)
    - [路由路径](#路由路径)
    - [路由句柄](#路由句柄)
    - [响应方法](#响应方法)
    - [app.route()](#approute)
    - [express.Router](#expressrouter)
* [中间件]()
    - [使用中间件](#使用中间件)
    - [应用级中间件](#应用级中间件)
    - [路由级中间件](#路由级中间件)
    - [错误处理中间件](#错误处理中间件)
    - [内置中间件](#内置中间件)
    - [第三方中间件](#第三方中间件)
* [模板引擎](#模板引擎)
    - [使用Jade](#使用jade)
    - [使用Handlebars](#使用handlebars)
* [调试 Express](调试-express)
    - [命令](#命令)
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

| 方法 | 描述 |
| - | - |
| res.download() | 提示下载文件 |
| res.end() | 终结响应处理流程。 |
| res.json() | 发送一个 JSON 格式的响应。 |
| res.jsonp()	| 发送一个支持 JSONP 的 JSON 格式的响应。 |
| res.redirect() | 重定向请求。 |
| res.render() | 渲染视图模板。 |
| res.send() | 发送各种类型的响应。 |
| res.sendFile | 以八位字节流的形式发送文件。 |
| res.sendStatus() | 设置响应状态代码，并将其以字符串形式作为响应体的一部分发送。 |

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

# 中间件

## 使用中间件

> Express 是一个自身功能极简，完全是由路由和中间件构成一个的 web 开发框架，从本质上来说，一个 Express 应用就是在调用各种中间件。

> 中间件（Middleware） 是一个函数，它可以访问请求对象（req）, 响应对象（res）, 和 web 应用中处于请求-响应循环流程中的中间件，一般被命名为 next 的变量。

中间件的功能包括:

  -  执行任何代码
  -  修改请求和相应对象
  -  终结请求和响应循环
  -  调用堆栈中下一个中间件

**Notice:**   
1、如果当前中间件没有终结请求-响应循环，则必须调用 next() 方法将控制权交给下一个中间件，否则请求就会挂起。  
2、如果需要在中间件栈中跳过剩余中间件，调用 next('route') 方法将控制权交给下一个路由。

## 应用级中间件

> 应用级中间件绑定到 app 对象 使用 app.use() 和 app.METHOD()

示例：
```js
var app = express();

// 没有挂载路径的中间件，应用的每个请求都会执行该中间件
app.use(function (req, res, next) {
  console.log('Time:', Date.now());
  next();
});

// 挂载至 /user/:id 的中间件，任何指向 /user/:id 的请求都会执行它
app.use('/user/:id', function (req, res, next) {
  console.log('Request Type:', req.method);
  next();
});

// 路由和句柄函数(中间件系统)，处理指向 /user/:id 的 GET 请求
app.get('/user/:id', function (req, res, next) {
  res.send('USER');
});
```

## 路由级中间件

> 路由级中间件和应用级中间件一样，只是它绑定的对象为 express.Router()。

示例：
```js
var router = express.Router();

var app = express();
var router = express.Router();

// 没有挂载路径的中间件，通过该路由的每个请求都会执行该中间件
router.use(function (req, res, next) {
  console.log('Time:', Date.now());
  next();
});

// 一个中间件栈，显示任何指向 /user/:id 的 HTTP 请求的信息
router.use('/user/:id', function(req, res, next) {
  console.log('Request URL:', req.originalUrl);
  next();
}, function (req, res, next) {
  console.log('Request Type:', req.method);
  next();
});

// 一个中间件栈，处理指向 /user/:id 的 GET 请求
router.get('/user/:id', function (req, res, next) {
  // 如果 user id 为 0, 跳到下一个路由
  if (req.params.id == 0) next('route');
  // 负责将控制权交给栈中下一个中间件
  else next(); //
}, function (req, res, next) {
  // 渲染常规页面
  res.render('regular');
});

// 处理 /user/:id， 渲染一个特殊页面
router.get('/user/:id', function (req, res, next) {
  console.log(req.params.id);
  res.render('special');
});

// 将路由挂载至应用
app.use('/', router);
```

## 错误处理中间件

> 错误处理中间件和其他中间件定义类似，只是要使用 4 个参数，其签名如下： (err, req, res, next)。

示例：
```js
app.use(function(err, req, res, next) {
  console.error(err.stack);
  res.status(500).send('Something broke!');
});
```

## 内置中间件

+ express.static(root, [options])

  express.static 是 Express 唯一内置的中间件，负责在 Express 应用中提托管静态资源。

示例：
```js
app.use(express.static('public'));
app.use(express.static('uploads'));
app.use(express.static('files'));
```

## 第三方中间件

> 安装所需功能的 node 模块，并在应用中加载，可以在应用级加载，也可以在路由级加载。

示例：
```js
var express = require('express');
var app = express();
var cookieParser = require('cookie-parser');

// 加载用于解析 cookie 的中间件
app.use(cookieParser());
```

# 模板引擎

> 有一些模板引擎不遵循这种约定，Consolidate.js 能将 Node 中所有流行的模板引擎映射为这种约定，这样就可以和 Express 无缝衔接。

## 使用Jade

设置如下：
```js
app.set('views', './views') // 放模板文件的目录
app.set('view engine', 'jade') // 模板引擎
```

## 使用Handlebars

设置如下：
```js
var exphbs = require('express-handlebars');

app.set('views', path.join(__dirname, 'views'));
app.engine('hbs', exphbs({
  layoutsDir: 'express/views',
  defaultLayout: 'layout',
  extname: '.hbs'
}));
app.set('view engine', 'hbs');
```

# 调试 Express

> Express 内部使用 debug 模块记录路由匹配、使用到的中间件、应用模式以及请求-响应循环。

## 命令
```bash
$ DEBUG=express:* node index.js
```

## Windows 命令
```bash
> set DEBUG=express:* & node index.js
```

