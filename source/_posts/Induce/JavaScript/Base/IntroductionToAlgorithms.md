title: JavaScript 算法初探
date: 2018-03-25 18:29:00
description:
categories:
- JavaScript
tags:
- Algorithms
toc: true
author:
comments:
original:
permalink:
---
https://www.jianshu.com/p/1b4068ccd505
http://louiszhai.github.io/2016/12/23/sort/

<!-- more -->

# 排序算法
## 冒泡排序

## 基数排序
![](https://user-gold-cdn.xitu.io/2017/3/16/150ead5b0c6e78b2b900709bc56d4f42.gif?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)
双重多循环，按位循环匹配
``` javascript
function radixSort(arr, maxDigit) {
	var counter = []
  var mod = 10
  var dev = 1
  for (var i = 0; i < maxDigit; i++, dev *= 10, mod *= 10) {
    for(var j = 0; j < arr.length; j++) {
      var bucket = ~~((arr[j] % mod) / dev)
      if(counter[bucket]==null) {
        counter[bucket] = []
      }
      counter[bucket].push(arr[j])
    }
    var pos = 0
    for(var j = 0; j < counter.length; j++) {
      var value = null
      if(counter[j]!=null) {
        while ((value = counter[j].shift()) != null) {
          arr[pos++] = value
        }
		  }
    }
  }
  return arr;
}
var sorts = [23, 10000000, 31, 11, 1, 2, 4]
radixSort(sorts, 8)
```

### 优化

``` javascript
function radixSort(arr) {
	var counter = []
	var maxDigit = Math.max(...arr).toString().length
	for (var i = 0; i < maxDigit; i++) {
		var reg = RegExp("^\\d*(\\d)\\d{" + i + "}$")
		arr.forEach((item) => {
			var bucket = 0
			item.toString().replace(reg, ($0, $1) => {
				bucket = $1
			})
			if (counter[bucket] == undefined) counter[bucket] = []
			counter[bucket].push(item)
		})
		var pos = 0
		counter.forEach((item) => {
			var value = null
			if (item != null) {
				while ((value = item.shift()) != null) {
					arr[pos++] = value
				}
			}
		})
	}
	return arr
}
var sorts = [23, 10000000, 31, 11, 1, 2, 4]
radixSort(sorts, 8)
```
通过正则判断不同位置的数值，默认为0。

各种排序性能对比如下:

| 排序类型 | 平均情况 | 最好情况 | 最坏情况 | 辅助空间 | 稳定性 |
| --- | --- | --- | --- | --- | --- |
| 冒泡排序 | O(n²) | O(n) | O(n²) | O(1) | 稳定 |
| 选择排序 | O(n²) | O(n²) | O(n²) | O(1) | 不稳定 |
| 直接插入排序 | O(n²) | O(n) | O(n²) | O(1) | 稳定 |
| 折半插入排序 | O(n²) | O(n) | O(n²) | O(1) | 稳定 |
| 希尔排序 | O(n^1.3) | O(nlogn) | O(n²) | O(1) | 不稳定 |
| 归并排序 | O(nlog₂n) | O(nlog₂n) | O(nlog₂n) | O(n) | 稳定 |
| 快速排序 | O(nlog₂n) | O(nlog₂n) | O(n²) | O(nlog₂n) | 不稳定 |
| 堆排序 | O(nlog₂n) | O(nlog₂n) | O(nlog₂n) | O(1) | 不稳定 |
| 计数排序 | O(n+k) | O(n+k) | O(n+k) | O(k) | 稳定 |
| 桶排序 | O(n+k) | O(n+k) | O(n²) | O(n+k) | (不)稳定 |
| 基数排序 | O(d(n+k)) | O(d(n+k)) | O(d(n+kd)) | O(n+kd) | 稳定 |



