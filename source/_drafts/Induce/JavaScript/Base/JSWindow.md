title: 原生JS技巧
date: 2018-08-08 18:29:00
description: 
categories:
- JavaScript
tags:
- Tool
toc: true
author:
comments:
original:
permalink: 
---
　　**自用笔记：**本文属于自用笔记，不做详解，仅供参考。在此记录自己已理解并开始遵循的前端代码规范。What How Why
<!-- more -->
# 信息API
## 获取浏览器参数

### URL参数获取
Split 拆分参数，循环遍历
``` javascript
function urlParams(name) {
  var url = window.location.href;
  var theRequest = {};
  if (url.indexOf("?") != -1) {
	  strs = url.split('?')[1].split("&");
	  for(var i = 0; i < strs.length; i++) {
	    theRequest[strs[i].split('=')[0]] = decodeURI(strs[i].split('=')[1]);
	  }
  }
  return theRequest[name] || theRequest;
}

urlParams('push');
// 数值
urlParams();
// 对象
```

### 获取URL某个参数的值
正则匹配，获取指定参数

注意：
正常网址获取参数
	var url = window.location.search.substr(1).match(reg);

Vue网址获取参数
	var url = window.location.href.split('?')[1].match(reg);

``` javascript
function getParam(name) {
  var reg = new RegExp('(^|&)' + name + '=([^&]*)(&|$)', 'i');
  var param = window.location.href.split('?')[1].match(reg);
  if (param != null) {
    return decodeURI(param[2]) || null;
  }
}
getParam('push');
```
### parseUrl URL参数 ***
截取参数，删除钩子，提取参数
``` javascript
function parseUrl(name) {
  var _ups = {};
  var _n1 = window.location.href.indexOf("?");
  if (_n1 != -1) {
    var _hash = window.location.href.substr(_n1 + 1);
    var _n2 = _hash.indexOf("#");
    if (_n2 != -1)
      _hash = _hash.substr(0, _n2);
    var _a = _hash.split("&");
    var _len = _a.length;
    for (var i = 0; i < _len; i++) {
      var _a2 = _a[i].split("=");
      _ups[_a2[0]] = _a2[1];
    }
  }
  return _ups[name] || _ups;
}
var ups = parseUrl();
```

### pathname
获取当前相对路径的方法

``` javascript
function GetUrlRelativePath() {
	var url = document.location.toString();
	var arrUrl = url.split('//');
	var start = arrUrl[1].indexOf('/');
	var relUrl = arrUrl[1].substring(start);
	// stop省略，截取从start开始到结尾的所有字符
	if (relUrl.indexOf('?') != -1) {
		relUrl = relUrl.split('?')[0];
	}
	return relUrl;
}

GetUrlRelativePath();
```

名称 | 描述
-------|------
assign() | 加载新的文档。
reload() | 重新加载当前文档。
replace() | 用新的文档替换当前文档。

``` javascript
window.location.assign("http://www.w3school.com.cn")

window.location.reload()

window.location.replace("http://www.w3school.com.cn")
与location.assign()的区别是，location.replace()跳转后的页面不会保存在浏览器历史中，即无法通过返回按钮返回到该页面。
```

### 浏览器编码
URL只能使用英文字母、阿拉伯数字和某些标点符号，不能使用其他文字和符号。
1.escape(): 会将所有的标点，空格，特殊字符以及其他非ASCII字符转化成`十六进制`转义序列，类似于%**的格式。返回一个字符的Unicode编码值。
2.encodeURI(): 不会对某些标点符号进行转码。亦不会对ASCII字母数字编码。不会进行转义的：;/?:@&=+$,#。
3.encodeURIComponent(): 不会对某些标点符号进行转码。亦不会对ASCII字母数字编码。能编码"; / ? : @ & = + $ , #"这些特殊字符。

encodeURI()和encodeURIComponent()函数的区别在于后者会假定其参数string是URI的一部分。
像@#$&=+;:/?,这些字符，encodeURI()是不会转义的，而encodeURIComponent()会将其转义成十六进制序列。

