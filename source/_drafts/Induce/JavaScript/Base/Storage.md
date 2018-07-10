title: JavaScript 本地存储
date: 2018-04-25 18:29:00
description:
categories:
- JavaScript
tags:
- JavaScript
toc: true
author:
comments:
original:
permalink:

---
# 概况
离线Web应用，就是在设备不能上网的情况下，仍然可以运行的应用。HTML5把离线应用作为重点。

！！！！未完成IndexedDB
<!-- more -->

# 离线检测
## navigator.onLine
``` javascript
if (navigator.onLine) {
	// 正常工作
} else {
	// 执行离线状态时的任务
}
```
兼容性：
1. IE6+和Safari5+能够正确检测

## 网络判断
### 事件判断
可以使用HTML5事件判断：online、offline
- [兼容性](https://caniuse.com/#search=online "")

支持离线检测的浏览器有IE6+（只支持navigator.onLine）、Firefox3、Safari4、Opera10.6、Chrome、iOS3.2版Safari和Android版WebKit。
``` javascript
var EventUtil = {
    addHandler: function (element, type, handler) {
        if (element.addEventListener) {
            element.addEventListener(type, handler, false)
        } else if (element.attachEvent) {
            element.attachEvent('on' + type, handler)
        } else {
            element['on' + type] = handler
        }
    },
    removeHandler: function (element, type, handler) {
        if (element.removeEventListener) {
            element.removeEventListener(type, handler, false)
        } else if (element.detachEvent) {
            element.detachEvent('on' + type, handler)
        } else {
            element['on' + type] = null
        }
    },
    getEvent: function (event) {
        return event ? event : window.event
    },
    getTarget: function (event) {
        return event.target || event.srcElement
    },
    preventDefault: function (event) {
        if (event.preventDefault) {
            event.preventDefault()
        } else {
            event.returnValue = false
        }
    },
    stopPropagation: function (event) {
        if (event.stopPropagation) {
            event.stopPropagation()
        } else {
            event.cancelBubbles = true
        }
    },
    getRelatedTarget: function (event) {
        if (event.relatedTarger) {
            return event.relatedTarget
        } else if (event.toElement) {
            return event.toElement
        } else if (event.fromElement) {
            return event.fromElement
        } else { return null }
    }
}

EventUtil.addHandler(window, "online", function(){
	alert("online")
})
EventUtil.addHandler(window, "offline", function(){
    alert("Offline")
})
```
总之在IE和Firefox中一般情况下不能触发这俩事件，只有在选择脱机状态下才能触发此事件。(存在兼容问题)
``` javascript
window.addEventListener('load', function() {
    var status = document.getElementById('status');
    function updateOnlineStatus(event) {
        var condition = navigator.onLine ? 'online' : 'offline'
        status.className = condition
        status.innerHTML = condition.toUpperCase()
    }
    window.addEventListener('online',  updateOnlineStatus)
    window.addEventListener('offline', updateOnlineStatus)
})
```

### 图片轮询
``` javascript
<!-- jquery -->
setInterval(function(){
    var imgD = $('<img src="https://www.baidu.com/favicon.ico?'+(new Date())+'">')
    imgD.appendTo('body').css('display','none').load(function(){
        console.log('连接成功！')
        $(this).remove()
    }).error(function(){
        console.log('断网了！')
        $(this).remove()
    })
},2000)

<!-- js -->
setInterval(function () {
	var div = document.createElement('img');
	div.id = "mDiv";
	let isLoad = false
	div.src = 'https://www.baidu.com/favicon.ico?' + (new Date())
	div.style.display = 'none'
	div.innerHTML = "新元素";
	div.onload = function () {
		isLoad = true
	}
	if (isLoad) {
	  console.log('连接成功！')
	} else {
	  console.log('断网了！')
	}
	document.body.appendChild(div);
})
有个问题请求的延迟问题

setInterval(function () {
	let image = new Image()
	let isLoad = false
	// image.crossOrigin = ''
	image.src = 'https://www.baidu.com/favicon.ico?' + (new Date())
	image.onload = function () {
	  console.log('连接成功！')
	}
	image.onerror = function () {
	  console.log('断网了！')
	}
})
```

### Ajax轮询
``` javascript
setInterval(function(){
    //Ajax...
},1000)
```

# 应用缓存（已废弃）


# 数据存储
在客户端(浏览器端)存储数据有诸多益处，最主要的一点是能快速访问(网页)数据。(以往)在客户端有五种数据存储方法，而目前就只有四种常用方法了(其中一种被废弃了)：
1. Cookies
1. Local Storage
1. Session Storage
1. IndexedDB
1. WebSQL (被废弃)

## cookie
Cookies 是一种在文档内存储字符串数据最典型的方式。大小4K，限制20个。
一般而言，cookies 会由服务端发送给客户端，客户端存储下来，然后在随后让请求中再发回给服务端。这可以用于诸如管理用户会话，追踪用户信息等事情。此外，客户端也用使用 cookies 存储数据。因而，cookies 常被用于存储一些通用的数据，如用户的首选项设置。
### 构成

名称 | 描述
-------|------
Name | 唯一确定cookie的名称（不区分大小写）名称必须经过URL编码
Value | 存储的字符串值，必须经过URL编码
Domain | 对哪个域是有效的
Path | 对于指定域中的那个路径
Expires/Max-Age | 何时应该被删除的时间戳
secure | secure标志
HTTP | HTTP

#### Name
同一域名下绑定的cookie，`name`不能相同，相同的`name`的值会被覆盖掉

#### Value
这个就是每个 cookie 拥有的一个属性，它表示该属性的值

#### 编码和解码
cookie的名/值中的值不允许包含分号，逗号和空格符，encodeURIComponent编码和decodeURIComponent解码

#### Expires/Max-Age
max-age：[单位：秒 seconds] — 设置缓存最大的有效时间.
Expires：这个是代表当前时间的，这个属性已经逐渐被我们下面这个主人公所取代——Max-Age。

1. Max-Age，是以秒为单位的，Max-Age 为正数时，cookie 会在 Max-Age 秒之后被删除。
1. 当 Max-Age 为负数时，表示的是临时储存，不会生出 cookie 文件，只会存在浏览器内存中，且只会在打开的浏览器窗口或者子窗口有效，一旦浏览器关闭， cookie 就会消失
1. 当 Max-Age 为 0 时，又会发生什么呢，删除 cookie ，因为cookie 机制本身没有设置删除 cookie ，失效的 cookie 会被浏览器自动从内存中删除，所以，它实现的就是让 cookie 失效。

#### Path
1. path这个属性默认是/，当你设置成比如/blog的时候，其实它会给 “domain+path” 范围内绑定 cookie

#### Domain
1. 如果没有设置，就会自动绑定到执行语句的当前域
1. 统一个域名下的二级域名也是不可以交换使用 cookie
1. 你设置 www.baidu.com 和 image.baidu.com ，依旧是不能公用的

#### secure(服务端设置)
指定在网络上如何传输cookie，默认是不安全的。
这个属性译为安全，http 不仅是无状态的，还是不安全的协议，容易被劫持。
所以当这个属性设置为 true 时，此 cookie 只会在 https 和 ssl 等安全协议下传输。但需要强调一下这个属性并不能对客户端的cookie进行加密，不能保证绝对的安全性

#### HttpOnly(服务端设置)
1. `HttpOnly`属性限制了`cookie`对`HTTP`请求的作用范围。
1. 不能通过js脚本来获取cookie的值，能有效的防止xss攻击

注意: HttpOnly 属性和 Secure 属性相互独立：一个 cookie 既可以是 HttpOnly 的也可以有 Secure 属性。


### 操作

``` javascript
// Create 创建
document.cookie = "user_name = Ire Aderinokun"
document.cookie = "user_age = 25; max-age = 31536000; secure"

// Read (All) 读取
console.log(document.cookie)

// Update 更新
document.cookie = "user_age = 24;max-age = 31536000; secure"

// Delete 删除
document.cookie = "user_name = Ire Aderinokun; expires = Thu, 01 Jan 1970 00:00:01 GMT"
```

Name | Value | Domain | Path | Expires/Max-Age | Size | HTTP | secure |
-------|------|------|------|------|------|------
user_name | Ire Aderinokun |  | / | Session | 23 |  |  |
user_age | 25 |  | / | 2018-12-29T05:47:09.605Z | 10 |  | ✓ |

### 浏览器Cookie
#### HTTP 1.1 头部响应

``` javascript
HTTP/1.1 200 OK
Date: Fri, 30 Oct 1998 13:19:41 GMT
Server: Apache/1.3.3 (Unix)
Cache-Control: max-age=3600, must-revalidate
Expires: Fri, 30 Oct 1998 14:19:41 GMT
Last-Modified: Mon, 29 Jun 1998 02:28:12 GMT
ETag: "3e86-410-3596fbbc"
Content-Length: 1040
Content-Type: text/html
```


### HTTP专有cookie
HTTP专有cookie可以从浏览器或服务器设置，但是只能从服务器端读取，因为JavaScript无法获取HTTP专有cookie的值

> 在IE5.0 中自定义行为引入了持久化用户数据

``` javascript
<div style="behavior:url(#default#userData)" id="dataStore"></div>
<!-- 使用CSS在某个元素上指定userData行为，就可以使用setAttribute()保存上面的数据 -->

<!-- 设置 -->
var dataStore = document.getElementById("dataStore")
dataStore.setAttribute("name", "Nicholas")
dataStore.setAttribute("book", "Professional JavaScript")
dataStore.save("BookInfo")

<!-- 获取 -->
dataStore.load("BookInfo")
alert(dataStore.getAttribute("name"))
//"Nicholas"
alert(dataStore.getAttribute("book"))
//"Professional JavaScript"

<!-- 删除 -->
dataStore.removeAttribute("name")
dataStore.removeAttribute("book")
dataStore.save("BookInfo")
```

### 优点
1. 能用于和服务端通信，极高的扩展性和可用性
1. 当`cookie`快要自动过期时，我们可以重新设置而不是删除
1. 通过良好的编程，控制保存在cookie中的session对象的大小。
2. 通过加密和安全传输技术（SSL），减少cookie被破解的可能性。
3. 只在cookie中存放不敏感数据，即使被盗也不会有重大损失。
4. 控制cookie的生命期，使之不会永远有效。偷盗者很可能拿到一个过期的cookie。

### 缺点
1. 增加了文档传输的负载
1. 只能存储少量的数据
1. 只能存储字符串
1. 潜在的安全问题
1. 自从有 Web Storage API (Local and Session Storage)，cookies 就不再被推荐用于存储数据了
1. `Cookie`数量和长度的限制。每个domain最多只能有20条cookie，每个cookie长度不能超过4KB，否则会被截掉。
1. 安全性问题。如果cookie被人拦截了，那人就可以取得所有的session信息。即使加密也与事无补，因为拦截者并不需要知道cookie的意义，他只要原样转发cookie就可以达到目的了。
1. 有些状态不可能保存在客户端。例如，为了防止重复提交表单，我们需要在服务器端保存一个计数器。如果我们把这个计数器保存在客户端，那么它起不到任何作用。

- [cookies](https://github.com/jaaulde/cookies "")
- [Cookies.js](https://github.com/luuman/Works/tree/gh-pages/cookie "")
<!-- - [cookies.js](https://github.com/franciscop/cookies.js?utm_source=codropscollective "") -->
- [源码分析之：cookies.js](https://www.jianshu.com/p/a55969e078fb "")
- [客户端(浏览器端)数据存储技术概览](https://github.com/dwqs/blog/issues/42 "")


# Web Storage存储机制

两种存储数据的对象：sessionStorage、localStorage
sessionStorage：严格用于一个浏览器会话中存储数据，应为数据在关闭浏览器后会立即删除。
localStorage：用于跨会话持久化数据，并遵循跨域安全策略。

原因：
1. 提供一种在Cookie之外存储数据的途径
1. 提供一种存储大量可以跨会话存在的数据机制

## sessionStorage
Local Storage 是 Web Storage API 的一种类型，能在浏览器端存储键值对数据。
Local Storage 因提供了更直观和安全的API来存储简单的数据，被视为替代 Cookies 的一种解决方案。
从技术上说，尽管 Local Storage 只能存储字符串，但是它也是可以存储字符串化的JSON数据。这就意味着，Local Storage 能比 Cookies 存储更复杂的数据。

### 基本操作

``` javascript
// 使用方法存储数据
sessionStorage.setItem('name', 'Nicholas')
// 使用属性存储数据
sessionStorage.book = 'Professional JavaScript'

// 使用方法读取数据
var name = sessionStorage.getItem('name')
// 使用属性读取数据
var book = sessionStorage.book

// 使用方法删除数据
sessionStorage.removeItem('name')
// 使用delete删除数据
delete sessionStorage.name

// 使用方法获取Key
sessionStorage.key(0)

// 使用方法删除所有（Firefox没有）
sessionStorage.clear()
```

Key | Value
-------|------
name | Nicholas
book | Professional JavaScript

``` javascript
// IE8
sessionStorage.begin()
sessionStorage.name = 'Nicholas'
sessionStorage.book = 'Professional JavaScript'
sessionStorage.commit()
```

## localStorage

Local Storage 补充一点，当浏览器处于无痕浏览时，不会缓存任何数据

### 基本操作

``` javascript
// 使用方法存储数据
localStorage.setItem('name', 'Nicholas')
// 使用属性存储数据
localStorage.book = 'Professional JavaScript'

// 使用方法读取数据
var name = localStorage.getItem('name')
// 使用属性读取数据
var book = localStorage.book

// 使用方法删除数据
localStorage.removeItem('name')

// 使用方法获取Key
localStorage.key(0)

// 使用方法删除所有（Firefox没有）
localStorage.clear()
```

Key | Value
-------|------
name | Nicholas
book | Professional JavaScript

### 兼容globalStorage

``` javascript
function getLocalStorage() {
    if (typeof localStorage == 'object') {
        return localStorage
    } else if (typeof globalStorage == 'object') {
        return globalStorage[location.host]
    } else {
    	// 抛出异常
        throw new Error('Local storage not available.')
    }
}
var storage = getLocalStorage()
storage.setItem('named', 'Nicholasd')
```

## 限制
对存储空间大小的限制都是以每个来源（协议、域、端口）为单位的。每个来源都有固定大小的空间用于保存自己的数据。

1. localStorage大多数浏览器每个来源5MB限制，Chrome、Safari、IOSSafari、AndroidWebkit为2.5MB，
1. sessionStorage的Chrome、Safari、iOSSafari、AndroidWebKit为2.5MB，IE8+、Opera为5MB
1. Local Storage 补充一点，当浏览器处于无痕浏览时，不会缓存任何数据
- [Web Storage Support Test](http://dev-test.nemikor.com/web-storage/support-test/ "")

## globalStorage
globalStorage对象不是Storage的实例，而具体globalStorage['wrox.com']。

### 基本操作

``` javascript
// 存储数据
globalStorage['wrox.com'].name = 'Nicholas'

// 读取数据
var name = globalStorage['wrox.com'].name

// 可以让任何以net结尾的域名访问
globalStorage['net'].name = 'Nicholas'
// 可以让任何人访问
globalStorage[''].name = 'Nicholas'
```

Key | Value
-------|------
name | Nicholas
book | Professional JavaScript

## storage事件
对Storage对象进行任何修改，都会在文档上触发storage事件。
遗憾的是，webkit（chrome）还不支持这个事件，尽管 IE8 以及 ff 支持其部分属性，但因为 chrome 的不支持，注定其到目前为止还无法广泛使用

### event对象
key: 设置或删除的键名
oldValue: 键被更改之前的值
newValue: 如果是设置值，则是新值；如果是删除键，则是null
url*: key改变发生的URL

``` javascript

window.addEventListener('storage'.function(event){
  var output = document.getElementById("output")
  output.innerHTML += "原有值：" + event.oldValue
  output.innerHTML += "<br/>新值：" + event.newValue
  output.innerHTML += "<br/>变动页面地址：" + utf8_decode(unescape(event.url))
  console.log(event.storageArea)
  // 此行代码只能在chrome浏览器中有效  
  console.log(event.storageArea === localStorage)
  // 输出true
})
function utf8_decode(utfText) {
  var string = ""
  var i = 0
  var c = c1 = c2= 0
  while(i < utfText.length) {
    c = utfText.charCodeAt(i)
    if (c <128) {
      string += String.fromCharCode(c)
      i++
    } else if ((c > 191) && (c < 224)) {
      c2 = utfText.charCodeAt(i+1)
      string += String.fromCharCode(((c & 31) << 6) | (c2 & 63))
      i += 2
    } else {
      c2 = utfText.charCodeAt(i+1)
      c3 = utfText.charCodeAt(i+2)
      string += String.fromCharCode(((c & 15) << 12) | ((c2 & 63) << 6) | (c3 & 63))
      i += 3
    }
  }  
  return string
}

EventUtil.addHandler(document, 'storage', function(event){
    alert('Storage changed for ' + event.domain);
});
```

### 问题
如何使localStorage变更事件在当前页响应?
监听storage变更事件会发现，当数据发生变化时本页是监听不到storage事件变更消息的。而同域的其他打开的页面反而监听到了该消息。
在firefox和chrome中存储和读取都是正常的, 但是对storage事件的触发似乎有点问题, 自身页面进行setItem后没有触发window的storage事件, 但是同时访问A.html和B.html, 在A页面中进行 setItem能触发B页面中window的storage事件, 同样的在B页面中进行setItem能触发A页面中window的storage事件. 在IE9中, 页面自身的设值能触发当前页面的storage事件,同样当前页面的设值能触发同一”起源”下其他页面window的storage事件,这看起来似乎更让人想的通些.

直接打开两个链接：
- [storage事件1](http://www.css88.com/demo/sessionStorage/index2.html "")
- [storage事件2](http://www.css88.com/demo/sessionStorage/index3.html "")

### 自定义监听
``` javascript
var oriSetItem = localStorage.setItem
localStorage.setItem = function (key, value) {
    // 这里抛出自定义事件
    var event = new Event('setItemEvent')
    event.newValue = value
    window.dispatchEvent(event)
    // 实现原方法
    oriSetItem.apply(this, arguments)
}
window.addEventListener('setItemEvent', function (e) {
    console.log('本地存储已变化，新值为' + e.newValue)
})


var oriSetItem = localStorage.setItem
localStorage.setItem = function(k, v) {
	var se = document.createEvent('StorageEvent')
	se.initStorageEvent('storage', false, false, k, null, v, null, null)
	window.dispatchEvent(se)
	oriSetItem.apply(this, arguments)
}
```

- [初试WebStorage之localstorage](http://www.cnblogs.com/AndyWithPassion/archive/2011/07/04/html5_localstorage.html "")

# cookie、sessionStorage和localStorage的区别
## 存储时效
1. cookie可以手动设置失效期，默认为会话级
1. sessionStorage的存储时长是会话级
1. localStorage的存储时长是永久，除非用户手动利用浏览器的工具删除

## 访问的局限性
1. cookie可以设置路径path，所有他要比另外两个多了一层访问限制
1. localStorage和sessionStorage的访问限制是文档源级别，即协议、主机名和端口
1. 还要注意的是，cookie可以通过设置domain属性值，可以不同二级域名下共享cookie，而Storage不可以，比如http://image.baidu.com的cookie http://map.baidu.com是可以访问的，前提是Cookie的domain设置为".http://baidu.com"，而Storage是不可以的（这个很容易实验，就不细说了）

## 存储大小限制
1. cookie适合存储少量数据，他的大小限制是个数进行限制，每个浏览器的限制数量不同
1. Storage的可以存储数据的量较大，此外他是通过占用空间大小来做限制的，每个浏览器的实现也是不同的，大家可以看这篇文章来进一进步了解[Web Storage Support Test](http://dev-test.nemikor.com/web-storage/support-test/ "")

## 操作方法
1. cookie是作为document的属性存在，并没有提供标准的方法来直接操作cookie
1. Storage提供了setItem()和getItem()还有removeItem()方法，操作方便不易出错

## 其他
1. cookie在发送http请求时，会将本地的cookie作为http头部信息传递给服务器
1. cookie可以由服务器通过http来设定

# IndexedDB
是一种类似SQL数据库的结构化数据机制。
