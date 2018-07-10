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
[1, 2, 3, 4, 5].copyWithin(1);
// [1, 1, 2, 3, 4]
[1, 2, 3, 4, 5].copyWithin(2);
// [1, 2, 1, 2, 3]
[1, 2, 3, 4, 5].copyWithin(-2);
// [1, 2, 3, 1, 2]
```
1. 基底为0，复杂序列到该位置
1. 如果是负数，target 将从末尾开始计算位置
1. 默认start为0，连接全部内容

``` javascript
[1, 2, 3, 4, 5].copyWithin(1, 0);
// [1, 1, 2, 3, 4]
[1, 2, 3, 4, 5].copyWithin(1, 3);
// [1, 4, 5, 4, 5]

[1, 2, 3, 4, 5].copyWithin(0, 3);
// [4, 5, 3, 4, 5]
[1, 2, 3, 4, 5].copyWithin(3, 2);
// [1, 2, 3, 3, 4]
[1, 2, 3, 4, 5].copyWithin(1, 2);
// [1, 3, 4, 5, 5]
```
1. 当替换的内容小于length时，原有的内容保持。
1. 开始复制元素的起始位置之后的内容。

``` javascript

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

### 兼容旧环境（Polyfill）
``` javascript
if (!Array.prototype.copyWithin) {
  Array.prototype.copyWithin = function(target, start/*, end*/) {
    // Steps 1-2.
    if (this == null) {
      throw new TypeError('this is null or not defined');
    }
    var O = Object(this);
    // Steps 3-5.
    var len = O.length >>> 0;
    // Steps 6-8.
    var relativeTarget = target >> 0;
    var to = relativeTarget < 0 ?
      Math.max(len + relativeTarget, 0) :
      Math.min(relativeTarget, len);
    // Steps 9-11.
    var relativeStart = start >> 0;

    var from = relativeStart < 0 ?
      Math.max(len + relativeStart, 0) :
      Math.min(relativeStart, len);

    // Steps 12-14.
    var end = arguments[2];
    var relativeEnd = end === undefined ? len : end >> 0;

    var final = relativeEnd < 0 ?
      Math.max(len + relativeEnd, 0) :
      Math.min(relativeEnd, len);

    // Step 15.
    var count = Math.min(final - from, len - to);

    // Steps 16-17.
    var direction = 1;

    if (from < to && to < (from + count)) {
      direction = -1;
      from += count - 1;
      to += count - 1;
    }

    // Step 18.
    while (count > 0) {
      if (from in O) {
        O[to] = O[from];
      } else {
        delete O[to];
      }

      from += direction;
      to += direction;
      count--;
    }

    // Step 19.
    return O;
  };
}
```

### 思考：如何用 splice 实现 copyWithin？

``` javascript
copyWithins
```

## entries()
方法返回一个新的Array Iterator对象，该对象包含数组中每个索引的键/值对。
一个新的 Array 迭代器对象。Array Iterator是对象，它的原型（__proto__:Array Iterator）上有一个next方法，可用用于遍历迭代器取得原数组的[key,value]。

``` javascript
var arr = ["a", "b", "c"];
var iterator = arr.entries();
// undefined

console.log(iterator);
// Array Iterator {}

console.log(iterator.next().value); 
// [0, "a"]
console.log(iterator.next().value); 
// [1, "b"]
console.log(iterator.next().value); 
// [2, "c"]
```

### 示例

