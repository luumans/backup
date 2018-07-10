title: JavaScript Date 时间对象
date: 2018-03-13 18:29:00
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
JavaScript提供的时间对象，Date 对象用于处理日期和时间。
<!-- more -->

# 基础
## 创建

初始化日期
``` javascript
new Date()
// 返回当前日期和时间
new Date(dateString)
// 时间字符串
new Date(milliseconds)
// 返回从 1970 年 1 月 1 日至今的毫秒数
new Date(year, month, day, hours, minutes, seconds, milliseconds)
// 默认为0，时间参数
new Date(timestamp)
// 时间戳
```

``` javascript
console.log(new Date())
// Tue Mar 13 2018 15:23:05 GMT+0800 (CST)

console.log(new Date("October 13, 1975 11:13:00"))
// Mon Oct 13 1975 11:13:00 GMT+0800 (CST)

console.log(new Date(79,5,24))
// Sun Jun 24 1979 00:00:00 GMT+0800 (CST)

console.log(new Date(79,5,24,11,33,0))
// Sun Jun 24 1979 11:33:00 GMT+0800 (CST)

console.log(new Date(1453094034000))
// Mon Jan 18 2016 13:13:54 GMT+0800 (CST)
```

## 设置
通过使用针对日期对象的方法，我们可以很容易地对日期进行操作。
``` javascript
var myDate = new Date()
myDate.setFullYear(2010, 0, 14)
```

## 方法

### Getter

#### getDate()
根据本地时间返回指定日期对象的月份中的第几天（1-31）。

``` javascript
console.log(new Date().getDate())
// 13
```

#### getDay()
根据本地时间返回指定日期对象的星期中的第几天（0-6）。

``` javascript
console.log(new Date().getDay())
// 2
```

#### getFullYear()
根据本地时间返回指定日期对象的年份（四位数年份时返回四位数字）。

``` javascript
console.log(new Date().getFullYear())
// 2018
```

#### getHours()
根据本地时间返回指定日期对象的小时（0-23）。

``` javascript
console.log(new Date().getHours())
// 17
```

#### getMilliseconds()
根据本地时间返回指定日期对象的微秒（0-999）。

``` javascript
console.log(new Date().getMilliseconds())
// 662
```

#### getMinutes()
根据本地时间返回指定日期对象的分钟（0-59）。

``` javascript
console.log(new Date().getMinutes())
// 44
```

#### getMonth()
根据本地时间返回指定日期对象的月份（0-11）。

``` javascript
console.log(new Date().getMonth())
// 2
```

#### getSeconds()
根据本地时间返回指定日期对象的秒数（0-59）。

``` javascript
console.log(new Date().getSeconds())
// 9
```

#### getTime()
返回从1970-1-1 00:00:00 UTC（协调世界时）到该日期经过的毫秒数，对于1970-1-1 00:00:00 UTC之前的时间返回负值。

``` javascript
console.log(new Date().getTime())
// 1520934249663
```

#### getTimezoneOffset()
返回当前时区的时区偏移。

``` javascript
console.log(new Date().getTimezoneOffset())
// -480
```

#### getUTCDate()
Returns the day (date) of the month (1-31) in the specified date according to universal time.

``` javascript
console.log(new Date().getUTCDate())
// 13
```

#### getUTCDay()
Returns the day of the week (0-6) in the specified date according to universal time.

``` javascript
console.log(new Date().getUTCDay())
// 2
```

#### getUTCFullYear()
Returns the year (4 digits for 4-digit years) in the specified date according to universal time.

``` javascript
console.log(new Date().getUTCFullYear())
// 2018
```

#### getUTCHours()
Returns the hours (0-23) in the specified date according to universal time.

``` javascript
console.log(new Date().getUTCHours())
// 9
```

#### getUTCMilliseconds()
Returns the milliseconds (0-999) in the specified date according to universal time.

``` javascript
console.log(new Date().getUTCMilliseconds())
// 663
```

#### getUTCMinutes()
Returns the minutes (0-59) in the specified date according to universal time.

``` javascript
console.log(new Date().getUTCMinutes())
// 44
```

#### getUTCMonth()
Returns the month (0-11) in the specified date according to universal time.

``` javascript
console.log(new Date().getUTCMonth())
// 2
```

#### getUTCSeconds()
Returns the seconds (0-59) in the specified date according to universal time.

``` javascript
console.log(new Date().getUTCSeconds())
// 9
```

#### getYear() 
Returns the year (usually 2-3 digits) in the specified date according to local time. Use getFullYear() instead.

