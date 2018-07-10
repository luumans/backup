title: add(1)(2)(3)...
date: 2018-02-27 19:29:00
description: 
categories:
- interview
tags:
- Life
toc: true
author:
comments:
original:
permalink: 
---
相信三月份不少同学拿了年终奖之后都在准备跳槽或者正在跳槽，如果你最近有重要的前端面试，不妨看看我们这套押题。这套题适合 15k 以下工资的前端面试。之后我们可能会出 15k 以上工资的题目，敬请关注。
<!-- more -->

[2018前端面试押题（附反馈）](https://mp.weixin.qq.com/s?__biz=MzA3NjA1MDU4NA==&mid=502653079&idx=1&sn=7e0af361cd82530430036def1f133db6&chksm=076679603011f0769f630d206fb2c56e57190b2716c3a1204e91fab8142095a992d399b35c4d#rd "")

# 面试题
## HTML 押题
### （必考） 你是如何理解 HTML 语义化的？
HTML语义化是为了方便搜索引擎的爬虫机器人，更好的去理解。
[HTML 5的革新——语义化标签(一)](http://www.html5jscss.com/html5-semantics-section.html "")

优点：
1. 有利于SEO：和搜索引擎建立良好沟通，有助于爬虫抓取更多的有效信息，爬虫依赖于标签来确定上下文和各个关键字的权重
1. 方便其他设备解析（如屏幕阅读器、盲人阅读器、移动设备）以意义的方式来渲染网页
1. 便于团队开发和维护，语义化更具可读性，是下一步吧网页的重要动向，遵循W3C标准的团队都遵循这个标准，可以减少差异化
1. 为了在没有CSS的情况下，页面也能呈现出很好地内容结构、代码结构

HTML注意事项：
1. 尽可能少的使用无语义的标签div和span
1. 不要使用纯样式标签，如：b、font、u等，改用css设置
1. 需要强调的文本，可以包含在strong或者em标签中（浏览器预设样式，能用CSS指定就不用他们），strong默认样式是加粗（不要用b），em是斜体（不用i）
1. 表格：标题要用caption，表头用thead，主体部分用tbody包围，尾部用tfoot包围。表头和一般单元格要区分开，表头标题用th，内容单元格用td；

``` javascript
<table border="1">
	<thead>
	  <tr>
	    <th>Month</th>
	    <th>Savings</th>
	  </tr>
  </thead>
  <tbody>
    <td>January</td>
    <td>$100</td>
  </tbody>
  <tfoot>
  	<td>$January</td>
  	<td>$100</td>
  </tfoot>
</table>
```

1. input: 每个input标签对应的说明文本都需要使用label标签，并且通过为input设置id属性，在lable标签中设置for=someld来让说明文本和相对应的input关联起来。

``` javascript
<div class="input">
	<label class="label-line" for="name">登录</label>
	<input class="input-text" type="text" value="" name="name"></span>
</div>
```

1. 表单域要用fieldset标签包起来，并用legend标签说明表单的用途

``` javascript
<form>
  <fieldset>
    <legend>Personalia:</legend>
    Name: <input type="text"><br>
    Email: <input type="text"><br>
    Date of birth: <input type="text">
  </fieldset>
</form>
```

### meta viewport 是做什么用的，怎么写？
Viewport视窗

一个常用的针对移动网页优化过的页面的 viewport meta 标签大致如下： 
``` javascript
<meta name=”viewport” content=”width=device-width, initial-scale=1, maximum-scale=1″> 
```

width：控制 viewport 的大小，可以指定的一个值，如果 600，或者特殊的值，如 device-width 为设备的宽度（单位为缩放为 100% 时的 CSS 的像素）。 
height：和 width 相对应，指定高度。 
initial-scale：初始缩放比例，也即是当页面第一次 load 的时候缩放比例。 
maximum-scale：允许用户缩放到的最大比例。 
minimum-scale：允许用户缩放到的最小比例。 
user-scalable：用户是否可以手动缩放

### canvas 元素是干什么的？
图形容器，您必须使用脚本来绘制图形

``` javascript
```

## CSS 押题
### （必考） 说说盒模型。
一个盒子包括了content（实际内容width,height控制）、border（边框）、padding（内边距）和margin（外边距）

ie盒子模型
content的宽度和高度包括了border和padding。

box-sizing
1. content-box:使元素遵循标准 w3c 盒子模型（默认值）。
1. border-box:使元素遵循ie 盒子模型。
1. inherit： 规定应从父元素继承 box-sizing 属性的值。

### css reset 和 normalize.css 有什么区别？
Reset 相对「暴力」，不管你有没有用，统统重置成一样的效果，且影响的范围很大，讲求跨浏览器的一致性。相反把浏览器的默认样式都重置了
Normalize 相对「平和」，注重通用的方案，重置掉该重置的样式，保留有用的 user agent 样式，同时进行一些 bug 的修复，这点是 reset 所缺乏的。保留浏览器的原来样式并且做到每个浏览显示一致。 
[来，让我们谈一谈 Normalize.css](http://jerryzou.com/posts/aboutNormalizeCss/ "")
[](http://blog.teamtreehouse.com/applying-normalize-css-reset-quick-tip "")
### （必考）如何居中？

[用CSS实现元素垂直居中方案]( "")
### 选择器优先级如何确定？
!important > 行内样式 > ID选择器 > 类选择器 > 标签 > 通配符 > 继承 > 浏览器默认属性
[CSS三大特性—— 继承、 优先级和层叠。](https://www.cnblogs.com/zxjwlh/p/6213239.html "")

### BFC 是什么？
Formatting context(DOM格式化上下文)
Block Formatting Contexts (块级格式化上下文)，它属于上述定位方案的普通流。

1. body 根元素
1. 浮动元素：float 除 none 以外的值
1. 绝对定位元素：position (absolute、fixed)
1. display 为 inline-block、table-cells、flex
1. overflow 除了 visible 以外的值 (hidden、auto、scroll)

> BFC 特性及应用

1.  同一个 BFC 下外边距会发生折叠
margin的值两者折叠
``` javascript
<head>
div{
  width: 100px;
  height: 100px;
  background: lightblue;
  margin: 100px;
}
</head>
<body>
  <div></div>
  <div></div>
</body>
```

放在两个不同的BFC中即可
``` javascript
<div class="container">
  <p></p>
</div>
<div class="container">
  <p></p>
</div>
.container {
  overflow: hidden;
}
p {
  width: 100px;
  height: 100px;
  background: lightblue;
  margin: 100px;
}
```

2. BFC 可以包含浮动的元素（清除浮动）
3. BFC 可以阻止元素被浮动元素覆盖
文字环绕效果

### 如何清除浮动？
浮动会导致父元素高度坍塌
> clear清除浮动

``` javascript
// 现代浏览器clearfix方案，不支持IE6/7
.clearfix:after {
  display: table;
  content: " ";
  clear: both;
}

// 全浏览器通用的clearfix方案
// 引入了zoom以支持IE6/7
.clearfix:after {
  display: table;
  content: " ";
  clear: both;
}
.clearfix{
  *zoom: 1;
}

// 全浏览器通用的clearfix方案【推荐】
// 引入了zoom以支持IE6/7
// 同时加入:before以解决现代浏览器上边距折叠的问题
.clearfix:before,
.clearfix:after {
  display: table;
  content: " ";
}
.clearfix:after {
  clear: both;
}
.clearfix{
  *zoom: 1;
}
```
> BFC清除浮动

1. float 为 left | right
1. overflow 为 hidden | auto | scorll
1. display 为 table-cell | table-caption | inline-block | flex | inline-flex
1. position 为 absolute | fixed

IE6/7不支持BFC，也不支持:after，所以IE6/7清除浮动要靠触发hasLayout，了解下就行，毕竟IE6/7已经是历史的产物了。

### 如何让弹窗自定义文字长度，超出自动换行，在不设置宽度的情况下

``` javascript
position: fixed;
background: #000;
z-index: 99999;
left: 50%;
display: table;
top: 50%;
transform: translate(-50%,-50%);
padding: 10px;
```

## JS 押题
### JS有哪些数据类型？
1. 基本数据类型
Undefined、Null、Boolean、Number、String

2. 复杂数据类型
Object、Array和Function则属于引用类型

### （必考） Promise 怎么使用？

``` javascript
var handler = new Promise(function (resolve, reject) {
  ... // 逻辑代码
}); 

handler.then(function (val) {
  ... // 肯定承诺的回调
}, function (e) {
  ... // 否定承诺的回调
});
```

### 优雅降级与渐进增强
``` javascript
.transition {   /*渐进增强写法*/
  -webkit-transition: all .5s;
     -moz-transition: all .5s;
       -o-transition: all .5s;
          transition: all .5s;  
} 
.transition {   /*优雅降级写法*/ 
          transition: all .5s;
       -o-transition: all .5s;
     -moz-transition: all .5s;
  -webkit-transition: all .5s;
}
```

### （必考） AJAX 手写一下？

``` javascript
function ajax(url, fnS, fnE) {
	if (window.XMLHttpRequest) {
		var Ajax = new XMLHttpRequest()
	} else {
		var Ajax = new ActiveXObject('Microsoft.XMLHTTP')
	}
	Ajax.opan('GET', 'url?=t' + new Date().getTime(), true)
	Ajax.send()
	Ajax.onreadystatechange = function () {
		if (Ajax.readyState == 4) {
			if (Ajax.status == 200) {
				fnS(Ajax.responseText)
			} else {
				if (fnE) {
					fnE(Ajax.status)
				}
			}
		}
	}
}
ajax('a.txt', function(res){
	alert(res)
})
```

### （必考）闭包是什么？
闭包就是由函数创造的一个词法作用域，里面创建的变量被引用后，可以在这个词法环境之外自由使用。

### （必考）这段代码里的 this 是什么？
### （必考）什么是立即执行函数？使用立即执行函数的目的是什么？
声明一个匿名函数，马上调用这个匿名函数
首先声明一个匿名函数 function(){alert('我是匿名函数')}。
然后在匿名函数后面接一对括号 ()，调用这个匿名函数。
(function(){alert('我是匿名函数')})()

只有一个作用：创建一个独立的作用域。
这个作用域里面的变量，外面访问不到（即避免「变量污染」）。
### async/await 语法了解吗？目的是什么？
async 异步 await 同步
### 如何实现深拷贝？
### 如何实现数组去重？1
``` javascript
var a = [1, 2, 3]
var b = [1, 4, 2]
var c = a.concat(b)
var list = {}
var resule = []
c.forEach((item) => {
	if (!list[item]) {
		resule.push(item)
		list[item] = true
	}
})
console.log(resule)
```

``` javascript
var arr = ['abc', 'abcd', 'sss', '2', 'd', 't', '2', 'ss', 'f', '22', 'd']
var s = []
arr.forEach((item) => {
	if (s.indexOf(item) == -1) {
		s.push(item)
	}
})
console.log(s)
```

### 如何用正则实现 string.trim() ？1
``` javascript
var str = '  jkdfdkf j j '
console.log(str.trim())

String.prototype.trim = function() {
  return this.replace(/(^\s*)|(\s*$)/g, '')
}
```
```javascript
```

### JS闭包
有权访问另一个函数作用域中的变量的函数

```javascript
function fn() {
	var a = 100
	return function () {
		console.log(a)
	}
}
var func = fn()
func()
//100
```

### JS 原型是什么？1
原型: 在JavaScript中原型是一个prototype对象，用于表示类型之间的关系。
原型链: JavaScript万物都是对象，对象和对象之间也有关系，并不是孤立存在的。对象之间的继承关系，在JavaScript中是通过prototype对象指向父类对象，直到指向Object对象为止，这样就形成了一个原型指向的链条，专业术语称之为原型链。

### JS实现拂霓裳Cookie()，实现cookie的读写，并且可以传入expires和path

### JS的继承方式

### JS跨域的方式

### 浏览器缓存机制

### HTTP协议、TCP协议处于什么层，之间的关系

### 浏览器渲染页面的流程

### CSRF防范

### 性能优化的规则
1. 图片懒加载
1. 代码压缩
1. 减少网络请求
1. CDN
1. Iconfont
1. 减少对DOM的操作

### socket的了解

### 网络安全，如何预防XSS和CSRF

### TCP建立连接的过程

# 类
工厂模式
密名函数
构造函数

### ES 6 中的 class 了解吗？1
### JS 如何实现继承？1
1. 使用原型prototype
1. 对象冒充

### == 相关题目直接反着答（放弃）1
### call 、apply 和 bind 的用法和区别是什么？
1. call的arg传参需一个一个传，apply则直接传一个数组。
1. call和apply直接执行函数，而bind需要再一次调用。

## DOM 押题
### DOM 事件模型是什么？
捕获 冒泡

### 移动端的触摸事件了解吗？
touch类事件
touchstart:手指触摸到屏幕会触发
touchmove:当手指在屏幕上移动时,会触发
touchend:当手指离开屏幕时,会触发
touchcancel:可由系统进行的触发

### 事件监听
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
			event.cancelBubble = true
		}
	}
}

### 事件委托是什么？有什么好处？
1. 提高性能
1. 新添加的元素还会有之前的事件

## HTTP 押题
### HTTP 状态码知道哪些？
304   （未修改）
403   （禁止） 服务器拒绝请求。
404   （未找到） 服务器找不到请求的网页。
### 301 和 302 的区别是什么？
301是永久重定向，而302是临时重定向

### HTTP 缓存怎么做？
### Cache-Control 和 Etag 的区别是什么？
缓存配置
被请求变量的实体值

### Cookie 是什么？Session 是什么？
存在客户端的用来保存一些用户信息
在服务器端生成的针对当前会话的存储结构，特点是只针对当前会话用户，且相对安全，缺点是连接一多极耗内存

### LocalStorage 和 Cookie 的区别是什么？
## 存储时效
1. cookie可以手动设置失效期，默认为会话级
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


### （必考）GET 和 POST 的区别是什么？
get用于获取数据，post用于提交数据。
get参数有长度限制（受限于url长度，具体的数值取决于浏览器和服务器的限制），而post无限制
GET是通过URL方式请求，可以直接看到，明文传输。
GET时默认可以复用前面的请求数据作为缓存结果返回，此时以完整的URL作为缓存数据的KEY。

### （必考）怎么跨域？JSONP 是什么？CORS 是什么？postMessage 是什么？

JSONP 填充式JSON
利用了使用src引用静态资源时不受跨域限制的机制。主要在客户端搞一个回调做一些数据接收与操作的处理，并把这个回调函数名告知服务端，而服务端需要做的是按照javascript的语法把数据放到约定好的回调函数之中即可。
CORS 跨域资源共享
通过添加HTTP Hearder部分字段请求与获取有权限访问的资源。CORS对开发者是透明的，因为浏览器会自动根据请求的情况（简单和复杂）做出不同的处理。CORS的关键是服务端的配置支持。

## Vue 押题
### （必考）Vue 有哪些生命周期钩子函数？
beforeCreate 组件实例刚创建，组件属性计算前
created 组件实例创建完成，属性已绑定，DOM未生成，$el不存在
beforeMount 挂载之前
mounted 挂载之后
beforeUpdate 组件更新之前
updated 组件更新之后
beforeDestroy 组件销毁前
destroy 组件销毁之后
activated keepalive激活
deactivated keepalive移除

[Vue2.0 探索之路——生命周期和钩子函数的一些理解](https://segmentfault.com/a/1190000008010666 "")

### （必考）Vue 如何实现组件通信？
1. $emit 触发
1. $on 监听
1. 数据绑定

### Vuex 的作用是什么？
1. 状态管理模式
1. 组件之间的数据通信
1. 使用单向数据流的方式进行数据的中心化管理

### VueRouter 路由是什么？
页面展示变化的页面管理

### Vue 的双向绑定是如何实现的？有什么缺点？
### Computed 计算属性的用法？跟 Methods 的区别。
用于实时计算数据变化后的结果

1. 数据变化就会执行，后者事件触发执行
1. computed必须返回一个值页面绑定的才能取得值，而methods中可以只执行逻辑代码，可以有返回值，也可以没有。

## 算法押题
### 排序算法（背诵冒泡排序、选择排序、计数排序、快速排序、插入排序、归并排序）
[常用算法js版](https://www.cnblogs.com/ytb-wpq/p/6479240.html "")

#### 冒泡排序
双循环，数值对调，前大后小
``` javascript
function bubbleSort(arr) {
  var len = arr.length
  for (var i = 0; i < len - 1; i++) {
    for (var j = 0; j < len - 1 - i; j++) {
      if (arr[j] > arr[j+1]) {
        var temp = arr[j+1]
        arr[j+1] = arr[j]
        arr[j] = temp
      }
    }
  }
  return arr;
}
```

#### 选择排序
``` javascript
双循环，比我小的，数值对调
function selectionSort(arr) {
  var len = arr.length
  var minIndex, temp
  for (var i = 0; i < len - 1; i++) {
    minIndex = i;
    for (var j = i + 1; j < len; j++) {
      if (arr[j] < arr[minIndex]) {
        minIndex = j
      }
    }
    temp = arr[i];
    arr[i] = arr[minIndex];
    arr[minIndex] = temp;
  }
  return arr;
}
```

#### 插入排序
``` javascript
function insertionSort(arr) {
  var len = arr.length
  var preIndex, current
  for (var i = 1; i < len; i++) {
    preIndex = i - 1
    current = arr[i]
    while(preIndex >= 0 && arr[preIndex] > current) {
      arr[preIndex+1] = arr[preIndex]
      preIndex--
    }
    arr[preIndex+1] = current
  }
  return arr
}
```

#### 希尔排
``` javascript
function shellSort(arr) {
  var len = arr.length,
    temp,
    gap = 1;
  while(gap < len/3) {     //动态定义间隔序列
    gap =gap*3+1;
  }
  for (gap; gap > 0; gap = Math.floor(gap/3)) {
    for (var i = gap; i < len; i++) {
      temp = arr[i];
      for (var j = i-gap; j >= 0 && arr[j] > temp; j-=gap) {
        arr[j+gap] = arr[j];
      }
      arr[j+gap] = temp;
    }
  }
  return arr;
}
```

#### 归并排序
``` javascript
function mergeSort(arr) { // 采用自上而下的递归方法
  var len = arr.length
  if(len < 2) {
    return arr;
  }
  var middle = Math.floor(len / 2),
    left = arr.slice(0, middle),
    right = arr.slice(middle);
  return merge(mergeSort(left), mergeSort(right));
}

function merge(left, right)
{
  var result = [];

  while (left.length && right.length) {
    if (left[0] <= right[0]) {
      result.push(left.shift());
    } else {
      result.push(right.shift());
    }
  }

  while (left.length)
    result.push(left.shift());

  while (right.length)
    result.push(right.shift());

  return result;
}
```

#### 快速排序
``` javascript
function quickSort(arr, left, right) {
  var len = arr.length,
    partitionIndex,
    left = typeof left != 'number' ? 0 : left,
    right = typeof right != 'number' ? len - 1 : right;

  if (left < right) {
    partitionIndex = partition(arr, left, right);
    quickSort(arr, left, partitionIndex-1);
    quickSort(arr, partitionIndex+1, right);
  }
  return arr;
}

function partition(arr, left ,right) {   // 分区操作
  var pivot = left,           // 设定基准值（pivot）
    index = pivot + 1;
  for (var i = index; i <= right; i++) {
    if (arr[i] < arr[pivot]) {
      swap(arr, i, index);
      index++;
    }    
  }
  swap(arr, pivot, index - 1);
  return index-1;
}

function swap(arr, i, j) {
  var temp = arr[i];
  arr[i] = arr[j];
  arr[j] = temp;
}
functiion paritition2(arr, low, high) {
 let pivot = arr[low];
 while (low < high) {
  while (low < high && arr[high] > pivot) {
   --high;
  }
  arr[low] = arr[high];
  while (low < high && arr[low] <= pivot) {
   ++low;
  }
  arr[high] = arr[low];
 }
 arr[low] = pivot;
 return low;
}

function quickSort2(arr, low, high) {
 if (low < high) {
  let pivot = paritition2(arr, low, high);
  quickSort2(arr, low, pivot - 1);
  quickSort2(arr, pivot + 1, high);
 }
 return arr;
}
```

#### 堆排序
``` javascript
function buildMaxHeap(arr) {  // 建立大顶堆
var len;  // 因为声明的多个函数都需要数据长度，所以把len设置成为全局变量
  len = arr.length
  for (var i = Math.floor(len/2); i >= 0; i--) {
    heapify(arr, i);
  }
}

function heapify(arr, i) {   // 堆调整
  var left = 2 * i + 1,
    right = 2 * i + 2,
    largest = i;

  if (left < len && arr[left] > arr[largest]) {
    largest = left;
  }

  if (right < len && arr[right] > arr[largest]) {
    largest = right;
  }

  if (largest != i) {
    swap(arr, i, largest);
    heapify(arr, largest);
  }
}

function swap(arr, i, j) {
  var temp = arr[i];
  arr[i] = arr[j];
  arr[j] = temp;
}

function heapSort(arr) {
  buildMaxHeap(arr);

  for (var i = arr.length-1; i > 0; i--) {
    swap(arr, 0, i);
    len--;
    heapify(arr, 0);
  }
  return arr;
}
```

#### 计数排序
``` javascript
function countingSort(arr, maxValue) {
  var bucket = new Array(maxValue+1),
    sortedIndex = 0;
    arrLen = arr.length,
    bucketLen = maxValue + 1;
  for (var i = 0; i < arrLen; i++) {
    if (!bucket[arr[i]]) {
      bucket[arr[i]] = 0;
    }
    bucket[arr[i]]++;
  }
  for (var j = 0; j < bucketLen; j++) {
    while(bucket[j] > 0) {
      arr[sortedIndex++] = j;
      bucket[j]--;
    }
  }

  return arr;
}
var sorts = [23, 10000000, 31, 11, 1, 2, 4]
radixSort(sorts, 10000000)
```

#### 桶排
``` javascript
function bucketSort(arr, bucketSize) {
  if (arr.length === 0) {
   return arr;
  }

  var i;
  var minValue = arr[0];
  var maxValue = arr[0];
  for (i = 1; i < arr.length; i++) {
   if (arr[i] < minValue) {
     minValue = arr[i];        // 输入数据的最小值
   } else if (arr[i] > maxValue) {
     maxValue = arr[i];        // 输入数据的最大值
   }
  }

  //桶的初始化
  var DEFAULT_BUCKET_SIZE = 5;      // 设置桶的默认数量为5
  bucketSize = bucketSize || DEFAULT_BUCKET_SIZE;
  var bucketCount = Math.floor((maxValue - minValue) / bucketSize) + 1;  
  var buckets = new Array(bucketCount);
  for (i = 0; i < buckets.length; i++) {
    buckets[i] = [];
  }

  //利用映射函数将数据分配到各个桶中
  for (i = 0; i < arr.length; i++) {
    buckets[Math.floor((arr[i] - minValue) / bucketSize)].push(arr[i]);
  }

  arr.length = 0;
  for (i = 0; i < buckets.length; i++) {
    insertionSort(buckets[i]);           // 对每个桶进行排序，这里使用了插入排序
    for (var j = 0; j < buckets[i].length; j++) {
      arr.push(buckets[i][j]);           
    }
  }

  return arr;
}
```



改进

### 二分查找法
### 翻转二叉树

把上面三个背一下，算法题必过。

左图右文，要求左边图片垂直居中，右边文字也垂直居中？
对图片设置vertical: middel;

使用display: table;

.list-box {
  display: table;
  background: #000;
  height: 400px;
}
.list-img{
  height: 400px;
	img{
		vertical-align: middle;
	}
}
.list-img, .list-text {
  display: table-cell;
  vertical-align: middle;
}

## 安全押题
### 什么是 XSS 攻击？如何预防？
XSS：跨站脚本（Cross-site scripting，通常简称为XSS）是一种网站应用程序的安全漏洞攻击，是代码注入的一种。它允许恶意用户将代码注入到网页上，其他用户在观看网页时就会受到影响。这类攻击通常包含了HTML以及用户端脚本语言。

### 什么是 CSRF 攻击？如何预防？
CSRF:跨站请求伪造（英语：Cross-site request forgery），也被称为 one-click attack 或者 session riding，通常缩写为 CSRF 或者 XSRF， 是一种挟制用户在当前已登录的Web应用程序上执行非本意的操作的攻击方法。


## Webpack 题
### Webpack转译出的文件过大怎么办？
1. 手动引入模块，按需加载
1. 使用webpack压缩插件
1. 提取第三方库
1. 拆分打包文件

### Webpack转译速度慢什么办？
1. 排查设备性能的问题

[](https://zhuanlan.zhihu.com/p/21748318 "")

### 写过 webpack loader 吗？



## 发散题
### 从输入 URL 到页面展现中间发生了什么？
### 你没有工作经历吗？
### 你遇到过最难的问题是什么？
难题是在未解决之前叫做难题，解决之后就不叫难题

### 你的期望薪资是多少？
### （任何你不会的问题）

承认不会

询问详细细节：你问的是不是XXX方面的知识？请问你想问的是哪方面知识？

根据面试官的回答，向有利于自己的方向引导话题。



## 刁钻代码题
### [1,2,3].map(parseInt) 输出什么？
parseInt不符合规定NaN
``` javascript
[1, NaN, NaN]
```

``` javascript
Array.map((element, index, array) => {
	return parseInt(element, index, array)
})

function test(element, index, array) {
	console.log(element, index, array)
	return element
}
[1, 2, 3].map(test)
// [1, 2, 3]
```

### a = {n:1}; a.x = a = {n:2}  问 a.x 的值是什么？
``` javascript
undefined
```

``` javascript
var a = {n:1} 
var b = a  
a.x = a = {n:2} 
console.log(a.x)
console.log(a)
// undefined
console.log(b)
console.log(b.x)
// [object Object]
```

1. a和b都是指向对象A{n:1}
``` javascript
         A对象
a ===> |
       | {n:1}
b ===> |
       |
```
1. 赋值运算顺序永远都是从右往左的，“.”是优先级最高的运算符
``` javascript
         A对象
a ===> |
       | {n:1, x: undefined}
b ===> |
       |
```

1. 执行a = {n:2}
``` javascript
         A对象
a ===> | {n:2}
```

1. 由于（ .  运算符最先计算）一开始js已经先计算了a.x，便已经解析了这个a.x是对象A的x，所以在同一条公式的情况下再回来给a.x赋值，也不会说重新解析这个a.x为对象B的x。

###  (a == 1 && a == 2 && a == 3) 可能为 true 吗？

``` javascript
var a = [1, 2, 3]
a.join = a.shift
a == 1 && a == 2 && a == 3
```

``` javascript
var a = {
	i: 1,
	toString: function () {
		return a.i++
	}
}
if (a == 1 && a == 2 && a == 3) {
  console.log('Hello World!')
}
```

``` javascript
with({
  get a() {
    return Math.floor(Math.random()*4);
  }
}){
  for(var i=0;i<1000;i++){
    if (a == 1 && a == 2 && a == 3){
      console.log("after "+(i+1)+" trials, it becomes true finally!!!");
      break;
    }
  }
}
```