####  Array Iterator
``` javascript
var arr = ["a", "b", "c"];
var iterator = arr.entries();
console.log(iterator);

/*Array Iterator {}
         __proto__:Array Iterator
         next:ƒ next()
         Symbol(Symbol.toStringTag):"Array Iterator"
         __proto__:Object
*/
```
#### iterator.next()
``` javascript
var arr = ["a", "b", "c"]; 
var iterator = arr.entries();
console.log(iterator.next());

/*{value: Array(2), done: false}
          done:false
          value:(2) [0, "a"]
           __proto__: Object
*/
// iterator.next()返回一个对象，对于有元素的数组，
// 是next{ value: Array(2), done: false }；
// next.done 用于指示迭代器是否完成：在每次迭代时进行更新而且都是false，
// 直到迭代器结束done才是true。
// next.value是一个["key":"value"]的数组，是返回的迭代器中的元素值。
```
#### iterator.next方法运行
``` javascript
var arr = ["a", "b", "c"];
var iter = arr.entries();
var a = [];

// for(var i=0; i< arr.length; i++){   // 实际使用的是这个 
for(var i=0; i< arr.length+1; i++){    // 注意，是length+1，比数组的长度大
    var tem = iter.next();             // 每次迭代时更新next
    console.log(tem.done);             // 这里可以看到更新后的done都是false
    if(tem.done !== true){             // 遍历迭代器结束done才是true
        console.log(tem.value);
        a[i]=tem.value;
    }
}
    
console.log(a);                         // 遍历完毕，输出next.value的数组
```
#### 二维数组按行排序
``` javascript
function sortArr(arr) {
    var goNext = true;
    var entries = arr.entries();
    while (goNext) {
        var result = entries.next();
        if (result.done !== true) {
            result.value[1].sort((a, b) => a - b);
            goNext = true;
        } else {
            goNext = false;
        }
    }
    return arr;
}

var arr = [[1,34],[456,2,3,44,234],[4567,1,4,5,6],[34,78,23,1]];
sortArr(arr);

/*(4) [Array(2), Array(5), Array(5), Array(4)]
    0:(2) [1, 34]
    1:(5) [2, 3, 44, 234, 456]
    2:(5) [1, 4, 5, 6, 4567]
    3:(4) [1, 23, 34, 78]
    length:4
    __proto__:Array(0)
*/
```
#### 使用for…of 循环
``` javascript
var arr = ["a", "b", "c"];
var iterator = arr.entries();
// undefined

for (let e of iterator) {
    console.log(e);
}

// [0, "a"] 
// [1, "b"] 
// [2, "c"]
```

## every()
方法测试数组的所有元素是否都通过了指定函数的测试。

### 语法
``` javascript
arr.every(callback[, thisArg])
```

``` javascript
function isBigEnough(element, index, array) {
  return (element >= 10);
}
var passed = [12, 5, 8, 130, 44].every(isBigEnough);
// false
passed = [12, 54, 18, 130, 44].every(isBigEnough);
// true
```

### 兼容旧环境（Polyfill）
``` javascript
if (!Array.prototype.every) {
  Array.prototype.every = function(fun /*, thisArg */) {
    'use strict';
    if (this === void 0 || this === null)
      throw new TypeError();
    var t = Object(this);
    var len = t.length >>> 0;
    if (typeof fun !== 'function')
      throw new TypeError();
    var thisArg = arguments.length >= 2 ? arguments[1] : void 0;
    for (var i = 0; i < len; i++) {
      if (i in t && !fun.call(thisArg, t[i], i, t))
        return false;
    }
    return true;
  };
}
```

## fill() ！
方法用一个固定值填充一个数组中从起始索引到终止索引内的全部元素。

### 语法
``` javascript
arr.fill(value[, start[, end]])
```

``` javascript
var array1 = [1, 2, 3, 4];

// fill with 0 from position 2 until position 4
console.log(array1.fill(0, 2, 4));
// expected output: [1, 2, 0, 0]

// fill with 5 from position 1
console.log(array1.fill(5, 1));
// expected output: [1, 5, 5, 5]

console.log(array1.fill(6));
// expected output: [6, 6, 6, 6]
```

