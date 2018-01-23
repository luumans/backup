title: 原生JS技巧
date: 2018-02-27 18:29:00
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

　　**自用笔记：**本文属于自用笔记，不做详解，仅供参考。在此记录自己已理解并开始遵循的前端代码规范。What How Why

<!-- more -->

# 字符串截取

```
sum = 'localhost:3000/#page-4'
var sum = sum.substring(sum.indexOf('_') + 1, sum.length) - 0
=> 4
```

# URL参数获取
``` javascript
function urlParams(name) {
  //获取url中"?"符后的字串
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
```

``` javascript
function getParam(name) {
  var reg = new RegExp('(^|&)' + name + '=([^&]*)(&|$)', 'i');
  var param = window.location.href.split('?')[1].match(reg);
  if (param != null) {
    return decodeURI(param[2]) || null;
  }
}
getParam('push');

function GetUrlParam(paraName) {
	var url = document.location.toString();
	var arrObj = url.split("?");
	if (arrObj.length > 1) {
		var arrPara = arrObj[1].split("&");
		var arr;
		for (var i = 0; i < arrPara.length; i++) {
			arr = arrPara[i].split("=");
			if (arr != null && arr[0] == paraName) {
				return arr[1];
			}
		}
		return "";
	} else {
		return "";
	}
}
GetUrlParam('push');
```

## 浏览器编码
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

# API
## Notification
Notification API 是 HTML5 新增的桌面通知 API，用于向用户显示通知信息。该通知是脱离浏览器的，即使用户没有停留在当前标签页，甚至最小化了浏览器，该通知信息也一样会置顶显示出来。

### 用户权限
想要向用户显示通知消息，需要获取用户权限，而相同的域名只需要获取一次权限。只有用户允许的权限下，Notification 才能起到作用，避免某些网站的广告滥用 Notification 或其它给用户造成影响。那么如何知道用户到底是允不允许的？

Notification.permission 表明当前通知显示的授权状态

1. default ：不知道用户的选择，默认。
1. granted ：用户允许。
1. denied ：用户拒绝。

``` javascript
if(Notification.permission === 'granted'){
  console.log('用户允许通知');
}else if(Notification.permission === 'denied'){
  console.log('用户拒绝通知');
}else{
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

## 
