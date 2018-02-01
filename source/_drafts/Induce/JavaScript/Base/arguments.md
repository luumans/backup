title: JavaScript arguments对象
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
是一个对应于传递给函数的参数的类数组对象。

arguments对象是所有（非箭头）函数中都可用的局部变量。
你可以使用arguments对象在函数中引用函数的参数。此对象包含传递给函数的每个参数的条目，第一个条目的索引从0开始。例如，如果一个函数传递了三个参数，你可以以如下方式引用他们：
1. 所有（非箭头）函数中都可用的局部变量
1. 对象不是Array，它类似于Array，但除了length属性和索引元素之外没有任何Array属性。它没有 pop 方法。但是它可以被转换为一个真正的Array：
1. 参数也可以被设置
<!-- more -->

## 实参与形参

``` javascript
function add(a, b) {
  var realLen = arguments.length;
  console.log("realLen:", arguments.length);
  var len = add.length;
  console.log("len:", add.length);
  if (realLen == len) {
    console.log('实参和形参个数一致');
  } else {
    console.log('实参和形参个数不一致');
  }
};
add(1,2,3,6,8);

=>
realLen: 5
len: 2
实参和形参个数不一致
```

## 判断Array
``` javascript
Array.prototype.testArg = 'test';
function funcArg() {
  console.log(funcArg.arguments.testArg);
  console.log(funcArg.arguments[0]);
}
console.log(new Array().testArg);
// result: "test"
funcArg(10);
// result: "undefined"  "10"
Arguments [10, callee: ƒ, Symbol(Symbol.iterator): ƒ]
```

## =>函数
``` javascript
function funcArg() {
	console.log(arguments);
	console.log(typeof arguments);
	console.log(typeof arguments[0]);
}
funcArg(10)

=>
Arguments [10, callee: ƒ, Symbol(Symbol.iterator): ƒ]
object
number

var funcArg = (a) => {
	console.log(a);
	if (typeof arguments !== "object") {
			console.log(typeof arguments);
	}
}
funcArg(10)

=>
10
undefined
```

## 索引
``` javascript
function funcArg() {
	console.log(arguments[0]);
	console.log(arguments[1]);
	console.log(arguments[2]);
}
funcArg(10, 1, 2)

=>
10
1
2
```

## Array转换

``` javascript
function funcArg() {
	var args = Array.prototype.slice.call(arguments);
	var arg = [].slice.call(arguments);
	console.log(args);
	console.log(arg);
	// ES2015
	const argES6 = Array.from(arguments);
	console.log(argES6);
	var argES6s = [...arguments];
	console.log(argES6s);
}
funcArg(10, 1, 2)

=>
[10, 1, 2]

function add() {
  return Array.prototype.reduce.call(arguments, function(n1, n2) {
    return n1 + n2;
  });   
};
add(1,2,3,6,8);

=>
20
```


## 属性

``` javascript
arguments.callee
// 引用函数自身
arguments.length
// 指向传递给当前函数的参数数量。

function funcArg() {
	console.log(arguments.callee)
	console.log(arguments.length)
}
funcArg(10, 1, 2)
```

### callee
实现递归

``` javascript
var sum = function(n) {  
  if(n == 1) { 
    return 1;
  } else {
    return n + arguments.callee(n-1);
　}
}
console.log("sum =", sum(5));
```

## 例子
### 定义连接字符串的函数

``` javascript
function myConcat(separator) {
	console.log(separator)
	// var args = Array.prototype.slice.call(arguments, 1);
	var args = [].slice.call(arguments, 1);
	console.log(args)
	return args.join(separator);
}
myConcat(", ", "red", "orange", "blue");

=>
, 
["red", "orange", "blue"]
"red, orange, blue"
```

### 定义创建HTML列表的方法

``` javascript
function list(type) {
  var result = "<" + type + "l><li>";
  var args = Array.prototype.slice.call(arguments, 1);
  result += args.join("</li><li>");
  // end list
  result += "</li></" + type + "l>";
  return result;
}
list("u", "One", "Two", "Three");

=>
"<ul><li>One</li><li>Two</li><li>Three</li></ul>"
```

