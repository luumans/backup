title: Javascript call apply bind
date: 2018-02-28 19:29:00
description: 
categories:
- Javascript
tags:
- Javascript
toc: true
author:
comments:
original:
permalink: 
---
# 来源
继承自Function.prototype中的，所以这三个方法都可以在对象，数组，函数中使用。
``` javascript
console.log(Function.prototype.hasOwnProperty('call'))
// true
console.log(Function.prototype.hasOwnProperty('apply'))
// true
console.log(Function.prototype.hasOwnProperty('bind'))
// true
```
<!-- more -->

``` javascript
function Person(name) {
	this.name = name;
}
Person.prototype = {
	constructor: Person,
	showName: function() {
		console.log(this.name);
	}
}
var person = new Person('qianlong');
person.showName();
// qianlong

var animal = {
  name: 'cat'
}
person.showName.call(animal);
// cat
person.showName.apply(animal);
// cat
person.showName.bind(animal)();
// cat
```
上面看起来三个函数的作用差不多，干的事几乎是一样的，那为什么要存在3个家伙呢，留一个不就可以。所以其实他们干的事从本质上讲都是一样的动态的改变this上下文,但是多少还是有一些差别的..

# call()
调用一个对象的一个方法，以另一个对象替换当前对象。
``` javascript
fun.call(thisArg[, arg1[, arg2[, ...]]])
```

Object.prototype.toString.call([])，就是一个Array对象借用了Object对象上的方法。

## thisArg
函数运行时指定的this值，可能的值为：

1. 不传，或者传null，undefined， this指向window对象
1. 传递另一个函数的函数名fun2，this指向函数fun2的引用
1. 值为原始值(数字，字符串，布尔值),this会指向该原始值的自动包装对象，如 String、Number、Boolean
1. 传递一个对象，函数中的this指向这个对象

## 案例

``` javascript
function a() {
  //输出函数a中的this对象
  console.log(this); 
}
//定义函数b
function b() {
} 

//定义对象obj
var obj = {
	name:'这是一个屌丝'
};
a.call();
// window
a.call(null);
// window
a.call(undefined);
// window
a.call(1);
// Number
a.call('');
// String
a.call(true);
// Boolean
a.call(b);
// function b(){}
a.call(obj);
// {name: "这是一个屌丝"}
```

### 使用call方法调用匿名函数并且指定上下文的this

``` javascript
function greet() {
  var reply = [this.person, '是一个轻量的', this.role].join(' ');
  console.log(reply);
}
var i = {
  person: 'JSLite.io', role: 'Javascript 库。'
};
greet.call(i); 
// JSLite.io 是一个轻量的 Javascript 库。
```

### 使用call方法调用匿名函数

``` javascript
var animals = [
  {species: 'Lion', name: 'King'},
  {species: 'Whale', name: 'Fail'}
];
for (var i = 0; i < animals.length; i++) {
  (function (i) { 
    this.print = function () { 
      console.log('#' + i  + ' ' + this.species + ': ' + this.name); 
    } 
    this.print();
  }).call(animals[i], i);
}
//#0 Lion: King
//#1 Whale: Fail
```

### 使用call方法调用函数传参数

``` javascript
var a = {
    // 定义a的属性
    name: 'JSLite.io',
    // 定义a的方法
    say: function() {
      console.log("Hi,I'm function a!");
    }
};
function b(name){
  console.log("Post params: " + name);
  console.log("I'm " + this.name);
  this.say();
}
b.call(a,'test');
//Post params: test
//I'm JSLite.io
//I'm function a!
```

### arguments转成数组
``` javascript
function list() {
	return Array.prototype.slice.call(arguments);
	// return [].call(arguments);
	// Array.from(arguments)
	// [].from(arguments)
}
list(1, 2, 3);
// [1, 2, 3]
```
为什么能实现这样的功能将arguments转成数组？（`arguments`对象借用了`Array`对象上的方法。）
首先call了之后，this指向了所传进去的arguments。我们可以假设slice方法的内部实现是这样子的：创建一个新数组，然后for循环遍历this，将this[i]一个个地赋值给新数组，最后返回该新数组。因此也就可以理解能实现这样的功能了。

