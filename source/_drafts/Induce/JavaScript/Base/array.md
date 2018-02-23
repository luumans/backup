title: JavaScript Array 数据结构
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
Array 对象方法
<!-- 方法	描述
concat()	连接两个或更多的数组，并返回结果。
join()	把数组的所有元素放入一个字符串。元素通过指定的分隔符进行分隔。
pop()	删除并返回数组的最后一个元素
push()	向数组的末尾添加一个或更多元素，并返回新的长度。
reverse()	颠倒数组中元素的顺序。
shift()	删除并返回数组的第一个元素
slice()	从某个已有的数组返回选定的元素
sort()	对数组的元素进行排序
splice()	删除元素，并向数组添加新元素。
toSource()	返回该对象的源代码。
toString()	把数组转换为字符串，并返回结果。
toLocaleString()	把数组转换为本地数组，并返回结果。
unshift()	向数组的开头添加一个或更多元素，并返回新的长度。
valueOf()	返回数组对象的原始值 -->
<!-- more -->

# 属性
## length
是Array的实例属性。返回或设置一个数组中的元素个数。该值是一个无符号 32-bit 整数，并且总是大于数组最高项的下标。属性的值是一个 0 到 (2^32)-1 的整数。
``` javascript
var items = ['shoes', 'shirts', 'socks', 'sweaters'];
items.length;
// 返回 4
```

### 截断数组

``` javascript
var numbers = [1, 2, 3, 4, 5];
if (numbers.length > 3) {
  numbers.length = 3;
}
console.log(numbers);
// [1, 2, 3]
console.log(numbers.length);
// 3
```

## prototype
属性表示 Array 构造函数的原型，并允许您向所有Array对象添加新的属性和方法。

1. Array.prototype 本身也是一个 Array(`Array.isArray(Array.prototype); // true`)

``` javascript
/*
如果JavaScript本身不提供 first() 方法，
添加一个返回数组的第一个元素的新方法。
*/ 
if(!Array.prototype.first) {
  Array.prototype.first = function() {
    console.log(`如果JavaScript本身不提供 first() 方法，添加一个返回数组的第一个元素的新方法。`);
    return this[0];
  }
}
var numbers = [1, 2, 3, 4, 5];
console.log(numbers.first())
```

## prototype[@@unscopables]
以下的代码在 ES5 或更早的版本中能正常工作。

``` javascript
arr[Symbol.unscopables]
```

# 方法
## from()
方法从一个类似数组或可迭代对象中创建一个新的数组实例。

``` javascript
const bar = ["a", "b", "c"];
Array.from(bar);
// ["a", "b", "c"]

Array.from('foo');
// ["f", "o", "o"]
```
### 语法
``` javascript
Array.from(arrayLike, mapFn, thisArg)

arrayLike
	想要转换成数组的伪数组对象或可迭代对象。
mapFn (可选参数)
	如果指定了该参数，新数组中的每个元素会执行该回调函数。
thisArg (可选参数)
	可选参数，执行回调函数 mapFn 时 this 对象。
```

### 示例

> Array from a String

``` javascript
Array.from('foo'); 
// ["f", "o", "o"]
```

> Array from a Set

ES6提供了新的数据结构Set。它类似于数组，但是成员的值都是唯一的，没有重复的值。
``` javascript
let s = new Set(['foo', window]); 
Array.from(s); 
// ["foo", window]
```

> Array from a Map

``` javascript
let m = new Map([[1, 2], [2, 4], [4, 8]]);
Array.from(m); 
// [[1, 2], [2, 4], [4, 8]]
```

> Array from an Array-like object (arguments)

``` javascript
function f() {
  return Array.from(arguments);
}
f(1, 2, 3);

// [1, 2, 3]
```

> Using arrow functions and Array.from

``` javascript
Array.from([1, 2, 3], x => x + x);      
// [2, 4, 6]

对象长度为5
Array.from({length: 5});
// [undefined, undefined, undefined, undefined, undefined]

Array.from({length: 5}, (v, i) => i); // v 为undefined
// [0, 1, 2, 3, 4]
```

> 多数组去重合并

``` javascript
function combine() {
  let arr = [].concat.apply([], arguments);
  //没有去重复的新数组
  return Array.from(new Set(arr));
}
var m = [1, 2, 2], n = [2,3,3];
console.log(combine(m,n));
// [1, 2, 3]
```

