# Vue 配置相关问题

## proxyTable

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