#### 将伪数组转化为数组

``` javascript
var fakeArr = {
	0: 'a',
	1: 'b',
	length: 2
};
var arr1 = Array.prototype.slice.call(fakeArr);
console.log(arr1[0]);
// a
var arr2 = [].slice.call(fakeArr);
console.log(arr2[0]);
// a
arr1.push('c');
console.log(arr1);
// ["a", "b", "c"]
```

### 判断变量类型
``` javascript
function isArray(obj) {
  return Object.prototype.toString.call(obj) == '[object Array]';
}
isArray([]);
// true
isArray('qianlong');
// false
```

# apply()
应用某一对象的一个方法，用另一个对象替换当前对象。 
语法与 call() 方法的语法几乎完全相同，唯一的区别在于，apply的第二个参数必须是一个包含多个参数的数组（或类数组对象）。
``` javascript
fun.apply(thisArg[, argsArray])
```
注意: 需要注意：Chrome 14 以及 Internet Explorer 9 仍然不接受类数组对象。如果传入类数组对象，它们会抛出异常。

## argsArray
一个数组或者类数组对象，其中的数组元素将作为单独的参数传给 fun 函数。如果该参数的值为null 或 undefined，则表示不需要传入任何参数。从ECMAScript 5 开始可以使用类数组对象。

``` javascript
function jsy(x, y, z) {
  console.log(x, y, z);
}
jsy.apply(null, [1, 2, 3]); 
// 1 2 3
```
## 案例

### 使用apply来链接构造器的例子

``` javascript
Function.prototype.construct = function(aArgs) {
  var fConstructor = this,
  fNewConstr = function() {
    fConstructor.apply(this, aArgs);
  };
  fNewConstr.prototype = fConstructor.prototype;
  return new fNewConstr();
};
function MyConstructor () {
    for (var i = 0; i < arguments.length; i++) {
        console.log(arguments, this)
        this["property" + i] = arguments[i];
    }
}
var myArray = [4, "Hello world!", false];
var myInstance = MyConstructor.construct(myArray);
console.log(myInstance.property1);
// Hello world!
console.log(myInstance instanceof MyConstructor);
// true
console.log(myInstance.constructor);
// MyConstructor() {}
```

### 内置函数调用
#### 数组拼接

``` javascript
var array1 = [1 , 2 , 3, 5];
var array2 = ["xie" , "li" , "qun" , "tsrot"];
Array.prototype.push.apply(array1, array2);
console.log(array1);
// [1, 2, 3, 5, "xie", "li", "qun", "tsrot"]

var array1 = [1 , 2 , 3, 5];
array1.push("xie" , "li" , "qun" , "tsrot");
console.log(array1);
// [1, 2, 3, 5, "xie", "li", "qun", "tsrot"]

var array1 = [1 , 2 , 3, 5];
var array2 = ["xie" , "li" , "qun" , "tsrot"];
var arrays = array1.concat(array2);
console.log(arrays);
// [1, 2, 3, 5, "xie", "li", "qun", "tsrot"]
```


#### 最大或者最小值
``` javascript
/* 使用 Math.min/Math.max 在 apply 中应用 */
// 里面有最大最小数字值的一个数组对象
// 一般情况是用 Math.max(5, 6, ..) 或者 Math.max(numbers[0], ...) 来找最大值
var numbers = [5, 6, 2, 3, 7];
var max = Math.max.apply(null, numbers);
var min = Math.min.apply(null, numbers);
console.log('max: ', max);
// max:  7
console.log('min: ', min);
// min:  2
```