### ECMA-262
``` javascript
// Production steps of ECMA-262, Edition 6, 22.1.2.1
// Reference: https://people.mozilla.org/~jorendorff/es6-draft.html#sec-array.from
if (!Array.from) {
  Array.from = (function () {
    var toStr = Object.prototype.toString;
    var isCallable = function (fn) {
      return typeof fn === 'function' || toStr.call(fn) === '[object Function]';
    };
    var toInteger = function (value) {
      var number = Number(value);
      if (isNaN(number)) { return 0; }
      if (number === 0 || !isFinite(number)) { return number; }
      return (number > 0 ? 1 : -1) * Math.floor(Math.abs(number));
    };
    var maxSafeInteger = Math.pow(2, 53) - 1;
    var toLength = function (value) {
      var len = toInteger(value);
      return Math.min(Math.max(len, 0), maxSafeInteger);
    };
    // The length property of the from method is 1.
    return function from(arrayLike/*, mapFn, thisArg */) {
      // 1. Let C be the this value.
      var C = this;
      // 2. Let items be ToObject(arrayLike).
      var items = Object(arrayLike);
      // 3. ReturnIfAbrupt(items).
      if (arrayLike == null) {
        throw new TypeError("Array.from requires an array-like object - not null or undefined");
      }
      // 4. If mapfn is undefined, then let mapping be false.
      var mapFn = arguments.length > 1 ? arguments[1] : void undefined;
      var T;
      if (typeof mapFn !== 'undefined') {
        // 5. else      
        // 5. a If IsCallable(mapfn) is false, throw a TypeError exception.
        if (!isCallable(mapFn)) {
          throw new TypeError('Array.from: when provided, the second argument must be a function');
        }
        // 5. b. If thisArg was supplied, let T be thisArg; else let T be undefined.
        if (arguments.length > 2) {
          T = arguments[2];
        }
      }
      // 10. Let lenValue be Get(items, "length").
      // 11. Let len be ToLength(lenValue).
      var len = toLength(items.length);
      // 13. If IsConstructor(C) is true, then
      // 13. a. Let A be the result of calling the [[Construct]] internal method 
      // of C with an argument list containing the single item len.
      // 14. a. Else, Let A be ArrayCreate(len).
      var A = isCallable(C) ? Object(new C(len)) : new Array(len);
      // 16. Let k be 0.
      var k = 0;
      // 17. Repeat, while k < len… (also steps a - h)
      var kValue;
      while (k < len) {
        kValue = items[k];
        if (mapFn) {
          A[k] = typeof T === 'undefined' ? mapFn(kValue, k) : mapFn.call(T, kValue, k);
        } else {
          A[k] = kValue;
        }
        k += 1;
      }
      // 18. Let putStatus be Put(A, "length", len, true).
      A.length = len;
      // 20. Return A.
      return A;
    };
  }());
}
```

## isArray()
用于确定传递的值是否是一个 Array。如果对象是 Array ，则返回true，否则为false。

``` javascript
Array.isArray([1, 2, 3]);  
// true
Array.isArray({foo: 123}); 
// false
Array.isArray("foobar");   
// false
Array.isArray(undefined);  
// false
5957096715082514
// 下面的函数调用都返回 true
Array.isArray([]);
Array.isArray([1]);
Array.isArray(new Array());
// 鲜为人知的事实：其实 Array.prototype 也是一个数组。
Array.isArray(Array.prototype); 

// 下面的函数调用都返回 false
Array.isArray();
Array.isArray({});
Array.isArray(null);
Array.isArray(undefined);
Array.isArray(17);
Array.isArray('Array');
Array.isArray(true);
Array.isArray(false);
Array.isArray({ __proto__: Array.prototype });
```

### instanceof与isArray
当检测Array实例时, Array.isArray 优于 instanceof,因为Array.isArray能检测iframes.
``` javascript
var iframe = document.createElement('iframe');
document.body.appendChild(iframe);
xArray = window.frames[window.frames.length-1].Array;
var arr = new xArray(1,2,3); // [1,2,3]

// Correctly checking for Array
Array.isArray(arr);  // true
// Considered harmful, because doesn't work though iframes
arr instanceof Array; // false
```

