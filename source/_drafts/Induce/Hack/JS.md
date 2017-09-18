title: 浏览器常见Bug——JS
date: 2017-01-02 18:29:00
description:
categories:
- Induce
tags:
- Hack
toc: true
author:
comments:
original:
permalink:
---

　　**自用笔记：**本文属于自用笔记，不做详解，仅供参考。在此记录自己已理解并开始遵循的前端代码规范。What How Why
<!-- more -->

# 时间兼容问题

```
通过字符串创建Date对象时各个浏览器可能得到不同的值，如 

let date = new Date('2017-5-1');
console.log(date);
// IE 11 ==> Invalid Date
// Chrome ==> Mon May 01 2017 00:00:00 GMT+0800 (中国标准时间)
let date = new Date('2017-05-01');
console.log(date);
// IE 11 ==> Mon May 01 2017 08:00:00 GMT+0800 (中国标准时间)
// Chrome ==> Mon May 01 2017 00:00:00 GMT+0800 (中国标准时间)
可以看到这两种情况IE和Chrome输出都是不同的，因此应该避免使用这种方法，而改用：

let date = new Date(2017,4,1,0,0,0,0);
// 参数分别为年 月 日 时 分 秒 毫秒，均为整型
```


