title: 前端跨域
date: 2017-08-25 18:29:00
description: 
categories:
- JavaScript
tags:
- Cross
toc: true
author:
comments:
original:
permalink: 
---
跨域一词从字面意思看，就是跨域名嘛，但实际上跨域的范围绝对不止那么狭隘。具体概念如下：只要协议、域名、端口有任何一个不同，都被当作是不同的域。之所以会产生跨域这个问题呢，其实也很容易想明白，要是随便引用外部文件，不同标签下的页面引用类似的彼此的文件，浏览器很容易懵逼的，安全也得不到保障了就。什么事，都是安全第一嘛。但在安全限制的同时也给注入iframe或是ajax应用上带来了不少麻烦。所以我们要通过一些方法使本域的js能够操作其他域的页面对象或者使其他域的js能操作本域的页面对象（iframe之间）。
<!-- more -->

# 跨域                                         
|  URL | 说明 | 是否允许通信 |
|  ---- | ---- | ---- |
| http://www.a.com/a.js | | |
| http://www.a.com/b.js        | 同一域名下             |  允许 |
| http://www.a.com/lab/a.js | | |
| http://www.a.com/script/b.js | 同一域名下不同文件夹    |  允许 |
| http://www.a.com:8000/a.js | | |
| http://www.a.com/b.js        | 同一域名，不同端口       | 不允许 |
| http://www.a.com/a.js | | |
| https://www.a.com/b.js       | 同一域名，不同协议       | 不允许 |
| http://www.a.com/a.js | | |
| http://70.32.92.74/b.js      | 域名和域名对应ip        | 不允许 |
| http://www.a.com/a.js | | |
| http://script.a.com/b.js     | 主域相同，子域不同       | 不允许（cookie这种情况下也不允许访问） |
| http://www.a.com/a.js | | |
| http://a.com/b.js            | 同一域名，不同二级域名（同上）| 不允许（cookie这种情况下也不允许访问） |
| http://www.cnblogs.com/a.js | | |
| http://www.a.com/b.js        | 不同域名                 | 不允许 |

## 判断方法
```javascript
<!-- 协议 -->
window.location.protocol
<!-- 域名 -->
window.location.host
```
## 注意
1. 如果是协议和端口造成的跨域问题“前台”是无能为力的；
1. 在跨域问题上，域仅仅是通过“URL的首部”来识别而不会去尝试判断相同的ip地址对应着两个域或两个域是否在同一个ip上。

## 浏览器有一个同源策略：
1. 不能通过ajax的方法去请求不同源中的文档。 
1. 限制是浏览器中不同域的框架之间是不能进行js的交互操作的。

# 方法
## document.domain
只适用于不同子域的框架间的交互。

|  URL | 说明 | 是否允许通信 |
|  ---- | ---- | ---- |
| http://www.damonare.cn/a.html | | |
| http://damonare.cn/b.html | 同一域名，不同二级域名（同上）| 不允许（cookie这种情况下也不允许访问） |

> 案例

有一个页面，它的地址是http://www.damonare.cn/a.html ， 在这个页面里面有一个iframe，它的src是http://damonare.cn/b.html, 很显然，这个页面与它里面的iframe框架是不同域的，所以我们是无法通过在页面中书写js代码来获取iframe中的东西的：
```javascript
<script type="text/javascript">
    function test(){
        var iframe = document.getElementById('￼ifame');
        // 可以获取到iframe里的window对象，但该window对象的属性和方法几乎是不可用的
        var win = document.contentWindow;
        // 这里获取不到iframe里的document对象
        var doc = win.document;
        // 这里同样获取不到window对象的name属性
        var name = win.name;
    }
</script>
<iframe id = "iframe" src="http://damonare.cn/b.html" onload = "test()"></iframe>
```

```javascript
<iframe id = "iframe" src="http://damonare.cn/b.html" onload = "test()"></iframe>
<script type="text/javascript">
    // 设置成主域
    document.domain = 'damonare.cn';
    function test(){
        // contentWindow 可取得子窗口的 window 对象
        alert(document.getElementById('iframe').contentWindow);
    }
</script>

<!-- 在页面http://damonare.cn/b.html 中也设置document.domain: -->
<script type="text/javascript">
    // 在iframe载入这个页面也设置document.domain，使之与主页面的document.domain相同
    document.domain = 'damonare.cn';
</script>
```
## location.hash
因为父窗口可以对iframe进行URL读写，iframe也可以读写父窗口的URL，URL有一部分被称为hash，就是#号及其后面的字符，它一般用于浏览器锚点定位，Server端并不关心这部分，应该说HTTP请求过程中不会携带hash，所以这部分的修改不会产生HTTP请求，但是会产生浏览器历史记录。此方法的原理就是改变URL的hash部分来进行双向通信。
每个window通过改变其他 window的location来发送消息（由于两个页面不在同一个域下IE、Chrome不允许修改parent.location.hash的值，所以要借助于父窗口域名下的一个代理iframe），并通过监听自己的URL的变化来接收消息。这个方式的通信会造成一些不必要的浏览器历史记录，而且有些浏览器不支持onhashchange事件，需要轮询来获知URL的改变