## of()
方法创建一个具有可变数量参数的新数组实例，而不考虑参数的数量或类型。

``` javascript
Array.of(7); // [7]
Array.of(1, 2, 3); // [1, 2, 3]
Array.of(undefined); // [undefined]

Array(7); // [ , , , , , , ]
Array(1, 2, 3); // [1, 2, 3]
```
 Array.of() 和 Array 构造函数之间的区别在于处理整数参数：Array.of(7) 创建一个具有单个元素 7 的数组，而 Array(7) 创建一个包含 7 个 undefined 元素的数组。

### 兼容旧环境
如果原生不支持的话，在其他代码之前执行以下代码会创建 Array.of() 。
if (!Array.of) {
  Array.of = function() {
    return Array.prototype.slice.call(arguments);
  };
}

## concat()
方法用于合并两个或多个数组。此方法不会更改现有数组，而是返回一个新数组。

1. concat方法不会改变this或任何作为参数提供的数组，而是返回一个浅拷贝，它包含与原始数组相结合的相同元素的副本。 
1. 如果引用的对象被修改，则更改对于新数组和原始数组都是可见的。 这包括也是数组的数组参数的元素。
1. concat将对象引用复制到新数组中。 原始数组和新数组都引用相同的对象。
1. 数据类型如字符串，数字和布尔（不是String，Number 和 Boolean 对象）：concat将字符串和数字的值复制到新数组中。
1. 数组/值在连接时保持不变。此外，对于新数组的任何操作（仅当元素不是对象引用时）都不会对原始数组产生影响，反之亦然。

### 语法
``` javascript
var new_array = old_array.concat(value1[, value2[, ...[, valueN]]])
```

### 连接两个数组
``` javascript
var alpha = ['a', 'b', 'c'];
var numeric = [1, 2, 3];

alpha.concat(numeric);
// ['a', 'b', 'c', 1, 2, 3]

[].concat([1], 1, 2, [2])
// [1, 1, 2, 2]
```

### 连接三个数组
``` javascript
var num1 = [1, 2, 3],
    num2 = [4, 5, 6],
    num3 = [7, 8, 9];

var nums = num1.concat(num2, num3);

console.log(nums); 
// [1, 2, 3, 4, 5, 6, 7, 8, 9]
```

### 将值连接到数组
``` javascript
var alpha = ['a', 'b', 'c'];

var alphaNumeric = alpha.concat(1, [2, 3]);

console.log(alphaNumeric); 
// ['a', 'b', 'c', 1, 2, 3]
```

### 合并嵌套数组
``` javascript
var num1 = [[1]];
var num2 = [2, [3]];

var nums = num1.concat(num2);

console.log(nums);
// results in [[1], 2, [3]]

// 修改第一个对象num1
num1[0].push(4);

console.log(nums);
// [[1, 4], 2, [3]]
```

### concat.apply
``` javascript
console.log([].concat(1, 2, 3));
// [1, 2, 3]
console.log([].concat([1, 2, 3]));
// [1, 2, 3]
console.log([].concat.apply([1, [1]], [2, [3]]));
// [1, [1], 2, 3]
console.log([].concat([1], [2, [3]]));
// [1, 2, [3]]
console.log([].concat.apply([1], [2, [3]]));
// [1, 2, 3]
console.log([].concat.apply([1, [1]], [[3]]));
// [1, Array(1), 3]
console.log([].concat.apply([1], [[3]], [[2]]));
// [1, 3]
```

## concat.apply()
为什么使用apply方法和没有使用apply方法会有区别？

``` javascript
console.log(['12'].concat([1], [2], [3]));
// ['12', 1, 2, 3]
console.log(Array.prototype.concat.apply(['12'], [[1], [2], [3]]));
// ['12', 1, 2, 3]
```
从上面对比的例子可以看出，原理实际上就是['12']这个Array使用了数组的concat方法，接收[1], [2], [3]三个参数。
1. 第一个参数，改变this指向（函数执行时所在的作用域）
1. 第二个参数，以数组作为函数执行时的参数

## 数组元素合并
``` javascript
var flatten = function (arr){
 return [].concat.apply([],arr);
};
flatten([[1, 2], [1, 2]])
// [1, 2, 1, 2]

var flatten = function (array){
 return array.reduce(function(a,b) {
	 return a.concat(b);
 },[])
}
flatten([[1, 2], [1, 2]])
```