``` javascript
escape("春节")
"%u6625%u8282"
unescape("%u6625%u8282")
"春节"
encodeURI("春节")
"%E6%98%A5%E8%8A%82"
decodeURI("%E6%98%A5%E8%8A%82")
"春节"
encodeURIComponent("春节")
"%E6%98%A5%E8%8A%82"
decodeURIComponent("%E6%98%A5%E8%8A%82")
"春节"

escape("http://www.haorooms.com/My first/")
"http%3A//www.haorooms.com/My%20first/"
unescape("http%3A//www.haorooms.com/My%20first/")
'http://www.haorooms.com/My first/'
encodeURI('http://www.haorooms.com/My first/')
"http://www.haorooms.com/My%20first/"
decodeURI("http://www.haorooms.com/My%20first/")
"http://www.haorooms.com/My first/"
encodeURIComponent('http://www.haorooms.com/My first/')
"http%3A%2F%2Fwww.haorooms.com%2FMy%20first%2F"
decodeURIComponent("http%3A%2F%2Fwww.haorooms.com%2FMy%20first%2F")
"http://www.haorooms.com/My first/"
```

## parseUA 获取UA ***
Navigator 对象包含有关浏览器的信息

``` javascript
function parseUA() {
  var u = navigator.userAgent;
  var u2 = navigator.userAgent.toLowerCase();
  return {
    // IE内核
    trident: u.indexOf('Trident') > -1,
    // opera内核
    presto: u.indexOf('Presto') > -1,
    // 苹果、谷歌内核
    webKit: u.indexOf('AppleWebKit') > -1,
    // 火狐内核
    gecko: u.indexOf('Gecko') > -1 && u.indexOf('KHTML') == -1,
    // 是否为移动终端
    mobile: !!u.match(/AppleWebKit.*Mobile.*/),
    // ios终端
    ios: !!u.match(/\(i[^;]+;( U;)? CPU.+Mac OS X/),
    // android终端或uc浏览器
    android: u.indexOf('Android') > -1 || u.indexOf('Linux') > -1,
    // 是否为iPhone或者QQHD浏览器
    iPhone: u.indexOf('iPhone') > -1,
    // 是否iPad
    iPad: u.indexOf('iPad') > -1,
    // 是否web应该程序，没有头部与底部
    webApp: u.indexOf('Safari') == -1,
    // 是否IOS版本
    iosv: u.substr(u.indexOf('iPhone OS') + 9, 3),
    // 是否WeChat
    weixin: u2.match(/MicroMessenger/i) == "micromessenger",
    // 是否阿里
    ali: u.indexOf('AliApp') > -1,
    // 是否盈科
    yk: u.indexOf('ykour') > -1,
  };
}
var ua = parseUA();
```

## 立即执行函数表达式（IIFE）
``` javascript
// IIFE 写法
(function () {
  var tmp = ...;
  ...
}());
```

## 字符串截取

```
sum = 'localhost:3000/#page-4'
var sum = sum.substring(sum.indexOf('_') + 1, sum.length) - 0
=> 4
```




## location
Location 对象包含有关当前 URL 的信息。Location 对象是 Window 对象的一个部分，可通过 window.location 属性来访问。

