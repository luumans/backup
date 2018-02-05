title: JavaScript currying
date: 2018-02-28 18:29:00
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

> 获取数据

1. 默认参数
1. 剩余参数(Array)
1. arguments对象(非Array)

> 数组拼接

1. arguments转数组
``` javascript
Array.from(arguments)
Array.prototype.slice.call(arguments)
```

1. 剩余参数

``` javascript
[...[1,3],...[1,2]] => [1, 3, 1, 2]
```

1. concat()
连接两个或更多的数组，并返回结果。
``` javascript
var new_array = old_array.concat(value1[, value2[, ...[, valueN]]])

sum.concat(Array.from(arguments))
a.concat(4,[1, 2]) => [4, 1, 2]
[].concat(4, 5) => [4, 5]
```

1. slice
从某个已有的数组返回选定的元素
``` javascript
[].slice.call([1, 2, 4], 1) => [2, 4]
[1, 3].slice.call([1, 2]) => [1, 2]
```

除了使用 Array.prototype.slice.call(arguments)，你也可以简单的使用 [].slice.call(arguments) 来代替。另外，你可以使用 bind 来简化该过程。
``` javascript
var unboundSlice = Array.prototype.slice;
var slice = Function.prototype.call.bind(unboundSlice);
function list() {
  return slice(arguments);
}
var list1 = list(1, 2, 3);
// [1, 2, 3]
```

1. push
向数组的末尾添加一个或更多元素，并返回新的长度。
``` javascript
[].push.apply(_args, [].slice.call(arguments));
Array.prototype.push.apply(_args, [].slice.call(arguments))

var a = [1];
var c = [1, 3, 2];
[].push.apply(a, c); // 返回length
console.log(a)
```

1. .apply()


### a+b+c

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

> 求和

这段代码实际上还是不能满足要求的，题主测试成功也只是因为所在环境的 console.log 会将结果转为 string 输出，为了 return tmp 不至于输出一个function，所以重新定义了函数 tmp 的 toString 方法，使其返回 sum 值。
比如如果使用以下代码测试会发现问题
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

``` javascript
var add = function(x, y) {
	if (arguments.length == 1) {
		return function(z) {
			return x + z;
		}
	} else {
		return x + y;
	}
}
console.log(add(2,5)(2)(5));
```

> 剩余参数

``` javascript
function addReduce(...a){
	let b = function(...b){
		console.log(...[...a,...b])
		return addReduce(...[...a,...b])
	}
	b.valueOf = () => {
		return a.reduce((x,y) => x+y )
	}
	return b;
}
console.log(addReduce(1)(2)(3)(4));
```

``` javascript
function add(...a){
  let sum = Array.from(a);
	let fn = function(...b) {
		sum = sum.concat(Array.from(b));
		return fn;
	}
	fn.valueOf = () => {
	  return sum.reduce((x, y) => {
	    return x + y;
	  })
	}
 return fn;
}
console.log(add(1,11)(2)(2,3,4)(4));
```

> arguments

``` javascript
function add(){
	var sum = Array.from(arguments);
	var fn = function() {
	  let argsC = sum.concat(Array.from(arguments));
	  return add.apply(null, argsC)
	}
	fn.valueOf = function() {
	  return sum.reduce((x,y) => {
	    return x + y;
	  })
	}
	return fn;
}
console.log(add(1)(2)(3)(4));
```

``` javascript
function add(){
  let sum = Array.from(arguments);
	let fn = function() {
		sum = sum.concat(Array.from(arguments));
		// return fn;
		return arguments.callee;
	}
	fn.valueOf = () => {
	  return sum.reduce((x, y) => {
	    return x + y;
	  })
	}
 return fn;
}
console.log(add(1)(2)(3)(4));
```

> 自定义方法

``` javascript
var currying = function (fn) {
	let args = [];
	let cuy = function() {
		[].push.apply(args, arguments);
		return arguments.callee;
	}
	cuy.valueOf = function() {
		return fn.apply(null, args);
	}
	return cuy
}
var add = currying(function() {
	let num = Array.from(arguments)
	return num.reduce((x, y) => {
		// console.log('add', num)
		return x + y
	})
})
var ride = currying(function() {
	let num = Array.from(arguments)
	return num.reduce((x, y) => {
		// console.log('ride', num)
		return x * y
	})
})
console.log(add(1)(2))
console.log(add(1, 2))
console.log(add())
// 6
console.log(ride(1, 2, 5))
console.log(ride())
// 10
```