``` javascript
console.log(new Date().getYear())
// 118
```

### Setter
返回getTime()

#### setDate()
根据本地时间为指定的日期对象设置月份中的第几天。

``` javascript
console.log(new Date().setDate(2010, 0, 14))
// 1693475120975
```

#### setFullYear()
根据本地时间为指定日期对象设置完整年份（四位数年份是四个数字）。

``` javascript
console.log(new Date().setFullYear(2010, 0, 14))
// 1263462320975
```

#### setHours()
根据本地时间为指定日期对象设置小时数。

``` javascript
console.log(new Date().setHours(2010, 0, 14))
// 1528106414975
```

#### setMilliseconds()
根据本地时间为指定日期对象设置毫秒数。

``` javascript
console.log(new Date().setMilliseconds(2010, 0, 14))
// 1520934322010
```

#### setMinutes()
根据本地时间为指定日期对象设置分钟数。

``` javascript
console.log(new Date().setMinutes(2010, 0, 14))
// 1521052200014
```

#### setMonth()
根据本地时间为指定日期对象设置月份。

``` javascript
console.log(new Date().setMonth(2010, 0, 14))
// 6800406320975
```

#### setSeconds()
根据本地时间为指定日期对象设置秒数。

``` javascript
console.log(new Date().setSeconds(2010, 0, 14))
// 1520936310000
```

#### setTime()
通过指定从 1970-1-1 00:00:00 UTC 开始经过的毫秒数来设置日期对象的时间，对于早于 1970-1-1 00:00:00 UTC的时间可使用负值。

``` javascript
console.log(new Date().setTime(2010, 0, 14))
// 2010
```

#### setUTCDate()
根据世界时设置 Date 对象中月份的一天 (1 ~ 31)。

``` javascript
console.log(new Date().setUTCDate(2010, 0, 14))
// 1693475217527
```

#### setUTCFullYear()
根据世界时设置 Date 对象中的年份（四位数字）。

``` javascript
console.log(new Date().setUTCFullYear(2010, 0, 14))
// 1263462417527
```

#### setUTCHours()
根据世界时设置 Date 对象中的小时 (0 ~ 23)。

``` javascript
console.log(new Date().setUTCHours(2010, 0, 14))
// 1528135214527
```

#### setUTCMilliseconds()
根据世界时设置 Date 对象中的毫秒 (0 ~ 999)。

``` javascript
console.log(new Date().setUTCMilliseconds(2010, 0, 14))
// 1520934419010
```

#### setUTCMinutes()
根据世界时设置 Date 对象中的分钟 (0 ~ 59)。

``` javascript
console.log(new Date().setUTCMinutes(2010, 0, 14))
// 1521052200014
```

#### setUTCMonth()
根据世界时设置 Date 对象中的月份 (0 ~ 11)。

``` javascript
console.log(new Date().setUTCMonth(2010, 0, 14))
// 6800406417528
```

#### setUTCSeconds()
根据世界时设置 Date 对象中的秒钟 (0 ~ 59)。

``` javascript
console.log(new Date().setUTCSeconds(2010, 0, 14))
// 1520936370000
```

#### setYear() 
setYear() 方法用于设置年份。请使用 setFullYear() 方法代替。

``` javascript
console.log(new Date().setYear(2010, 0, 14))
// 1268473617528
```

### Conversion getter

#### toDateString()
以人类易读（human-readable）的形式返回该日期对象日期部分的字符串。

``` javascript
console.log(new Date().toDateString())
```

#### toISOString()
把一个日期转换为符合 ISO 8601 扩展格式的字符串。

``` javascript
console.log(new Date().toISOString())
```

#### toJSON()
使用 toISOString() 返回一个表示该日期的字符串。为了在 JSON.stringify() 方法中使用。

``` javascript
console.log(new Date().toJSON())
```

#### toGMTString() 
返回一个基于 GMT (UT) 时区的字符串来表示该日期。请使用 toUTCString() 方法代替。

``` javascript
console.log(new Date().toGMTString())
```

#### toLocaleDateString()
返回一个表示该日期对象日期部分的字符串，该字符串格式与系统设置的地区关联（locality sensitive）。

``` javascript
console.log(new Date().toLocaleDateString())
```

#### toLocaleFormat() +No
使用格式字符串将日期转换为字符串。

``` javascript
console.log(new Date().toLocaleFormat())
```

#### toLocaleString()
返回一个表示该日期对象的字符串，该字符串与系统设置的地区关联（locality sensitive）。覆盖了 Object.prototype.toLocaleString() 方法。