## copyWithin()
方法浅复制数组的一部分到同一数组中的另一个位置，并返回它，而不修改其大小。

``` javascript
["alpha", "beta", "copy", "delta"].copyWithin(1, 2, 3);
// 0:"alpha" 1:"beta" 2:"copy" 3:"delta"
// ["alpha", "copy", "copy", "delta"]
// 0:"alpha" 1:"copy" 2:"copy" 3:"delta"

// target === 1:"beta"
// start === 2:"copy", 
// end === 3:"delta"

// 1:"beta" => 1:"copy"

['alpha', 'bravo', 'charlie', 'delta'].copyWithin(2, 0);

// results in ["alpha", "bravo", "alpha", "bravo"]
```

### 参数
``` javascript
arr.copyWithin(target, start, end)
arr.copyWithin(目标索引, [源开始索引], [结束源索引])
```
1. target
0 为基底的索引，复制序列到该位置。如果是负数，target 将从末尾开始计算。
如果 target 大于等于 arr.length，将会不发生拷贝。如果 target 在 start 之后，复制的序列将被修改以符合 arr.length。
1. start
0 为基底的索引，开始复制元素的起始位置。如果是负数，start 将从末尾开始计算。
如果 start 被忽略，copyWithin 将会从0开始复制。
1. end
0 为基底的索引，开始复制元素的结束位置。copyWithin 将会拷贝到该位置，但不包括 end 这个位置的元素。如果是负数， end 将从末尾开始计算。
如果 end 被忽略，copyWithin 将会复制到 arr.length。


``` javascript
[1, 2, 3, 4, 5].copyWithin(2);
// [1, 2, 1, 2, 3]

[1, 2, 3, 4, 5].copyWithin(-2);
// [1, 2, 3, 1, 2]

[1, 2, 3, 4, 5].copyWithin(0, 3);
// [4, 5, 3, 4, 5]

[1, 2, 3, 4, 5].copyWithin(0, 3, 4);
// [4, 2, 3, 4, 5]

[1, 2, 3, 4, 5].copyWithin(-2, -3, -1);
// [1, 2, 3, 3, 4]

[].copyWithin.call({length: 5, 3: 1}, 0, 3);
// {0: 1, 3: 1, length: 5}

// ES2015 Typed Arrays are subclasses of Array
var i32a = new Int32Array([1, 2, 3, 4, 5]);

i32a.copyWithin(0, 2);
// Int32Array [3, 4, 5, 4, 5]

// On platforms that are not yet ES2015 compliant: 
[].copyWithin.call(new Int32Array([1, 2, 3, 4, 5]), 0, 3, 4);
// Int32Array [4, 2, 3, 4, 5]
```
### 思考：如何用 splice 实现 copyWithin？

## entries()
Array.entries()
## every()
Array.every()
## fill()
Array.fill()
## filter()
Array.filter()
## find()
Array.find()
## findIndex()
Array.findIndex()
## forEach()
Array.forEach()
## includes()
Array.includes()
## indexOf()
Array.indexOf()
## join()
Array.join()
## keys()
Array.keys()
## lastIndexOf()
Array.lastIndexOf()
## map()
Array.map()
## pop()
Array.pop()
## push()
Array.push()
## reduce()
Array.reduce()
## reduceRight()
Array.reduceRight()
## reverse()
Array.reverse()
## shift()
Array.shift()
## slice()
方法返回一个从开始到结束（不包括结束）选择的数组的一部分`浅拷贝`到一个新数组对象。原始数组不会被修改。

> Array.slice()

``` javascript
var animals = ['ant', 'bison', 'camel', 'duck', 'elephant'];
console.log(animals.slice(2));
// ["camel", "duck", "elephant"]
console.log(animals.slice(2, 4));
// ["camel", "duck"]
console.log(animals.slice(1, 5));
// ["bison", "camel", "duck", "elephant"]
```

### 语法
``` javascript
arr.slice();
// [0, end]

arr.slice(begin);
// [begin, end]

arr.slice(begin, end);
// [begin, end)
```