### 编写一个通用的 curry()

``` javascript
function curry(fn) {
  return function curried() {
    var args = [].slice.call(arguments);
    return args.length >= fn.length ?
      fn.apply(null, args) :
      function () {
	      var rest = [].slice.call(arguments);
	      return curried.apply(null, args.concat(rest));
	    };
  };
}
function foo(a,b,c) {
	return a+b+c;
}
var curriedFoo = curry(foo);
console.log(curriedFoo(1,2,3));
// 6
console.log(curriedFoo(1)(2,3));
// 6
console.log(curriedFoo(1)(2)(3));
// 6
console.log(curriedFoo(1,2)(3));
// 6
```

``` javascript
var currying = function (fn) {
	let args = [];
	let cuy = function() {
		[].push.apply(args, arguments);
		return arguments.callee;
	}
	cuy.valueOf = function() {
		return fn.apply(null, args);
	}
	return cuy
}
var add = currying(function() {
	return args.reduce((x, y) => {
		// console.log('add', args)
		return x + y
	})
})
var ride = currying(function() {
	return args.reduce((x, y) => {
		// console.log('ride', args)
		return x * y
	})
})
console.log(add(1)(2))
console.log(add(1, 2))
console.log(add())
// 6
console.log(ride(1, 2, 5))
console.log(ride())
// 10
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

### f(['1','2']) => '12'

``` javascript
var concatArray = function(chars) {
  return chars.reduce(function(a, b) {
  	return a.concat(b);
  });
}
concat(['1','2','3'])

=>
'123'
```

### 固定易变因素
柯里化特性决定了它这应用场景。提前把易变因素，传参固定下来，生成一个更明确的应用函数。最典型的代表应用，是bind函数用以固定this这个易变对象。
``` javascript
Function.prototype.bind = function(context) {
  var _this = this,
  _args = Array.prototype.slice.call(arguments, 1);
  return function() {
    return _this.apply(context, _args.concat(Array.prototype.slice.call(arguments)))
  }
}
```



## Uncurrying
函数柯里化的对偶是Uncurrying，一种使用匿名单参数函数来实现多参数函数的方法。

## 偏函数
Partial Application(偏函数应用) 是指使用一个函数并将其应用一个或多个参数，但不是全部参数，在这个过程中创建一个新函数。

``` javascript
function add3(a, b, c) { return a+b+c; }  
add3(2,4,8);
// 14

var add6 = add3.bind(this, 2, 4);  
add6(8);
// 14  
```

## 拓展
-[JavaScript函数柯里化](https://juejin.im/entry/5a142d6e6fb9a0451170c2c5 "")
-[我理解的函数柯里化](https://juejin.im/entry/5a69501e6fb9a01cbf3888a7 "")
-[高阶函数应用--currying](https://juejin.im/post/599c2448f265da2499602415 "")
-[从一道面试题谈谈函数柯里化 (Currying)](https://juejin.im/entry/5884efee128fe1006c3b64d5 "")
-[Javascript中有趣的反柯里化技术](https://juejin.im/entry/5a1e85d55188253ee45b34ba "")
-[为什么要柯里化（why-curry-helps）slide](https://gist.github.com/jcouyang/b56a830cd55bd230049f "")
-[前端开发者进阶之函数柯里化Currying](https://www.cnblogs.com/pigtail/p/3447660.html "")
-[JS中的柯里化(currying)](http://www.zhangxinxu.com/wordpress/2013/02/js-currying/ "")
-[何为Curry化/柯里化？](https://www.cnblogs.com/zztt/p/4142891.html "")
-[一道题看透函数柯里化(currying)](http://blog.csdn.net/faremax/article/details/72738285 "")
-[掌握 JavaScript 函数的柯里化](https://juejin.im/entry/579ac1f60a2b580058efdb48 "")
-[]( "")
-[]( "")
-[]( "")
-[]( "")
-[]( "")


