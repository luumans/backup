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

# parseUrl URL参数
``` javascript
function parseUrl() {
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
  return _ups;
}
var ups = parseUrl();
```
正常网址获取参数
``` javascript
function getFlag(name) {
  var reg = new RegExp("(^|&)" + name + "=([^&]*)(&|$)");
  var url = window.location.search.substr(1).match(reg);
  if (url != null) {
    return unescape(url[2]);
  } else {
    return null;
  }
};
```

Vue网址获取参数
``` javascript
function getFlag(name) {
  var reg = new RegExp("(^|&)" + name + "=([^&]*)(&|$)");
  var url = window.location.href.split('?')[1].match(reg);
  if (url != null) {
    return unescape(url[2]);
  } else {
    return null;
  }
};
```

# parseUA 获取UA
``` javascript
function parseUA() {
  var u = navigator.userAgent;
  var u2 = navigator.userAgent.toLowerCase();
  return {
    trident: u.indexOf('Trident') > -1, // IE内核
    presto: u.indexOf('Presto') > -1, // opera内核
    webKit: u.indexOf('AppleWebKit') > -1, // 苹果、谷歌内核
    gecko: u.indexOf('Gecko') > -1 && u.indexOf('KHTML') == -1, // 火狐内核
    mobile: !!u.match(/AppleWebKit.*Mobile.*/), // 是否为移动终端
    ios: !!u.match(/\(i[^;]+;( U;)? CPU.+Mac OS X/), // ios终端
    android: u.indexOf('Android') > -1 || u.indexOf('Linux') > -1, // android终端或uc浏览器
    iPhone: u.indexOf('iPhone') > -1, // 是否为iPhone或者QQHD浏览器
    iPad: u.indexOf('iPad') > -1, // 是否iPad
    webApp: u.indexOf('Safari') == -1, // 是否web应该程序，没有头部与底部
    iosv: u.substr(u.indexOf('iPhone OS') + 9, 3),
    weixin: u2.match(/MicroMessenger/i) == "micromessenger",
    ali: u.indexOf('AliApp') > -1,
    yk: u.indexOf('AliApp') > -1,
  };
}
var ua = parseUA();
```

# 立即执行函数表达式（IIFE）
``` javascript
// IIFE 写法
(function () {
  var tmp = ...;
  ...
}());
```

# 右击
oncontextmenu 在用户使用鼠标右键单击客户区打开上下文菜单时触发。 MouseEvent

## 禁用右键
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

## 修改右键菜单
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

# 事件禁用网页上选取的内容
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

# 事件禁用复制
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

# 禁用鼠标事件
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

# 禁用键盘中的ctrl、alt、shift

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

# 禁止网页另存为
在<body>后面加入以下代码
``` javascript
<noscript>
  <iframe src="*.htm"></iframe>
</noscript>
```







# 页转
``` javascript
```