### 剩余参数、默认参数和解构赋值参数
arguments对象可以与剩余参数、默认参数和解构赋值参数结合使用。

``` javascript
function foo(...args) {
  return args;
}
foo(1, 2, 3);

=>
[1,2,3]
```

1. 在严格模式下，剩余参数、默认参数和解构赋值参数的存在不会改变 arguments对象的行为，但是在非严格模式下就有所不同了。

#### 剩余参数
剩余参数语法允许我们将一个不定数量的参数表示为一个数组。

``` javascript
function(a, b, ...theArgs) {
  // ...
}
```

``` javascript
function fun1(a, ...theArgs) {
  console.log(theArgs.length);
}
fun1();
fun1(5);
fun1(5, 6, 7);

=>
// 弹出 "0", 因为theArgs没有元素
// 弹出 "0", 因为theArgs没有元素
// 弹出 "2", 因为theArgs有二个元素
```

``` javascript
function sortRestArgs(...theArgs) {
  var sortedArgs = theArgs.sort();
  return sortedArgs;
}
console.log(sortRestArgs(5,3,7,1));

=>
[1, 3, 5, 7]
```

##### 剩余参数和 arguments对象的区别
1. 剩余参数只包含那些没有对应形参的实参，而 arguments 对象包含了传给函数的所有实参。
1. arguments对象不是一个真正的数组，而剩余参数是真正的 Array实例，也就是说你能够在它上面直接使用所有的数组方法，比如 sort，map，forEach或pop。
1. arguments对象还有一些附加的属性 （如callee属性）。

arguments
``` javascript
function sortArguments() {
  var sortedArgs = arguments.sort();
  // arguments 不是Array
  return sortedArgs;
  // 不会执行到这里
}
alert(sortArguments(5,3,7,1));

=>
// 抛出TypeError异常:arguments.sort is not a function

function sortArguments() {
  var args = Array.prototype.slice.call(arguments);
  var sortedArgs = args.sort();
  return sortedArgs;
}
console.log(sortArguments(5, 3, 7, 1));

=>
[1, 3, 5, 7]
```

#### 默认参数
允许在没有值或undefined被传入时使用默认形参。

``` javascript
function multiply(a, b = 1) {
  return a * b;
}
multiply(5, 2);
multiply(5, 1);
multiply(5);

=>
10
5
5
```

#### 解构赋值参数
剩余参数可以被解构，这意味着他们的数据可以被解包到不同的变量中。

``` javascript
function f(...[a, b, c]) {
  return a + b + c;
}
console.log(f(1))
console.log(f(1, 2, 3))
console.log(f(1, 2, 3, 4))

=>
// NaN (b and c are undefined)
// 6
// 6 (the fourth parameter is not destructured)
```

### 非严格模式
当非严格模式中的函数没有包含剩余参数、默认参数和解构赋值，那么arguments对象中的值会跟踪参数的值（反之亦然）。看下面的代码：

``` javascript
function func(a) { 
  arguments[0] = 99;
  // 更新了arguments[0] 同样更新了a
  console.log(a);
}
func(10);

=>
99

function func(a) { 
  a = 99;
  // 更新了a 同样更新了arguments[0] 
  console.log(arguments[0]);
}
func(10);

=>
99
```

当非严格模式中的函数有包含剩余参数、默认参数和解构赋值，那么arguments对象中的值不会跟踪参数的值（反之亦然）。相反, arguments反映了调用时提供的参数：

``` javascript
function func(a = 55) { 
  arguments[0] = 99;
  // 更新arguments，a没有更新
  console.log(a);
}
func(10);

=>
10

function func(a = 55) { 
  a = 99;
  // 更新a，arguments没有更新
  console.log(arguments[0]);
}
func(10);

=>
10

function func(a = 55) { 
  console.log(arguments[0]);
}
func();

=>
undefined
```

## 拓展
-[]( "")
-[]( "")