### 兼容旧环境（Polyfill）
``` javascript
if (!Array.prototype.fill) {
  Object.defineProperty(Array.prototype, 'fill', {
    value: function(value) {
      // Steps 1-2.
      if (this == null) {
        throw new TypeError('this is null or not defined');
      }
      var O = Object(this);
      // Steps 3-5.
      var len = O.length >>> 0;
      // Steps 6-7.
      var start = arguments[1];
      var relativeStart = start >> 0;
      // Step 8.
      var k = relativeStart < 0 ?
        Math.max(len + relativeStart, 0) :
        Math.min(relativeStart, len);
      // Steps 9-10.
      var end = arguments[2];
      var relativeEnd = end === undefined ?
        len : end >> 0;
      // Step 11.
      var final = relativeEnd < 0 ?
        Math.max(len + relativeEnd, 0) :
        Math.min(relativeEnd, len);
      // Step 12.
      while (k < final) {
        O[k] = value;
        k++;
      }
      // Step 13.
      return O;
    }
  });
}
```

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
用就地（ in-place ）的算法对数组的元素进行排序，并返回数组。 sort 排序不一定是稳定的。默认排序顺序是根据字符串Unicode码点。

``` javascript
var fruit = ['cherries', 'apples', 'bananas'];
fruit.sort(); 
// ['apples', 'bananas', 'cherries']

var scores = [1, 10, 21, 2]; 
scores.sort(); 
// [1, 10, 2, 21]
// 注意10在2之前,
// 因为在 Unicode 指针顺序中"10"在"2"之前

var things = ['word', 'Word', '1 Word', '2 Words'];
things.sort(); 
// ['1 Word', '2 Words', 'Word', 'word']
// 在Unicode中, 数字在大写字母之前,
// 大写字母在小写字母之前.
```

### 语法
``` javascript
arr.sort() 

arr.sort(compareFunction)
```

compareFunction
可选。用来指定按某种顺序进行排列的函数。如果省略，元素按照转换为的字符串的各个字符的Unicode位点进行排序。

sort 方法可以使用 函数表达式 方便地书写：
``` javascript
var numbers = [4, 2, 5, 1, 3];
numbers.sort(function(a, b) {
  return a - b;
});
console.log(numbers);

也可以写成：
var numbers = [4, 2, 5, 1, 3]; 
numbers.sort((a, b) => a - b); 
console.log(numbers);

// [1, 2, 3, 4, 5]
```
### 示例

#### 创建、显示及排序数组
下述示例创建了四个数组，并展示原数组。之后对数组进行排序。对比了数字数组分别指定与不指定比较函数的结果。

``` javascript
var stringArray = ["Blue", "Humpback", "Beluga"];
var numericStringArray = ["80", "9", "700"];
var numberArray = [40, 1, 5, 200];
var mixedNumericArray = ["80", "9", "700", 40, 1, 5, 200];

function compareNumbers(a, b) {
  return a - b;
}

console.log('stringArray:' + stringArray.join());
console.log('Sorted:' + stringArray.sort());

console.log('numberArray:' + numberArray.join());
console.log('Sorted without a compare function:'+ numberArray.sort());
console.log('Sorted with compareNumbers:'+ numberArray.sort(compareNumbers));

console.log('numericStringArray:'+ numericStringArray.join());
console.log('Sorted without a compare function:'+ numericStringArray.sort());
console.log('Sorted with compareNumbers:'+ numericStringArray.sort(compareNumbers));

console.log('mixedNumericArray:'+ mixedNumericArray.join());
console.log('Sorted without a compare function:'+ mixedNumericArray.sort());
console.log('Sorted with compareNumbers:'+ mixedNumericArray.sort(compareNumbers));
该示例的返回结果如下。输出显示，当使用比较函数后，数字数组会按照数字大小排序。

stringArray: Blue,Humpback,Beluga
Sorted: Beluga,Blue,Humpback

numberArray: 40,1,5,200
Sorted without a compare function: 1,200,40,5
Sorted with compareNumbers: 1,5,40,200

numericStringArray: 80,9,700
Sorted without a compare function: 700,80,9
Sorted with compareNumbers: 9,80,700

mixedNumericArray: 80,9,700,40,1,5,200
Sorted without a compare function: 1,200,40,5,700,80,9
Sorted with compareNumbers: 1,5,9,40,80,200,700
```

