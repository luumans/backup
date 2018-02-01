title: JavaScript currying
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
在计算机科学中，柯里化（英语：Currying），又译为`卡瑞化`或`加里化`，是把接受多个参数的函数变换成接受一个单一参数（最初函数的第一个参数）的函数，并且返回接受余下的参数而且返回结果的新函数的技术。这个技术由`克里斯托弗·斯特雷奇`以逻辑学家`哈斯凯尔·加里`命名的，尽管它是`Moses Schönfinkel`和`戈特洛布·弗雷格`发明的。

高阶函数的特点：
1. 函数可以作为参数被传递；
1. 函数可以作为返回值输出。

``` javascript
var foo = function(a) {
  return function (b) {
    return function (c) {
      return a+b+c;
    };
  };
};
```
<!-- more -->



反柯里化：函数柯里化的对偶是Uncurrying，一种使用匿名单参数函数来实现多参数函数的方法。

## 理解
Currying概念其实很简单，只传递给函数一部分参数来调用它，让它返回一个函数去处理剩下的参数。


### 简单求和函数

> a+b+c

``` javascript
function add(a, b, c) {
	return a + b + c;
}
console.log(add(1, 2, 3))

=>
6
```

> a+b+c Currying

``` javascript
function add(x) {
	return function (y) {
		return function (z) {
			return x + y + z;
		}
	}
}
var addOne = add(1);
var addOneAndTwo = addOne(2);
var addOneAndTwoAndThree = addOneAndTwo(3);
console.log(addOneAndTwoAndThree);
```
这里我们定义了一个add函数，它接受一个参数并返回一个新的函数。调用add之后，返回的函数就通过闭包的方式记住了add的第一个参数。一次性地调用它实在是有点繁琐，好在我们可以使用一个特殊的curry帮助函数（helper function）使这类函数的定义和调用更加容易。

用ES6的箭头函数，我们可以将上面的add实现成这样：

``` javascript
const add = x => y => z => x + y + z;
```
好像使用箭头函数更清晰了许多。



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

``` javascript
function Arguments() {
	console.log(arguments)
	var fn = function () {
		console.log(arguments)
		return fn
	}
	fn.toString = function () {
		return arguments;
	}
	return fn;
}
console.log(Arguments(1)(2)(3)(4))

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

## 场景使用
实际场景的运用能更好的加深我们的理解，所以我们用实际的生活场景来看看 柯里化 如何运用。
### a+b+c...
> x + y + z + d

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

> 方法一

``` javascript
function add(a) {
	function fn(b) {
		a = a + b;
		return fn;
	}
	fn.valueOf = function () {
		return a;
	}
	return fn;
}
console.log(add(1)(2)(3)(4));
```

``` javascript
function addCurry(x) {
  var sum = x;
  var fn = function (y) {
    sum = sum + y;
    return fn;
  };
  fn.valueOf = function () {
    return sum;
  };
  console.log('addCurry', fn);
  return fn;
}
console.log(addCurry(1)(2)(3)(4))
```

``` javascript
function add() {
	var sum = arguments[0];
	var fn =  function () {
		sum = sum + arguments[0];
		return arguments.callee;
	}
	fn.valueOf = function () {
		return sum;
	}
	return fn;
}
console.log(add(1)(2)(3)(4));
```

> 方法一

``` javascript
```


``` javascript
function addValueOf(n){
  var fn = function(x) {
		return addValueOf(n + x);
  };
  fn.valueOf = function (){
		return n;
  };
	return fn;
}
console.log(addValueOf(1)(2)(3)(4))
```

``` javascript
function addReduce(...a){
	let b = function(...b){
		return addReduce(...[...a,...b])
	}
	b.valueOf = () => {
		return a.reduce((x,y) => x+y )
	}
	return b;
}
console.log(addReduce(1)(2)(3)(4))
```

> 方法一

``` javascript
function addArgsP(){
	var argsP = Array.from(arguments);
	var fn = function(){
	  let argsC = argsP.concat(Array.from(arguments));
	  return addArgsP.apply(null, argsC)
	}
	fn.valueOf = function(){
	  return argsP.reduce((x,y) => {
	    return x+y;
	  })
	}
	return fn;
}
console.log(addArgsP(1)(2)(3)(4))
```

``` javascript
function add() {
	var sum = arguments;
	return function () {
		sum = sum + arguments;
		this.valueOf = fucntion () {
			return sum;
		}
		return arguments.callee;
	}
}
console.log(add(1)(2)(3)(4));

var currying = function (fn) {
	
}
```

### 记账本
记账本，每天记录使用多少钱，一个月算一次总花费（或者在我想要知道的时候）

``` javascript
function account(){
	var total = [];
	function money(p) {
		if (arguments.length === 0) {
			// 计算一共花了多少钱
			total = total.reduce((sum, value) => {
			  return sum + value;
			}, 0)
			console.log('一共花了', total+' 元');
		} else {
			// 记录每天花了多少钱
			total.push(p)
			console.log('今天花了', p+' 元');
		}
	}
	return money;
}
var spend = account();
console.log(spend(15))
console.log(spend(30))
// 不传参数的时候就返回真正的结果
console.log(spend())

