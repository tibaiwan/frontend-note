# Vue 配置相关问题

## 兼容 IE 浏览器

IE报：vuex requires a Promise polyfill in this browser.

安装 `babel-polyfill`
```js
npm install -S babel-polyfill
```

在入口文件 `main.js` 引入
```js
import 'babel-polyfill'
```

配置（webpack.conf.js）
```js
module.exports ={
  entry: {
    app: ['babel-polyfill', './src/main.js']
  }
}
```
**注意：** `babel-polyfill` 必要在`main.js`前面才能生效。

## 配置 proxyTable

> Vue 暴露了 proxyTabe 做跨域处理

config/index.js
```js
dev: {
    env: require('./dev.env'),
    port: 8080,
    autoOpenBrowser: true,
    assetsSubDirectory: 'static',
    assetsPublicPath: '/',
    proxyTable: {},   
    cssSourceMap: false
  }
```

如果接口是：`https://www.exaple.com/server_new/login`  
那么接口请求url： `/server_new/login`  
proxyTable 配置如下：
```js
proxyTable: {
        '/apis': {    //将www.exaple.com印射为/apis
            target: 'https://www.exaple.com',  // 接口域名
            secure: false,  // 如果是https接口，需要配置这个参数
            changeOrigin: true,  //是否跨域
            pathRewrite: {
                '^/apis': ''   //需要rewrite的,
            }              
        }
  }
```

[See Also](https://segmentfault.com/a/1190000011007043)