#### 对非 ASCII 字符排序
当排序非 ASCII 字符的字符串（如包含类似 e, é, è, a, ä 等字符的字符串）。一些非英语语言的字符串需要使用 String.localeCompare。这个函数可以将函数排序到正确的顺序。

``` javascript
var items = ['réservé', 'premier', 'cliché', 'communiqué', 'café', 'adieu'];
items.sort(function (a, b) {
  return a.localeCompare(b);
});

// items is ['adieu', 'café', 'cliché', 'communiqué', 'premier', 'réservé']
```

#### 使用映射改善排序
compareFunction 可能需要对元素做多次映射以实现排序，尤其当 compareFunction 较为复杂，且元素较多的时候，某些 compareFunction 可能会导致很高的负载。使用 map 辅助排序将会是一个好主意。基本思想是首先将数组中的每个元素比较的实际值取出来，排序后再将数组恢复。

``` javascript
var list = ['Delta', 'alpha', 'CHARLIE', 'bravo'];

// 对需要排序的数字和位置的临时存储
var mapped = list.map(function(el, i) {
  return { index: i, value: el.toLowerCase() };
})

// 按照多个值排序数组
mapped.sort(function(a, b) {
  return +(a.value > b.value) || +(a.value === b.value) - 1;
});

// 根据索引得到排序的结果
var result = mapped.map(function(el){
  return list[el.index];
});
```
// 需要被排序的数组

## splice()
方法通过删除现有元素和/或添加新元素来更改一个数组的内容。

``` javascript
var myFish = ['angel', 'clown', 'mandarin', 'sturgeon'];

myFish.splice(2, 0, 'drum'); // 在索引为2的位置插入'drum'
// myFish 变为 ["angel", "clown", "drum", "mandarin", "sturgeon"]

myFish.splice(2, 1); // 从索引为2的位置删除一项（也就是'drum'这一项）
// myFish 变为 ["angel", "clown", "mandarin", "sturgeon"]
```
### 语法
``` javascript
array.splice(start)

array.splice(start, deleteCount) 

array.splice(start, deleteCount, item1, item2, ...)
```

``` javascript
var myFish = ['angel', 'clown', 'trumpet', 'sturgeon'];
var removed = myFish.splice(0, 2, 'parrot', 'anemone', 'blue');
// 运算后的myFish： ["parrot", "anemone", "blue", "trumpet", "sturgeon"] 
// 被删除元素数组：["angel", "clown"]
```

## toLocaleString()
返回一个字符串表示数组中的元素。数组中的元素将使用各自的 toLocaleString 方法转成字符串，这些字符串将使用一个特定语言环境的字符串（例如一个逗号 ","）隔开。

> 数组中的元素将会使用各自的 toLocaleString 方法：

1. Object: Object.prototype.toLocaleString()
1. Number: Number.prototype.toLocaleString()
1. Date: Date.prototype.toLocaleString()

### 使用 toLocaleString

``` javascript
var number = 1337;
var date = new Date();
var myArr = [number, date, "foo"];

var str = myArr.toLocaleString(); 

console.log(str); 
// 输出 "1,337,2017/8/13 下午8:32:24,foo"
// 假定运行在中文（zh-CN）环境，北京时区
```

## toSource() ！
Array.toSource()
## toString()
返回一个字符串，表示指定的数组及其元素。

``` javascript
var monthNames = ['Jan', 'Feb', 'Mar', 'Apr'];
var myVar = monthNames.toString(); // assigns "Jan,Feb,Mar,Apr" to myVar.
```

## unshift()
方法将一个或多个元素添加到数组的开头，并返回新数组的长度。

``` javascript
let a = [1, 2, 3];
a.unshift(4, 5);

console.log(a);
// [4, 5, 1, 2, 3]

var arr = [1, 2];

arr.unshift(0); //result of call is 3, the new array length
//arr is [0, 1, 2]

arr.unshift(-2, -1); // = 5
//arr is [-2, -1, 0, 1, 2]

arr.unshift( [-3] );
//arr is [[-3], -2, -1, 0, 1, 2]
```