缺点:
诸如数据直接暴露在了url中，数据容量和类型都有限等。


> 案例

假如父页面是baidu.com/a.html,iframe嵌入的页面为google.com/b.html（此处省略了域名等url属性），要实现此两个页面间的通信可以通过以下方法。
1. a.html传送数据到b.html
1. a.html下修改iframe的src为google.com/b.html#paco
1. b.html监听到url发生变化，触发相应操作
1. b.html传送数据到a.html，由于两个页面不在同一个域下IE、Chrome不允许修改parent.location.hash的值，所以要借助于父窗口域名下的一个代理iframe
1. b.html下创建一个隐藏的iframe，此iframe的src是baidu.com域下的，并挂上要传送的hash数据，如src=”http://www.baidu.com/proxy.html#data”
1. proxy.html监听到url发生变化，修改a.html的url（因为a.html和proxy.html同域，所以proxy.html可修改a.html的url hash）
1. a.html监听到url发生变化，触发相应操作

```javascript
try {  
    parent.location.hash = 'data';  
} catch (e) {  
    // ie、chrome的安全机制无法修改parent.location.hash，  
    var ifrproxy = document.createElement('iframe');  
    ifrproxy.style.display = 'none';  
    ifrproxy.src = "http://www.baidu.com/proxy.html#data";  
    document.body.appendChild(ifrproxy);  
}
```

```javascript
// 因为parent.parent（即baidu.com/a.html）和baidu.com/proxy.html属于同一个域，所以可以改变其location.hash的值  
parent.parent.location.hash = self.location.hash.substring(1);
```



## postMessage


## window.name
即在一个窗口(window)的生命周期内,窗口载入的所有的页面都是共享一个window.name的，每个页面对window.name都有读写的权限，window.name是持久存在一个窗口载入过的所有页面中的，并不会因新页面的载入而进行重置。

特点：
1. 设置与窗口上，不会因为页面跳转而消失。
1. 窗口消失后将，不会存在。

```javascript
<!-- 我们在任意一个页面输入 -->
window.name = "My window's name";
setTimeout(function(){
    window.location.href = "http://damonare.cn/";
},1000)
```

```javascript
<!-- 进入damonare.cn页面后，返回我们再检测再检测 window.name : -->
window.name;
// My window's name
```

> 案例

可以看到，如果在一个标签里面跳转网页的话，我们的 window.name 是不会改变的。基于这个思想，我们可以在某个页面设置好 window.name 的值，然后跳转到另外一个页面。在这个页面中就可以获取到我们刚刚设置的 window.name 了。

