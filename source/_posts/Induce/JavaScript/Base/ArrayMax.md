title: JavaScript 数组方法汇总
date: 2018-03-25 18:29:00
description:
categories:
- JavaScript
tags:
- Array
toc: true
author:
comments:
original:
permalink:
---

# 数组最大值
## es6拓展运算符...
``` javascript
var arr = [-1, 1, 101, -52, 10, 1001, 1001]
Math.max(...arr)
```

## es5 apply(与方法1原理相同)
``` javascript
var arr = [-1, 1, 101, -52, 10, 1001, 1001]
Math.max.apply(null,arr)
```
<!-- more -->

## for循环
``` javascript
var arr = [-1, 1, 101, -52, 10, 1001, 1001]
let max = arr[0];
for (let i = 0; i < arr.length - 1; i++) {
  max = arr[i] < arr[i+1] ? arr[i+1] : arr[i]
}
```

## 数组sort()
``` javascript
var arr = [-1, 1, 101, -52, 10, 1001, 1001]
arr.sort((num1, num2) => {
  return num1 - num2 < 0
})
arr[0]
```

## 数组reduce
``` javascript
var arr = [-1, 1, 101, -52, 10, 1001, 1001]
arr.reduce((num1, num2) => {
  return num1 > num2 ? num1 : num2}
)
```

# 数组去重

## ES6的set

``` javascript
var arr = [-1, 1, 101, -52, 10, 1001, 1001]
console.log(Array.from(new Set(arr)))

var arr = [-1, 1, 101, -52, 10, 1001, 1001]
console.log([...new Set(arr)])
```

## 过滤push

``` javascript
```
Array.prototype.distinct = function () {
  var arr = this, result = [], len = arr.length
  for (i = 0; i < len; i++) {
    for (j = i + 1; j < len; j++) {
      if (arr[i] === arr[j]) {
        j = ++i
      }
    }
    result.push(arr[i])
  }
  return result
}
var arra = [1, 2, 3, 4, 4, 1, 1, 2, 1, 1, 1]
arra.distinct()
// [3, 4, 2, 1]

## 过滤splice
优点：简单易懂
缺点：占用内存高，速度慢

``` javascript
Array.prototype.distinct = function () {
  var arr = this, len = arr.length
  for (i = 0; i < len; i++) {
    for (j = i + 1; j < len; j++) {
      if (arr[i] === arr[j]) {
        arr.splice(j, 1)
        len--
        j--
      }
    }
  }
  return arr
}
var arra = [1, 2, 3, 4, 4, 1, 1, 2, 1, 1, 1]
arra.distinct()
// [1, 2, 3, 4]
```


## 利用对象过滤
``` javascript
Array.prototype.distinct = function () {
  var arr = this, obj = {}, result = [], len = arr.length
  for (i = 0; i < len; i++) {
    if (!obj[arr[i]]) {
      obj[arr[i]] = true;
      result.push(arr[i])
    }
  }
  return result
}
var arra = [1, 2, 3, 4, 4, 1, 1, 2, 1, 1, 1]
arra.distinct()
// [1, 2, 3, 4]
```

## 数组递归去重
运用递归的思想，先排序，然后从最后开始比较，遇到相同，则删除

``` javascript
Array.prototype.distinct = function () {
  var arr = this, len = arr.length
  arr.sort(function(a, b) {
    return a - b
  })
  function loop(index) {
    if (index >= 1) {
      if (arr[index] === arr[index - 1]) {
        arr.splice(index, 1)
      }
      loop(index - 1)
    }
  }
  loop(len - 1)
  return arr
}
var arra = [1, 2, 3, 4, 4, 1, 1, 2, 1, 1, 1]
arra.distinct()
```

## 利用indexOf以及forEach

``` javascript
Array.prototype.distinct = function () {
  var arr = this, result = [], len = arr.length
  arr.forEach(function (v, i, arr) {
    if (arr.indexOf(v, i+1) === -1) {
      result.push(v)
    }
  })
  return result
}
var arra = [1, 2, 3, 4, 4, 1, 1, 2, 1, 1, 1]
arra.distinct()
```

# 数组合并去重

## concat()方法
``` javascript
var a = [1, 2, 3];
var b = [4, 5, 2];
function concatArray(a, b) {
	a = a.concat(b)
	a = a.distinct()
	return a
}
concatArray(a, b)
// [1, 3, 4, 5, 2]
```

## apply()方法
``` javascript
var a = [1, 2, 3];
var b = [4, 5, 2];
function concatArray(a, b) {
	Array.prototype.push.apply(a, b)
	a = a.distinct()
	return a
}
concatArray(a, b)
// [1, 3, 4, 5, 2]
```