## values() ！
PS: Chrome 未实现，Firefox未实现，Edge已实现
方法返回一个新的 Array Iterator 对象，该对象包含数组每个索引的值。


## Array.prototype[@@iterator]()
## Array.unobserve()
## get Array[@@species]

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
  console.log(arrs[i])
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




# 数组数据合并

## dfdf
``` javascript
var a = [
  {
    id: '001',
    name: '张三',
    age: 18,
    address: '北京市朝阳区',
    school: '朝阳区第二中学'
  },
  {
    id: '1002',
    name: '李四',
    age: 15,
    address: '北京市海淀区',
    school: '海淀区第二中学'
  },
  {
    id: '1003',
    name: '王五',
    age: 16,
    address: '北京市石景山区',
    school: '石景山区第二中学'
  }
]
var b = [
  {
    id: '004',
    name: '小毛',
    age: 18,
    address: '北京市朝阳区',
    school: '朝阳区第二中学'
  },
  {
    id: '1003',
    name: '王五',
    age: 16,
    address: '北京市石景山区',
    school: '石景山区第二中学'
  }
]
var c = a.concat(b)
var name = {}
var result = []
c.forEach(function (item, index) {
  if (!name[item.id]) {
    result.push(item)
    name[item.id] = true
  }
})
console.log(result)

[
  {
    id: '001',
    name: '张三',
    age: 18,
    address: '北京市朝阳区',
    school: '朝阳区第二中学'
  },
  {
    id: '1002',
    name: '李四',
    age: 15,
    address: '北京市海淀区',
    school: '海淀区第二中学'
  },
  {
    id: '1003',
    name: '王五',
    age: 16,
    address: '北京市石景山区',
    school: '石景山区第二中学'
  },
  {
    id: '1004',
    name: '小毛',
    age: 18,
    address: '北京市朝阳区',
    school: '朝阳区第二中学'
  }
]
```

## 循环检测

var a = ["2013-01","2013-01","2013-02","2013-02","2013-02","2013-03","2013-03"];  
    Array.prototype.del = function() {   
        var a = {}, c = [], l = this.length;   
        for (var i = 0; i < l; i++) {   
            var b = this[i];   
            var d = (typeof b) + b;   
            if (a[d] === undefined) {   
                c.push(b);   
                a[d] = 1;   
            }   
        }   
        return c;   
    }   
    alert(a.del());  

## 优化遍历数组法
获取没重复的最右一值放入新数组。（检测到有重复值时终止当前循环同时进入顶层循环的下一轮判断）

``` javascript
function repeatFor(array) {
  var n = []
  for (var i = 0, l = array.length; i < l; i++) {
    for (var j = i + 1; j < l; j++) {
      if (array[i] === array[j]) {
        j = ++i
        n.push(array[i])
      }
    }
  }
  return n
}
repeatFor([1, 3, 4, 5, 7, 2, 3])
// [1, 3, 4, 5, 7, 2]
```

## 排序后相邻去除法
给传入数组排序，排序后相同值相邻，然后遍历时新数组只加入不与前一值重复的值。

``` javascript
function repeatNext(array) {
  array.sort()
  var n = [array[0]]
  for (var i = 1; i < array.length; i++) {
    if (array[i] != n[n.length - 1]) {
      n.push(array[i])
    }
  }
  return n
}
repeatNext([1, 3, 4, 5, 7, 2, 3])
// [1, 3, 4, 5, 7, 2]
```

## 数组下标判断法
如果当前数组的第i项在当前数组中第一次出现的位置不是i，那么表示第i项是重复的，忽略掉。否则存入结果数组

``` javascript
function repeatId(array) {
  var n = [array[0]]
  for (var i = 1; i < array.length; i++) {
    if (array.indexOf(array[i]) == i) {
      n.push(array[i])
    }
  }
  return n
}
repeatId([1, 3, 4, 5, 7, 2, 3])
// [1, 3, 4, 5, 7, 2]
```

