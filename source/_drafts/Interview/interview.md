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
add(1)(2)(3)...
我们有一个需求，用js实现一个无限极累加的函数add(1)(2)(3)...

方法 | 结果
-------|------
add(1) | 1
add(1)(2)  | 2
add(1)(2)(3) |  6
add(1)(2)(3)(4) | 10
以此类推。。。。。 | ...
<!-- more -->


## toString()
首先，我们要了解一个知识点： 函数的 toString()方法当我们直接alert() 一个函数的时候会被调用（或者 用 console.log() 打印一个函数的时候会被调用）。

``` javascript
function fn(a){
  return  a+1;
}
console.log(fn);

=>
ƒ fn(a){
  return  a+1;
}
```

## 自定义toSting() 
定义了fn.toString方法后，直接console.log(s)的话，会打印出这个函数， 如果直接 alert(s)的话，我们可以看到会弹出 “2”， 我们检测一下s 的类型（typeof(s)）,
除了valueOf，还有toString
``` javascript
function fn(a) {
  return a+1;
}
fn.toString = function () {
	return 2;
}
console.log(fn);
typeof(fn)

=>
ƒ 2
"function"

function fn(a) {
  return a+1;
}
fn.valueOf = function () {
	return 2;
}
console.log(fn);
typeof(fn)
```
## 包裹
在上面的代码中，我们给之前的代码 包裹了一层，并且也修改了一下fn.toSting()方法，让它返回的是外面传递进来的参数a, 而不是之前固定的2。包裹了一层后，返回值为一个函数，这样就形成了一个闭包。 这样，我们在调用 add(3) 的时候，返回值其实是一个函数， 里面的 s 这个函数。但是，当我们 alert(s(3)) 的时候， 会弹出  3 

``` javascript
function add(a){
	function fn(a){
	  return  a+1;
	}
	fn.toString = function(){
		return a;
	}
	return fn;
}
console.log(add(3));
```
## 函数柯里化

``` javascript
function add(x) {
	return function (y) {
		return function (z) {
			return function (d) {
				return x + y + z + d;
			}
		}
	}
}
console.log(add(1)(2)(3)(4))
```

``` javascript
function addFn(x) {
	return function fn(y) {
		console.log(y)
		return fn
	}
}
console.log(addFn(1)(2)(3)(4))

=>
addFn(1) fn(2) fn(3) fn(4)
```

## 结果

``` javascript
function add(a) {
	function fn(b) {
		a = a + b;
		return fn;
	}
	fn.toString = function () {
		return a;
	}
	return fn;
}
console.log(add(1)(2)(3)(4));
```

``` javascript
function addValueOf(n){
  var fn = function(x) {
		return addValueOf(n + x);
  };
  fn.valueOf = function (){
		return n;
  };
  console.log('addValueOf', fn);
	return fn;
}
console.log(addValueOf(1)(2)(3)(4))

function addReduce(...a){
	let b = function(...b){
		return addReduce(...[...a,...b])
	}
	b.valueOf = () => {
		return a.reduce((x,y) => x+y )
	}
	console.log('addReduce', b);
	return b;
}
console.log(addReduce(1)(2)(3)(4))

function addArgsP(){
	var argsP = Array.from(arguments);
	var fn = function(){
	  let argsC = argsP.concat(Array.from(arguments));
	  // console.log(argsC)
	  return addArgsP.apply(null, argsC)
	}
	fn.valueOf = function(){
	  return argsP.reduce((x,y) => {
	    return x+y;
	  })
	}
	console.log('addArgsP', fn);
	return fn;
}
console.log(addArgsP(1)(2)(3)(4))
```






我都不敢相信，真的有创业公司会在望京SOHO。到了楼下果然气派，深圳什么有限公司，到公司门口就被那炫酷的UI设计图吸引力，果然是创业公司的氛围。
老板就呆在很小的隔间里。大家都在努力的工作，应该还有一个管理人员，HR让我坐下准备。结果，他们领导说他不用准备简历。当时，就把我下到了。这是什么情况，我也不太理解，结果HR就给我递过来一个面试题。
拿到面试题的那一瞬间，我觉得自己干劲十足，可以第二秒，我就跪了。这是什么呀，全是JS。我都水平明明很菜，结果给我弄了这么题目。
我当时就觉得自己跪了。
然后喃，我叫了面试题目，过了一会儿。HR进来了，不是说不要简历的吗，干嘛还带着电脑。接下来，他蹬蹬点了几下，突然有个人出现在屏幕里面。呀，他说这是我们的技术大牛CTO，人现在在深圳，远远的看到他的背景是个很豪气的房间，当时我十分紧张。
让我自我介绍，我简单的自我介绍了一下，接下来，可让我无语了，他说，你做的什么呀，这下题目都是从你博客上面看到的，不是你自己整理的吗？怎么不会写，我当时就我无语了。我整理他，是给自己看的，结果搬石头砸自己脚了。也没什么，去了我也卡我不了。
最后我觉得CTO也是，无语了，这是什么呀！对我已经无语了。这怪谁，要怪就怪自己太菜。

回来之后，群里的人也在讨论面试，现在的牛人真的很多呀，大三，出来面试对JS有很深的了解。读了很多框架的源码。而且，英文很好，擅长阅读英文源码。

## if (!('a' in window)) {
``` javascript
if (!('a' in window)) {
	var a=1;
}
alert(a);

=>
undefined
```

前面我说过function和var会提前声明，而其实{…}内的变量也会提前声明。于是代码还没执行前，a变量已经被声明，于是 ‘a’ in window 返回false，a没有赋值。

## 实现 var a=(5).apiont(3).minte(6);

实现 var a=add(1)(2)(3);//6
简单描述Bootscript、A、E框架的优点缺点。
闭包
原型链

JS 中 var add=function(){} add(a)(b)(c); 是什么简写？
https://www.zhihu.com/question/22258223

function add(x) {
	return function (y) {
		return function (z) {
			return x + y + z;
		}
	}
}
add(1)(2)(3);

function add(num){
    var sum=0;
    sum= sum+num;
    return function tempFun(numB){
        sum= sum+ numB;
        return tempFun;
    }
}

function add(num){
    var sum=0;
    sum= sum+num;
    return function(numB){
        sum= sum+ numB;
        return function(numC){
            sum= sum+ numC;
            return sum;
        }
    }
}

function add(num){
    var sum=0;
    sum= sum+num;
    return function tempFun(numB){
        sum= sum+ numB;
        return tempFun;
    }
}

function add(...a){
 let b = function(...b){
    return add(...[...a,...b])
 }
 b.valueOf = () => {
  return a.reduce((x,y) => x+y )
 }
 return b;
}

function add(n){
  var fn = function(x) {
		return add(n + x);
  };
  fn.valueOf = function (){
		return n;
  };
	return fn;
}

function add(x) {
    var sum = x;
    var tmp = function (y) {
        sum = sum + y;
        return tmp;
    };
    tmp.toString = function () {
        return sum;
    };
    return tmp;
}
add(1)(2)(3)(4)

建议75：函数柯里化
http://book.2cto.com/201211/9320.html



渐渐的面试好多家公司，几次被虐之后，你会发现自己还有很多知识，没有掌握良好。虽然，被各种面试官看不起，却会让你发现自己还有很多的不足，还有很多牛逼的人，还在不断地努力前进。同时也让你的知识有了体系，进行了重新的架构，了解更多新鲜的技术。