``` javascript
//通常情况我们会这样来找到数字的最大或者最小值
//比对上面的栗子，是不是下面的看起来没有上面的舒服呢？
max = -Infinity, min = +Infinity;
for (var i = 0; i < numbers.length; i++) {
  if (numbers[i] > max)
    max = numbers[i];
  if (numbers[i] < min) 
    min = numbers[i];
}
```

### 参数数组切块后循环传入
找到数字的最小值
``` javascript
function minOfArray(arr) {
  var min = Infinity;
  var QUANTUM = 32768;
  for (var i = 0, len = arr.length; i < len; i += QUANTUM) {
    var submin = Math.min.apply(null, arr.slice(i, Math.min(i + QUANTUM, len)));
    min = Math.min(submin, min);
  }
  return min;
}
var min = minOfArray([5, 6, 2, 3, 7]);
// 2
```

# bind() +ES5
函数会创建一个新函数（称为绑定函数）,在ECMAScript5中扩展了叫bind的方法（IE6,7,8不支持），应用某一对象的一个方法，用另一个对象替换当前对象。 
``` javascript
fun.bind(thisArg[, arg1[, arg2[, ...]]]);
```

1. bind是ES5新增的一个方法
1. 传参和call或apply类似
1. 不会执行对应的函数，call或apply会自动执行对应的函数
1. 返回对函数的引用


## 案例
### 保存this变量
``` javascript
var foo = {
  bar: 1,
  eventBind: function() {
    var _this = this;
    $('.someClass').on('click', function(event) {
      console.log(_this.bar);
    });
  }
}

var foo = {
  bar: 1,
  eventBind: function() {
    $('.someClass').on('click', function(event) {
      console.log(this.bar);
    }.bind(this));
  }
}
```

### EventClick
``` javascript
/**
 * 给document添加click事件监听，并绑定EventClick函数
 * 通过bind方法设置EventClick的this为obj，并传递参数p1,p2
 */
document.addEventListener('click',EventClick.bind(obj,'p1','p2'),false);
var obj = {name:'JSLite.io'};
function EventClick(a,b) {
  console.log(
    this.name, //JSLite.io
    a, //p1
    b  //p2
  )
}
// 当点击网页时触发并执行
// JSLite.io p1 p2
```
> call

页面会直接输出 JSLite.io p1 p2
``` javascript
var obj = {name:'JSLite.io'};
document.addEventListener('click',EventClick.call(obj,'p1','p2'),false);
function EventClick(a,b) {
  console.log(
    this.name, //JSLite.io
    a, //p1
    b  //p2
  )
}
// JSLite.io p1 p2
```

bind()方法会创建一个新函数，称为绑定函数。 bind是ES5新增的一个方法，不会执行对应的函数（call或apply会自动执行对应的函数），而是返回对绑定函数的引用。 当调用这个绑定函数时，thisArg参数作为 this，第二个以及以后的参数加上绑定函数运行时本身的参数按照顺序作为原函数的参数来调用原函数。 简单地说，bind会产生一个新的函数，这个函数可以有预设的参数。

``` javascript
function list() {
  return Array.prototype.slice.call(arguments);  
  var leadingThirtysevenList = list.bind(undefined, 37); 
var list = leadingThirtysevenList(1, 2, 3); 
console.log(list); 
bind调用简单
把类数组换成真正的数组，bind能够更简单地使用：

var slice = Array.prototype.slice;
slice.apply(arguments);  
var unboundSlice = Array.prototype.slice;
var slice = Function.prototype.apply.bind(unboundSlice);
slice(arguments);  
```

### 原型扩展

``` javascript
function test() {
  // 检测arguments是否为Array的实例
  console.log(
    arguments instanceof Array, //false
    Array.isArray(arguments)  //false
  );
  // 判断arguments是否有forEach方法
  console.log(arguments.forEach);
  // undefined
  // 将数组中的forEach应用到arguments上
  Array.prototype.forEach.call(arguments,function(item){
    console.log(item);
    // 1 2 3 4
  });
}
test(1, 2, 3, 4);
```