1. 如果该参数为负数，则表示从原数组中的倒数第几个元素开始提取，slice(-2)表示提取原数组中的倒数第二个元素到最后一个元素（包含最后一个元素）。
1. 返回值，一个含有提取元素的新数组
1. slice(1,4) 提取原数组中的第二个元素开始直到第四个元素的所有元素 （索引为 1, 2, 3的元素）。

### 类似数组（Array-like）对象
slice方法可以用来将一个类数组（Array-like）对象/集合转换成一个数组。你只需将该方法绑定到这个对象上。
除了使用 Array.prototype.slice.call(arguments)，你也可以简单的使用 [].slice.call(arguments) 来代替。

``` javascript
function list() {
  return Array.prototype.slice.call(arguments);
  // return [].slice.call(arguments)
}
var list1 = list(1, 2, 3);
// [1, 2, 3]
```

bind来简化该过程。
``` javascript
var unboundSlice = Array.prototype.slice;
var slice = Function.prototype.call.bind(unboundSlice);
function list() {
  return slice(arguments);
}
var list1 = list(1, 2, 3);
// [1, 2, 3]
```

## some()
Array.some()
## sort()
Array.sort()
## splice()
Array.splice()
## toLocaleString()
Array.toLocaleString()
## toSource()
Array.toSource()
## toString()
Array.toString()
## unshift()
Array.unshift()
## values()
Array.values()
Array.prototype[@@iterator]()
Array.unobserve()
get Array[@@species]

# 使用
## 遍历一个数组
### forEach
``` javascript
var arrs = ['aaa', 'bbb'];
arrs.forEach(function (item, index, array) {
  console.log(item, index);
});
// aaa  0
// bbb 1
```

### for循环
``` javascript
var arrs = ['aaa', 'bbb'];
for(var i = 0;i < arrs.length;i++){
  console.log(arrs[i]);
}
// aaa
// bbb
```

### for in
``` javascript
var arrs = ['aaa', 'bbb'];
for(var i in arrs){
  console.log(arrs[i]);
}
// aaa
// bbb
```

### for of
``` javascript
var arrs = ['aaa', 'bbb'];
for(var value of arrs){
  console.log(value);
}
// aaa
// bbb
```

### map()
方法创建一个新数组，其结果是该数组中的每个元素都调用一个提供的函数后返回的结果
``` javascript
let array = arr.map(function callback(currentValue, index, array) { 
  // Return element for new_array
}[, thisArg])
方法会将其传入三个参数，分别是`currentValue`当前成员、`index `当前位置和`array `数组本身
```

``` javascript
var numbers = [1, 2, 3];
numbers.map(function (n) {
  return n + 1;
});
// [2, 3, 4]
```

## 为 Array 对象添加一个去除重复项的方法

``` javascript
[false, true, undefined, null, NaN, 0, 1, {}, {}, 'a', 'a', NaN].uniq()
=>
[false, true, undefined, null, NaN, 0, 1, {}, {}, 'a']
```

### 分析
题目要求给 Array 添加方法，所以我们需要用到 prototype。数组去重本身算法不是很难，但是在 JavaScript 中很多人会忽视 NaN 的存在，因为在 JS 中 NaN !== NaN 。但是在去重中我们又不能保留两个 NaN ，所以需要进行一下判断，这是很多人容易忽视的。ES5的实现如下：

### ES5的实现
``` javascript
Array.prototype.uniq = function () {
  var arr = [];
  var flag = true;
  this.forEach(function(item) {
    // 排除 NaN (重要！！！)
    if (item != item) {
      flag && arr.indexOf(item) === -1 ? arr.push(item) : '';
      flag = false;
    } else {
      arr.indexOf(item) === -1 ? arr.push(item) : ''
    }
  });
  return arr;
}
```

``` javascript
var data = [false, true, undefined, null, NaN, 0, 1, {}, {}, 'a', 'a', NaN]
data.uniq()
// [false, true, undefined, null, NaN, 0, 1, {…}, {…}, "a"]
```

### ES6的实现
ES6新增了 Set 对象，也就是我们所说的“集合”，它类似于数组，但是成员的值都是唯一的，没有重复的值。所以可以方便去重。
Set本身是一个构造函数，用来生成Set数据结构。


``` javascript
Array.prototype.uniq = function() {
  return Array.from(new Set(this));
}

Array.prototype.uniq = function() {
  return [...new Set(this)];
}
```