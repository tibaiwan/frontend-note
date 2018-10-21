# 函数防抖和函数节流

## 函数防抖

> 概念

当调用动作过n毫秒后，才会执行该动作，若在这n毫秒内又调用此动作则将重新计算执行时间。

> 使用场景

- window对象的resize、scroll事件
- 对于输入框连续输入进行AJAX验证时，用函数防抖能有效减少请求次数
- 给按钮加函数防抖防止表单多次提交

> 简单实现函数防抖（debounce）

```js
function _debounce(fn,wait){
    var timer = null;
    return function(){
        clearTimeout(timer)
        timer = setTimeout(()=>{
            fn()
        }, wait)
    }
}

function _log(){
    console.log(1)
}
window.onscroll = _debounce(_log, 500)
```

## 函数节流

> 概念

预先设定一个执行周期，当调用动作的时刻大于等于执行周期则执行该动作，然后进入下一个新周期。

> 使用场景

- DOM 元素的拖拽功能实现（mousemove）
- 射击游戏的 mousedown/keydown 事件（单位时间只能发射一颗子弹）
- 计算鼠标移动的距离（mousemove）
- Canvas 模拟画板功能（mousemove）
- 搜索联想（keyup）
- 监听滚动事件判断是否到页面底部自动加载更多：给 scroll 加了 debounce 后，只有用户停止滚动后，才会判断是否到了页面底部；如果是 throttle 的话，只要页面滚动就会间隔一段时间判断一次

> 简单实现函数节流（throttle）

```js

function _throttle(fn, wait, time){
    var previous = null; // 记录上一次运行的时间
    var timer = null;

    return function(){
        var now = +new Date();

        if(!previous) previous = now;
        // 当上一次执行的时间与当前的时间差大于设置的执行间隔时长的话，就主动执行一次
        if(now - previous > time){
            clearTimeout(timer);
            fn();
            previous = now; // 执行函数后，马上记录当前时间
        }else{
            clearTimeout(timer);
            timer = setTimeout(function(){
                fn();
            }, wait);
        }
    }
}
function _log(){
    console.log(1)
}
window.onscroll = _debounce(_log, 500, 2000)
```

## 防抖和节流区别

**函数防抖**：如果有人进电梯（触发事件），那电梯将在10秒钟后出发（执行事件监听器），这时如果又有人进电梯了（在10秒内再次触发该事件），我们又得等10秒再出发（重新计时）。

**函数节流**：就像公交车，每20分钟（固定的时间间隔）发一班车，不管前方多少人在等车。

Strong 版的防抖和节流函数可参考 [lodash](https://github.com/lodash/lodash)

See Also:  
[函数节流与函数防抖](https://segmentfault.com/a/1190000008768202)  
[JavaScript 函数节流和函数去抖应用场景辨析](https://github.com/hanzichi/underscore-analysis/issues/20)  
[JavaScript专题之跟着underscore学防抖](https://github.com/mqyqingfeng/Blog/issues/22)  
[JavaScript专题之跟着underscore学节流](https://github.com/mqyqingfeng/Blog/issues/26)  