``` javascript
console.log(new Date().toLocaleString())
```

#### toLocaleTimeString()
返回一个表示该日期对象时间部分的字符串，该字符串格式与系统设置的地区关联（locality sensitive）。

``` javascript
console.log(new Date().toLocaleTimeString())
```

#### toSource()  +No
返回一个与Date等价的原始字符串对象，你可以使用这个值去生成一个新的对象。重写了 Object.prototype.toSource() 这个方法。

``` javascript
console.log(new Date().toSource())
```

#### toString()
返回一个表示该日期对象的字符串。覆盖了Object.prototype.toString() 方法。

``` javascript
console.log(new Date().toString())
```

#### toTimeString()
以人类易读格式返回日期对象时间部分的字符串。

``` javascript
console.log(new Date().toTimeString())
```

#### toUTCString()
把一个日期对象转换为一个以UTC时区计时的字符串。

``` javascript
console.log(new Date().toUTCString())
```

#### valueOf()
返回一个日期对象的原始值。覆盖了 Object.prototype.valueOf() 方法。

``` javascript
console.log(new Date().valueOf())
```

# 例子
## 创建一个日期对象的几种方法

``` javascript
var today = new Date()
var today = new Date(1453094034000)
// by timestamp(accurate to the millimeter)
var birthday = new Date("December 17, 1995 03:24:00")
var birthday = new Date("1995-12-17T03:24:00")
var birthday = new Date(1995,11,17)
var birthday = new Date(1995,11,17,3,24,0)
```

## 计算经过的时间

``` javascript
// 使用 Date 对象
var start = Date.now()

// 这里进行耗时的方法调用: 
doSomethingForALongTime()
var end = Date.now()
var elapsed = end - start
// 运行时间的毫秒值
// 使用内建的创建方法
var start = new Date()

// 这里进行耗时的方法调用: 
doSomethingForALongTime()
var end = new Date()
var elapsed = end.getTime() - start.getTime()
// 运行时间的毫秒值
      
// to test a function and get back its return
function printElapsedTime (fTest) {
    var nStartTime = Date.now(),
        vReturn = fTest(),
        nEndTime = Date.now()
    alert("Elapsed time: " + String(nEndTime - nStartTime) + " milliseconds") 
    return vReturn
}
yourFunctionReturn = printElapsedTime(yourFunction)
```

## 浏览器兼容问题
一般 直接new Date() 是不会出现兼容性问题的，而 new Date(datetimeformatstring) 常常会出现浏览器兼容性问题，为什么，datetimeformatstring中的某些格式浏览器不兼容。

``` javascript
new Date()
// 火狐
Date 2018-03-13T10:15:30.979Z
UTC 时间
// Safari
Tue Mar 13 2018 18:15:30 GMT+0800 (CST)
// Chrome
Tue Mar 13 2018 18:15:30 GMT+0800 (CST)
```

### ISO 8601
日期格式：YYYY-MM-DDThh:mm:ss.sTZD

``` javascript
console.log(new Date("2014-06-12T09:08:36"))
// 用户本地时间
console.log(new Date("2014-06-12T01:08:36Z"))
// UTC 世界标准时间 Coordinated Universal Time
console.log(new Date("2014-06-12T09:08:36 + 08:00"))
// specific offset 时差（时区）

// Safari
Thu Jun 12 2014 17:08:36 GMT+0800 (CST)
Thu Jun 12 2014 09:08:36 GMT+0800 (CST)
Thu Jun 12 2014 09:08:36 GMT+0800 (CST)

// 火狐
Date 2014-06-12T01:08:36.000Z
Date 2014-06-12T01:08:36.000Z
Date 2014-06-12T01:08:36.000Z

// Chrome
Thu Jun 12 2014 09:08:36 GMT+0800 (CST)
Thu Jun 12 2014 09:08:36 GMT+0800 (CST)
Thu Jun 12 2014 09:08:36 GMT+0800 (CST)
```

由此可以看出，火狐返回的时间为UTC时间，与本地时间有8小时的时差。

在 ECMA-262 v 5.1 的第 15.9.1.15 章节：[ECMA-262](http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15 "")

在 ECMA-262 v 6.0 的第 20.3.1.16 章节：[ECMA-262](http://www.ecma-international.org/ecma-262/6.0/index.html#sec-date-time-string-format "")

> 关于JavaScript的new Date一个奇怪的日期在Firefox和chrome的不同表现?

``` javascript
new Date("2017-01-10T15:59:01.568").getTime() === 1484035141568

// Safari
false

// 火狐
true

// Chrome
true
```








