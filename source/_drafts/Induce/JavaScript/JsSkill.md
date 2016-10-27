title: 原生JS技巧
date: 2016-02-27 18:29:00
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

　　**自用笔记：**本文属于自用笔记，不做详解，仅供参考。在此记录自己已理解并开始遵循的前端代码规范。What How Why

<!-- more -->

## 字符串截取

```
sum = 'localhost:3000/#page-4';
var sum = sum.substring(sum.indexOf('_') + 1, sum.length) - 0;
=> 4
```

## 获取标签指定属性值

```
this.getAttribute('data-url');
```