# Ajax 相关

## 实现 ajax 请求

回调函数：
```js
var Request = {
    get: function(url, fn) {
        // XMLHttpRequest对象用于在后台与服务器交换数据
        var xhr = new XMLHttpRequest();
        xhr.open('GET', url, true);
        xhr.onreadystatechange = function() {
            // readyState == 4说明请求已完成
            if (xhr.readyState == 4 && xhr.status == 200) {
                // 从服务器获得数据
                fn.call(this, xhr.responseText);
            }
        };
        xhr.send();
    }
}
```

使用 Promise

```js
var Request = {
  get: function(url) {
      return new Promise((resolve, reject) => {
          var xhr = new XMLHttpRequest();
          xhr.open('GET', url, true);
          xhr.onreadystatechange = function() {
              // readyState == 4说明请求已完成
              if (xhr.readyState == 4 && xhr.status == 200) {
                  // 从服务器获得数据
                  resolve(xhr.responseText)
              }
          };
          xhr.send();
      })
  },
}
```

## 取消 ajax 请求

```js
var native = new XMLHttpRequest();
native.open("GET","https://api.github.com/");
native.send();
native.onreadystatechange=function(){
    if(native.readyState==4&&native.status==200){
        console.log(native.response);           
    }else{
        console.log(native.status);
    }
}
native.abort();
```