## 兼容处理
-[Polyfill（兼容旧浏览器）](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/bind#Compatibility "")

``` javascript
if (!Function.prototype.bind) {
  Function.prototype.bind = function (self) {
    if (typeof this !== "function") {
      // bind是ES5新增的一个方法
      // 抛出异常
      throw new TypeError("Function.prototype.bind - what is trying to be bound is not callable");
    }
    var aArgs = Array.prototype.slice.call(arguments, 1), 
	    // this在这里指向的是目标函数
	    fToBind = this,
	    fNOP = function () {},
	    fBound = function () {
	      return fToBind.apply(this instanceof fNOP
		      // 此时的this就是new出的obj
	        ? this 
	        // 如果传递的oThis无效，就将fBound的调用者作为this
	        : self || this,
	        // 将通过bind传递的参数和调用时传递的参数进行合并，并作为最终的参数传递
	        aArgs.concat(Array.prototype.slice.call(arguments)));
	    };
    fNOP.prototype = this.prototype;
    //将目标函数的原型对象拷贝到新函数中，因为目标函数有可能被当作构造函数使用
    fBound.prototype = new fNOP();
    //返回fBond的引用，由外部按需调用
    return fBound;
  };
}
```
# 区别
1. call的arg传参需一个一个传，apply则直接传一个数组。
1. call和apply直接执行函数，而bind需要再一次调用。

# this
``` javascript
var name = 'windowsName';
function a() {
	var name = 'Cherry';
	console.log(this.name);// windowsName
	console.log('inner: ' + this);// inner: Window
}
a();
console.log('outer: ' + this);// outer: Window
this -> window.a() -> window
```

``` javascript
var name = 'windowsName';
var a = {
	name: 'Cherry',
	fn: function () {
		console.log(this.name);
	}
}
a.fn();
// Cherry
this -> a.fn() -> a
```

``` javascript
var name = 'windowsName';
var a = {
	name: 'Cherry',
	fn: function () {
		console.log(this.name);
	}
}
window.a.fn();
// Cherry
this -> window.a.fn() -> a
```

``` javascript
var name = 'windowsName';
var a = {
	fn: function () {
		console.log(this.name);
	}
}
window.a.fn();
// undefined
this -> window.a.fn() -> a
```

``` javascript
var name = 'windowsName';
var a = {
	fn: function () {
		console.log(this.name);
	}
}
var f = a.fn();
fn();
// windowsName
this -> window.f() -> window
```

``` javascript
var name = 'windowsName';
var a = {
	name: 'Cherry',
	fn: function () {
		innerFunction();
		function innerFunction() {
			console.log(this.name);
		}
	}
}
window.a.fn();
// windowsName
this -> window.innerFunction() -> window
```

## 改变this指向
1. 使用 ES6 的箭头函数
1. 在函数内部使用 _this = this
1. 使用 apply、call、bind
1. new 实例化一个对象

``` javascript
var name = 'windowsName';
var a = {
	name: 'Cherry',
	func1: function () {
		console.log(this.name);
	},
	func2: function () {
		setTimeout(function () {
			this.func1();
		}, 100);
	}
};
a.func2();
// this.func1 is not a function
```
在不使用箭头函数的情况下，是会报错的，因为最后调用 setTimeout 的对象是 window，但是在 window 中并没有 func1 函数。

> 箭头函数

``` javascript
var name = 'windowsName';
var a = {
	name: 'Cherry',
	func1: function () {
		console.log(this.name);
	},
	func2: function () {
		setTimeout(() => {
			this.func1();
		}, 100);
	}
};
a.func2();
// Cherry
```

> 函数内部使用

``` javascript
var name = 'windowsName';
var a = {
	name: 'Cherry',
	func1: function () {
		console.log(this.name);
	},
	func2: function () {
		var self = this;
		setTimeout(function() {
			self.func1();
		}, 100);
	}
};
a.func2();
// Cherry
```

> 使用 apply、call、bind

``` javascript
var name = 'windowsName';
var a = {
	name: 'Cherry',
	func1: function () {
		console.log(this.name);
	},
	func2: function () {
		setTimeout(function() {
			this.func1();
		}.apply(a), 100);
	}
};
a.func2();
// Cherry
```

``` javascript
var name = 'windowsName';
var a = {
	name: 'Cherry',
	func1: function () {
		console.log(this.name);
	},
	func2: function () {
		setTimeout(function() {
			this.func1();
		}.call(a), 100);
	}
};
a.func2();
// Cherry
```

``` javascript
var name = 'windowsName';
var a = {
	name: 'Cherry',
	func1: function () {
		console.log(this.name);
	},
	func2: function () {
		setTimeout(function() {
			this.func1();
		}.bind(a)(), 100);
	}
};
a.func2();
// Cherry
```

# JS 中的函数调用
## 作为一个函数调用

``` javascript
var name = 'windowsName';
function a() {
	var name = 'Cherry';
	console.log(this.name);
	console.log('inner: ' + this);
}
a();
console.log('outer: ' + this);
// windowsName
// inner: Window
// outer: Window
```

## 函数作为方法调用
``` javascript
var name = 'windowsName';
var a = {
	name: 'Cherry',
	fn : function () {
	  console.log(this.name);
	}
}
a.fn();
// Cherry
```

## 使用构造函数调用函数
``` javascript
function myFunction(arg1, arg2) {
  this.firstName = arg1;
  this.lastName  = arg2;
}
var a = new myFunction('Li', 'Cherry');
a.lastName;
```

伪代码表示：
``` javascript
var a = new myFunction('Li', 'Cherry');
new myFunction {
  var obj = {};
  obj.__proto__ = myFunction.prototype;
  var result = myFunction.call(obj, 'Li', 'Cherry');
  return typeof result === 'obj' ? result : obj;
}
创建一个空对象 obj;
将新创建的空对象的隐式原型指向其构造函数的显示原型。
使用 call 改变 this 的指向
如果无返回值或者返回一个非对象值，则将 obj 返回作为新对象；如果返回值是一个新对象的话那么直接直接返回该对象。
```


## 作为函数方法调用函数（call、apply）
``` javascript
var name = 'windowsName';
function fn() {
	var name = 'Cherry';
	innerFunction();
	function innerFunction() {
		console.log(this.name);      // windowsName
	}
}
fn();

var name = 'windowsName';
var a = {
	name : 'Cherry',
	func1: function () {
	  console.log(this.name)     
	},
	func2: function () {
	  setTimeout(  function () {
	      this.func1()
	  },100);
	}
};
a.func2();
// this.func1 is not a function
```

# function
## 属性
1. length：形参的个数；
1. name：函数名；
1. prototype：类的原型，在原型上定义的方法都是当前这个类的实例的公有方法；
1. \__proto__：把函数当做一个普通对象，指向Function这个类的原型

# 拓展
-[回味JS基础:call apply 与 bind](https://juejin.im/post/57dc97f35bbb50005e5b39bd "基础知识")
-[JS 中的 call、apply、bind 方法详解](https://juejin.im/entry/57eb38b45bbb50005d754341 "")
-[JS 中 call、apply、bind 那些事](https://juejin.im/entry/5812edcea22b9d006779b067 "")
-[this、apply、call、bind](https://juejin.im/post/59bfe84351882531b730bac2 "")
-[JavaScript 中的 call、apply、bind 深入理解](https://juejin.im/entry/58871eba570c350062d2eab0 "")
<!-- -[]( "")
-[]( "")
-[]( "")
-[]( "")
-[]( "")
-[]( "")
-[]( "")
 -->
