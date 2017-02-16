title:  requireJS 简要介绍和完整例子
date: 2017-02-06 18:29:00
description: 
categories:
- Javascript
tags:
- Module
toc: true
author: 
comments:
original:
permalink: 
---
## 模块化编程
目前，通行的Javascript模块规范共有两种：CommonJS和AMD。我主要介绍AMD，但是要先从CommonJS讲起。
### 模块
模块就是实现特定功能的一组方法。
只要把不同的函数（以及记录状态的变量）简单地放在一起，就算是一个模块。
```
function m1(){}
function m2(){}
```
函数m1()和m2()，组成一个模块。直接调用使用。缺点：全局变量的污染。缺点："污染"了全局变量，无法保证不与其他模块发生变量名冲突，而且模块成员之间看不出直接关系。

### 模块对象

var module = new Object({
	_count: 0,
	m1: function(){},
	m2: function(){}
})

<!-- more -->
## AMD异步模块定义
Asynchronous Module Definition
它采用异步方式加载模块，模块的加载不影响它后面语句的运行。所有依赖这个模块的语句，都定义在一个回调函数中，等到加载完成之后，这个回调函数才会运行。

AMD也采用require()语句加载模块，但是不同于CommonJS，它要求两个参数：

```
require([module], callback); 
```

1. 第一个参数[module]，是一个数组，里面的成员就是要加载的模块；
1. 第二个参数callback，则是加载成功之后的回调函数。如果将前面的代码改写成AMD形式，就是下面这样：

```
require(['math'], function (math) {  
math.add(2, 3);
});
```

math.add()与math模块加载不是同步的，浏览器不会发生假死。所以很显然，AMD比较适合浏览器环境。

目前，主要有两个Javascript库实现了AMD规范：require.js和curl.js。本系列的第三部分，将通过介绍require.js，进一步讲解AMD的用法，以及如何将模块化编程投入实战。

最早的时候，所有Javascript代码都写在一个文件里面，只要加载这一个文件就够了。后来，代码越来越多，一个文件不够了，必须分成多个文件，依次加载。下面的网页代码，相信很多人都见过。
这样的写法有很大的缺点。首先，加载的时候，浏览器会停止网页渲染，加载文件越多，网页失去响应的时间就会越长；其次，由于js文件之间存在依赖关系，因此必须严格保证加载顺序（比如上例的1.js要在2.js的前面），依赖性最大的模块一定要放到最后加载，当依赖关系很复杂的时候，代码的编写和维护都会变得困难。

### 解决问题
1. 实现js文件的异步加载，避免网页失去响应；
1. 管理模块之间的依赖性，便于代码的编写和维护。

### 加载require.js的方法：
1. 将其放置底部加载。
```
<script src="js/require.js"></script>
```
1. 异步加载文件。
```
<script src="js/require.js" defer async="true" ></script>
```

```
var module = function(){}

require.config({
	baseURL: 'js/',
	path: {
		'jquery': 'https://ajax.googleapis.com/ajax/libs/jquery/1.7.2/jquery.min',
		'backbone': 'backbone',
	}
})
```

模块化加载：
require.js要求，每个模块是一个单独的js文件。这样的话，如果加载多个模块，就会发出多次HTTP请求，会影响网页的加载速度。因此，require.js提供了一个优化工具，当模块部署完毕以后，可以用这个工具将多个模块合并在一个文件中，减少HTTP请求数。


## 案例

### 项目结构

```
userExample01
    |---- *.html
    |----scripts
		|---- main.js //主文件
		|---- require.js
        |----lib   //各个第三方框架
			|----jquery
				|---- *.js
        |----work  //各个业务工作JS文件
			|---- *.js
```
### HTML
```
<script src="js/main.js"></script>
<script src="js/require.js"></script>
```


### main.js
这个例子的设计要求是workjs01.js 文件依赖 jQuery, workjs02.js 依赖workjs01.js，只有等依赖文件加载完了，最后在页面打出提示信息

```
require.config({
    //定义所有JS文件的基本路径,实际这可跟script标签的data-main有相同的根路径
	baseURL: './scripts',
    //定义各个JS框架路径名,不用加后缀 .js
	paths: {
		'jquery': '',
		'workjs01': 'work/workjs01',
		'workjs02': 'work/workjs02',
		'underscore': ''//路径未提供，可网上搜索然后加上即可
	},
	//包含其它非AMD规范的JS框架
    shim: {
	    'underscore': {
	    	'exports': '_'
		}
	}
});
//按不同先后的依赖关系加载各个JS文件  
require(['jquery','workjs01'],function($,w1){
	require(['workjs02']);
});
```

### 业务模块
```
define(['jquery'],function($){
	var myModule = {}; //推荐方式  
    var moduleName = "work module 01";
    var version = "1.0.0";
    //函数定义区
    var loadTip = function(tipMsg, refConId){  
        var tipMsg = tipMsg || "module is loaded finish.";
        if(undefined === refConId || null === refConId || "" === refConId+""){  
            alert(tipMsg);
        }else{  
            $('#' + (refConId+"")).html(tipMsg);
        }  
    };
    //如有需要暴露(返回)本模块API(相关定义的变量和函数)给外部其它模块使用  
    myModule.moduleName = moduleName;
    myModule.version = version;
    myModule.loadTip = loadTip; 
    return myModule;

    //另一种 
    return{
    	'moduleName': 'work module 01',
    	'version': '1.0.0',
    	'loadTip': loadTip
    };
})
```

```
define(['workjs01'],function(w01){
	var moduleName = 'work module 02';
	var moduleversion = '1.0.0';

	var setHtml = function(refId,strHtml){
		if(undefined === refConId)
	}
})
```

参考资料：
[Require.js](http://requirejs.org/ "")
[Javascript模块化编程（一）：模块的写法](http://www.ruanyifeng.com/blog/2012/10/javascript_module.html "")
[Javascript模块化编程（二）：AMD规范](http://www.ruanyifeng.com/blog/2012/10/asynchronous_module_definition.html "")
[Javascript模块化编程（三）：require.js的用法](http://www.ruanyifeng.com/blog/2012/11/require_js.html "")
[AMD 和 CMD 的区别有哪些](https://www.zhihu.com/question/20351507 "")
[]( "")
[requireJS 简要介绍和完整例子](http://blog.csdn.net/shenzhennba/article/details/51660732 "")
