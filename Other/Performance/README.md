# 性能优化

## 方案一

### 两个优化方向：
> 减少页面体积，提升网络加载速度  
> 优化页面渲染  

#### 减少页面体积，提升网络加载

    静态资源的压缩合并（JS 代码压缩合并、CSS 代码压缩合并、使用雪碧图、iconfont）
    静态资源缓存（资源名称加 MD5 戳）
    使用 CDN 让资源加载更快

#### 优化页面渲染
    CSS 放前面，JS 放后面
    懒加载（图片懒加载、下拉加载更多）
    减少DOM 查询，对 DOM 查询做缓存
    减少DOM 操作，多个操作尽量合并在一起执行（DocumentFragment）
    事件节流
    尽早执行操作（DOMContentLoaded）
    使用 SSR 后端渲染，数据直接输出到 HTML 中，减少浏览器使用 JS - 模板渲染页面 HTML 的时间
    
## 方案二

    请求数量：
    合并脚本和样式表，CSS Sprites，拆分初始化负载，划分主域。
    请求带宽：
    开启GZip，精简JavaScript，移除重复脚本，图像优化，将icon做成字体。
    缓存利用：
    使用CND，使用外部JS和CSS，添加Expires头，减少DNS查询，配置ETag，使Ajax可缓存。
    页面结构：
    将样式表放在顶部，将脚本放在底部，尽早刷新文档的输出。
    代码校验：
    避免CSS表达式，避免重定向。

SEE ALSO：

[《前端工程与性能分析》](https://div.io/topic/371)   
[一篇文章搞定前端面试 -- 前端性能优化](https://juejin.im/post/5bbaa549e51d450e827b6b13#heading-8)  
[vue + webpack 前端性能优化](https://juejin.im/post/5bc5c106e51d450e7a253e1b)  