名称 | 描述
-------|------
hash | 设置或返回从井号 (#) 开始的 URL（锚）。
host | 设置或返回主机名和当前UR的端口号。
hostname | 设置或返回当前 URL 的主机名。
href | 设置或返回完整的 URL。
pathname | 设置或返回当前 URL 的路径部分。
port | 设置或返回当前 URL 的端口号。
protocol | 设置或返回当前 URL 的协议。
search | 设置或返回从问号 (?) 开始的 URL（查询部分）。

``` javascript
http://www.baidu.com:4000/2018/02/27/Induce/JavaScript/Base/2.2.3-DomLifeCycle/?push=123#dfjdkf

hash: "#dfjdkf"
host: "www.baidu.com:4000"
hostname: "www.baidu.com"
href: "http://www.baidu.com:4000/2018/02/27/Induce/JavaScript/Base/2.2.3-DomLifeCycle/?push=123#dfjdkf"
origin: "http://www.baidu.com:4000"
pathname: "/2018/02/27/Induce/JavaScript/Base/2.2.3-DomLifeCycle/"
port: "4000"
protocol: "http:"
search: "?push=123"

location.toString() => location.href
```

vue问题：
vue默认的路由使用的hash进行路由判断，实际访问的还是同一个页面，这样就会使得search搜索的是#号及其之后的？号及其的内容。
``` javascript
http://www.baidu.com/2018/02/27/Induce/#/JavaScript/Base/2.2.3-DomLifeCycle/?push=123
location.hash
"#/JavaScript/Base/2.2.3-DomLifeCycle/?push=123"
location.search
""
```






# 操作API
## 右击
oncontextmenu 在用户使用鼠标右键单击客户区打开上下文菜单时触发。 MouseEvent

### 禁用右键
``` javascript
document.oncontextmenu = function(){
  event.returnValue = false;
}

// 或者直接返回整个事件
document.oncontextmenu = function(){
  return false;
}

<body oncontextmenu = "return false" ></body>
```

### 修改右键菜单
``` javascript
document.oncontextmenu = function(e) {  
  awesomeMenu(e);
}
function awesomeMenu(e) {
  var x = e.clientX;
  var y = e.clientY;
  // 获取到鼠标位置后就可以自定义菜单了
}
```

## 事件禁用网页上选取的内容
onselectstart 页面开始选择事件 Event

``` javascript
document.onselectstart = function(){
  event.returnValue = false;
}
// 或者直接返回整个事件
document.onselectstart = function(){
  return false;
}

<body onselectstart = "return false" ></body>
```

## 事件禁用复制
oncopy 当用户复制对象或选中区，将其添加到系统剪贴板上时在源元素上触发。 ClipboardEvent
<!-- ondragstart 事件在用户开始拖动元素或选择的文本时触发。 -->
onbeforecopy 当选中区复制到系统剪贴板之前在源对象触发。

``` javascript
document.oncopy = function(){
  event.returnValue = false;
}

// 或者直接返回整个事件
document.oncopy = function(){
  return false;
}

<body oncopy = "return false" ></body>
```

## 禁用鼠标事件
onmousedown 按下鼠标时触发此事件
``` javascript
document.onmousedown = function(e){
  if (e.which == 2) {
    // 鼠标滚轮的按下，滚动不触发
    return false;
  }
  if (e.which == 3) {
    // 鼠标右键
    return false;
  }
}
```

## 禁用键盘中的ctrl、alt、shift

``` javascript
document.onkeydown = function(){
  if (event.ctrlKey) {
    return false;
  }
  if (event.altKey) {
    return false;
  }
  if (event.shiftKey) {
    return false;
  }
}
```

## 禁止网页另存为
在<body>后面加入以下代码
``` javascript
<noscript>
  <iframe src="*.htm"></iframe>
</noscript>
```




# 通知API
## Notification
Notification API 是 HTML5 新增的桌面通知 API，用于向用户显示通知信息。该通知是脱离浏览器的，即使用户没有停留在当前标签页，甚至最小化了浏览器，该通知信息也一样会置顶显示出来。

### 用户权限
想要向用户显示通知消息，需要获取用户权限，而相同的域名只需要获取一次权限。只有用户允许的权限下，Notification 才能起到作用，避免某些网站的广告滥用 Notification 或其它给用户造成影响。那么如何知道用户到底是允不允许的？

Notification.permission 表明当前通知显示的授权状态

1. default ：不知道用户的选择，默认。
1. granted ：用户允许。
1. denied ：用户拒绝。

``` javascript
if (Notification.permission === 'granted') {
  console.log('用户允许通知');
} else if (Notification.permission === 'denied') {
  console.log('用户拒绝通知');
} else {
  console.log('用户还没选择，去向用户申请权限吧');
}
```

### 请求权限
当用户还没选择的时候，我们需要向用户去请求权限。Notification 对象提供了 requestPermission() 方法请求用户当前来源的权限以显示通知。

``` javascript
Notification.requestPermission().then(function(permission) {
  if (permission === 'granted') {
    console.log('用户允许通知')
  } else if (permission === 'denied') {
    console.log('用户拒绝通知')
  }
})
```

### 推送通知
获取用户授权之后，就可以推送通知了。
``` javascript
var notification = new Notification(title, options)
```
1. title：通知的标题
1. options：通知的设置选项（可选）。
	1. body：通知的内容。
	1. tag：代表通知的一个识别标签，相同tag时只会打开同一个通知窗口。
	1. icon：要在通知中显示的图标的URL。
	1. image：要在通知中显示的图像的URL。
	1. data：想要和通知关联的任务类型的数据。
	1. requireInteraction：通知保持有效不自动关闭，默认为false。

``` javascript
var n = new Notification('状态更新提醒',{
  body: '你的朋友圈有3条新状态，快去查看吧',
  tag: 'linxin',
  icon: 'http://blog.gdfengshuo.com/images/avatar.jpg',
  requireInteraction: true
})
```

### 关闭通知
从上面的参数可以看出，并没有一个参数用来配置显示时长的。我想要它 3s 后自动关闭的话，这时可以调用 close() 方法来关闭通知。

``` javascript
var n = new Notification('状态更新提醒', {
  body: '你的朋友圈有3条新状态，快去查看吧'
})
setTimeout(function() {
  n.close()
}, 3000);
```

### 事件
var n = new Notification('状态更新提醒', {
  body: '你的朋友圈有3条新状态，快去查看吧',
  data: {
    url: 'http://blog.gdfengshuo.com'
  }
})
n.onclick = function() {
  // 打开网址
  window.open(n.data.url, '_blank')
  // 并且关闭通知
  n.close()
}

### 关闭页面
不过它在关闭标签卡的时候，通知也会被关闭，那是因为监听了页面 beforeunload 事件。

``` javascript
function addOnBeforeUnload(e) {
	FERD_NavNotice.notification.close()
}
if (window.attachEvent) {
	window.attachEvent('onbeforeunload', addOnBeforeUnload)
} else {
	window.addEventListener('beforeunload', addOnBeforeUnload, false)
}

// attachEvent——兼容：IE7、IE8；不兼容firefox、chrome、IE9、IE10、IE11、safari、opera
// addEventListener——兼容：firefox、chrome、IE、safari、opera；不兼容IE7、IE8
```

### 兼容性
1. PC端的还好大多都能支持，除了IE
1. 移动端的几乎全倒

[HTML5 桌面通知：Notification API](https://segmentfault.com/a/1190000011670082 "")
[React Notification](https://github.com/react-component/notification "")
[Vue notification](https://github.com/vue-bulma/notification "Notification component for Vue Bulma")
[notification](https://github.com/rentiansheng/notification "浏览器桌面通知")


# 时间处理
## 转换为时间间隔

``` javascript
function formatTimeRead (time) {
  let date = (typeof time === 'number') ? new Date(time) : new Date((time || '').replace(/-/g, '/'))
  let diff = (((new Date()).getTime() - date.getTime()) / 1000)
  console.log((new Date()).getTime() - date.getTime())
  console.log(diff)
  let dayDiff = Math.floor(diff / 86400)

  let isValidDate = Object.prototype.toString.call(date) === '[object Date]' && !isNaN(date.getTime())

  if (!isValidDate) {
    console.error('not a valid date')
  }
  const formatDate = function (date) {
    let today = new Date(date)
    let year = today.getFullYear()
    let month = ('0' + (today.getMonth() + 1)).slice(-2)
    let day = ('0' + today.getDate()).slice(-2)
    let hour = today.getHours()
    let minute = today.getMinutes()
    let second = today.getSeconds()
    return `${year}-${month}-${day} ${hour}:${minute}:${second}`
  }

  if (isNaN(dayDiff) || dayDiff < 0 || dayDiff >= 31) {
    return formatDate(date)
  }

  return dayDiff === 0 && (
      diff < 60 && '刚刚' ||
      diff < 120 && '1分钟前' ||
      diff < 3600 && Math.floor(diff / 60) + '分钟前' ||
      diff < 7200 && '1小时前' ||
      diff < 86400 && Math.floor(diff / 3600) + '小时前') ||
    dayDiff === 1 && '昨天' ||
    dayDiff < 7 && dayDiff + '天前' ||
    dayDiff < 31 && Math.ceil(dayDiff / 7) + '周前'
}

// http://www.cnblogs.com/zhangpengshou/archive/2012/07/19/2599053.html
// [1, 'Just Now', 'Just Now'],
// [2, '1 Second Ago', '1 Second From Now'],
// [60, 'Seconds', 1],                         // 60
// [120, '1 Minute Ago', '1 Minute From Now'], // 60*2
// [3600, 'Minutes', 60],                      // 60*60, 60
// [7200, '1 Hour Ago', '1 Hour From Now'],    // 60*60*2
// [86400, 'Hours', 3600],                     // 60*60*24, 60*60
// [172800, 'Yesterday', 'Tomorrow'],          // 60*60*24*2
// [604800, 'Days', 86400],                    // 60*60*24*7, 60*60*24
// [1209600, 'Last Week', 'Next Week'],        // 60*60*24*7*4*2
// [2628000, 'Weeks', 604800],                 // 30.416 days
// [5256000, 'Last Month', 'Next Month'],      // 60.832 days
// [31557600, 'Months', 2628000],              // 365.25 days
// [63115200, 'Last Year', 'Next Year']
```

# 页转
``` javascript
```


## 