=>
今天花了 15 元
今天花了 30 元
一共花了 45 元
```

### 老婆的测试

``` javascript
var currying = function(fn) {
    // fn 指官员消化老婆的手段
    console.log(arguments);
    var args = [].slice.call(arguments, 1);
    console.log(args);
    // args 指的是那个合法老婆
    return function() {
    	console.log(arguments);
        // 已经有的老婆和新搞定的老婆们合成一体，方便控制
        var newArgs = args.concat([].slice.call(arguments));
        // 这些老婆们用 fn 这个手段消化利用，完成韦小宝前辈的壮举并返回
        return fn.apply(null, newArgs);
    };
};

// 下为官员如何搞定7个老婆的测试
// 获得合法老婆
var getWife = currying(function() {
		console.log(arguments);
    var allWife = [].slice.call(arguments);
    // allwife 就是所有的老婆的，包括暗渡陈仓进来的老婆
    console.log(allWife.join(";"));
}, "合法老婆");

// 获得其他6个老婆
console.log('大老婆')
getWife("大老婆","小老婆","俏老婆","刁蛮老婆","乖老婆","送上门老婆");

// 换一批老婆
console.log('超越韦小宝的老婆')
getWife("超越韦小宝的老婆");

=>
Arguments(2) [ƒ, "合法老婆", callee: ƒ, Symbol(Symbol.iterator): ƒ]
["合法老婆"]
大老婆
Arguments(6) ["大老婆", "小老婆", "俏老婆", "刁蛮老婆", "乖老婆", "送上门老婆", callee: ƒ, Symbol(Symbol.iterator): ƒ]
Arguments(7) ["合法老婆", "大老婆", "小老婆", "俏老婆", "刁蛮老婆", "乖老婆", "送上门老婆", callee: ƒ, Symbol(Symbol.iterator): ƒ]
合法老婆;大老婆;小老婆;俏老婆;刁蛮老婆;乖老婆;送上门老婆
超越韦小宝的老婆
Arguments ["超越韦小宝的老婆", callee: ƒ, Symbol(Symbol.iterator): ƒ]
Arguments(2) ["合法老婆", "超越韦小宝的老婆", callee: ƒ, Symbol(Symbol.iterator): ƒ]
合法老婆;超越韦小宝的老婆
```

### 实现柯里化
``` javascript
var currying = function(fn) {
  var args = [];
  return function() {
    if (arguments.length === 0) {
      // 没传参数时，调用这个函数
      return fn.apply(this, args);
    } else {
      // 传入了参数，把参数保存在args
      [].push.apply(args, arguments);
      // 返回这个函数的引用
      return arguments.callee;
    }
  }
}
var cost = (function() {
  var money = 0;
  return function() {
    for (var i = 0; i < arguments.length; i++) {
      money += arguments[i];
    }
    return money;
  }
})();
// 返回currying function的方法
var costs = currying(cost);
console.log(costs());
// 传入了参数，不真正求值（返回这个函数的引用）
console.log(costs(100));
// 传入了参数，不真正求值（返回这个函数的引用）
console.log(costs(200)(300));
// 传入了参数，不真正求值（返回这个函数的引用）
console.log(costs(300));
// 求值并且输出900
console.log(costs());
```

``` javascript
var currying = function(fn) {
  var args = Array.prototype.slice.call(arguments, 1);
  return function() {
    if (arguments.length === 0) {
      // 没传参数时，调用这个函数
      return fn.apply(this, args);
    } else {
      // 传入了参数，把参数保存在args
      [].push.apply(args, arguments);
      // 返回这个函数的引用
      return arguments.callee;
    }
  }
}
var cost = (function() {
  var money = 0;
  return function() {
    for (var i = 0; i < arguments.length; i++) {
      money += arguments[i];
    }
    return money;
  }
})();
function uncurrying(fn) {
  return function(...args) {
    var ret = fn;
    for (let i = 0; i < args.length; i++) {
      // 反复调用currying版本的函数
      ret = ret(args[i]);
    }
    // 返回结果
    return ret;
  };
}
// 返回currying function的方法
var curryingCost = currying(cost);
// 返回uncurrying function的方法
var uncurryingCost = uncurrying(curryingCost);
// 600
console.log(uncurryingCost(100, 200, 300));
console.log(uncurryingCost()());
```

## 拓展
-[JavaScript函数柯里化](https://juejin.im/entry/5a142d6e6fb9a0451170c2c5 "")
-[我理解的函数柯里化](https://juejin.im/entry/5a69501e6fb9a01cbf3888a7 "")
-[高阶函数应用--currying](https://juejin.im/post/599c2448f265da2499602415 "")
-[从一道面试题谈谈函数柯里化 (Currying)](https://juejin.im/entry/5884efee128fe1006c3b64d5 "")
-[Javascript中有趣的反柯里化技术](https://juejin.im/entry/5a1e85d55188253ee45b34ba "")
-[]( "")