注意：由于安全原因，浏览器始终会保持 window.name 是string 类型。
我的页面(http://damonare.cn/index.html)中内嵌了一个iframe：
```
<iframe id="iframe" src="http://www.google.com/iframe.html"></iframe>
```

```javascript
var iframe = document.getElementById('iframe');
var data = '';

iframe.onload = function() {
    data = iframe.contentWindow.name;
};
```

Boom!报错！肯定的，因为两个页面不同源嘛，想要解决这个问题可以这样干：

```javascript
var iframe = document.getElementById('iframe');
var data = '';

iframe.onload = function() {
    iframe.onload = function(){
        data = iframe.contentWindow.name;
    }
    iframe.src = 'about:blank';
};
```

## CORS[服务器]
CORS（Cross-Origin Resource Sharing）跨域资源共享，它允许浏览器向跨源服务器，发出XMLHttpRequest请求，从而克服了AJAX只能同源使用的限制。
定义了必须在访问跨域资源时，浏览器与服务器应该如何沟通。
CORS背后的基本思想就是使用自定义的HTTP头部让浏览器与服务器进行沟通，从而决定请求或响应是应该成功还是失败。
目前，所有浏览器都支持该功能，IE浏览器不能低于IE10。整个CORS通信过程，都是浏览器自动完成，不需要用户参与。对于开发者来说，CORS通信与同源的AJAX通信没有差别，代码完全一样。浏览器一旦发现AJAX请求跨源，就会自动添加一些附加的头信息，有时还会多出一次附加的请求，但用户不会有感觉。

注意：
1. 实现CORS通信的关键是服务器。只要服务器实现了CORS接口，就可以跨源通信。
1. IE浏览器不能低于IE10。
1. 浏览器一旦发现AJAX请求跨源，就会自动添加一些附加的头信息。
1. 

1. 服务器端对于CORS的支持
1. 通过设置Access-Control-Allow-Origin



```javascript
<script type="text/javascript">
    var xhr = new XMLHttpRequest();
    xhr.open("￼GET", "http://segmentfault.com/u/trigkit4/",true);
    xhr.send();
</script>
```

- [跨域资源共享 CORS 详解](http://www.ruanyifeng.com/blog/2016/04/cors.html "")

### CORS请求分类

#### 简单请求（simple request）

> AJAX请求

```
GET /cors HTTP/1.1
Origin: http://api.bob.com
<!-- Origin（协议 + 域名 + 端口） -->
Host: api.alice.com
Accept-Language: en-US
Connection: keep-alive
User-Agent: Mozilla/5.0...
```

> 服务器响应

如果Origin指定的域名在许可范围内，服务器返回的响应，会多出几个头信息字段。
```
Access-Control-Allow-Origin: http://api.bob.com
Access-Control-Allow-Credentials: true
Access-Control-Expose-Headers: FooBar
Content-Type: text/html; charset=utf-8
```
Access-Control-Allow-Origin
该字段是必须的。它的值要么是请求时Origin字段的值，要么是一个*，表示接受任意域名的请求。

Access-Control-Allow-Credentials
该字段可选。它的值是一个布尔值，表示是否允许发送Cookie。默认情况下，Cookie不包括在CORS请求之中。设为true，即表示服务器明确许可，Cookie可以包含在请求中，一起发给服务器。这个值也只能设为true，如果服务器不要浏览器发送Cookie，删除该字段即可。

Access-Control-Expose-Headers
该字段可选。CORS请求时，XMLHttpRequest对象的getResponseHeader()方法只能拿到6个基本字段：Cache-Control、Content-Language、Content-Type、Expires、Last-Modified、Pragma。如果想拿到其他字段，就必须在Access-Control-Expose-Headers里面指定。上面的例子指定，getResponseHeader('FooBar')可以返回FooBar字段的值。

#### 非简单请求（not-so-simple request）

### CORS和JSONP对比
1. CORS与JSONP的使用目的相同，但是比JSONP更强大。
1. JSONP只能实现GET请求，而CORS支持所有类型的HTTP请求。
1. 使用CORS，开发者可以使用普通的XMLHttpRequest发起请求和获得数据，比起JSONP有更好的错误处理。
1. JSONP的优势在于支持老式浏览器，以及可以向不支持CORS的网站请求数据。
1. CORS与JSONP相比，无疑更为先进、方便和可靠。


JSONP只支持GET请求，CORS支持所有类型的HTTP请求。

```javascript
```


# 拓展

1. 中间件跨域
1. 服务器代理跨域
1. Flash URLLoader跨域
1. 动态创建script标签（简化版本的jsonp）不作讨论。

- [前端跨域知识总结](https://mp.weixin.qq.com/s?__biz=MzAxODE2MjM1MA==&mid=2651552471&idx=1&sn=794c182bd4504930650beae83fc020aa&chksm=8025ad16b7522400df25d0156c4327123f21118e82ee71c5c6235513a2455819a3bb0e023f75&scene=0&key=01cef8d7e510532405654c6e62029331b46223acd4904fc4a838032303910abb4039d981008c2e9138aba6406a90c7338d63c9f2e16ab319b9c705fc73f4fcea55905da3aa8fdf89e314ae0e30320aa9&ascene=0&uin=NjgwMDIxNzEw&devicetype=iMac+MacBookPro12%2C1+OSX+OSX+10.12.3+build(16D32)&version=12020810&nettype=WIFI&fontScale=100&pass_ticket=8FRxRzxcZuPPecHqrc219fsuSg9sxZtW%2BsLmdCLPav9%2Bfi6P1q7be9QXl0zABcV0 "")
- [详解js跨域问题](http://www.cnblogs.com/aoldman/p/4666406.html "")
- [详解js跨域问题](https://segmentfault.com/a/1190000000718840 "")
- []( "")
- []( "")
- []( "")
- []( "")





