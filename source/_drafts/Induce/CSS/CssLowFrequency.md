title: CSS低频属性
date: 2017-06-02 18:29:00
description:
categories:
- Induce
tags:
- CSS
toc: true
author:
comments:
original:
permalink:
---

　　**自用笔记：**本文属于自用笔记，不做详解，仅供参考。在此记录自己已理解并开始遵循的前端代码规范。What How Why
<!-- more -->
# 概况

## 文字省略
通过CSS判断，这个区域宽度
### 省略
```
<!-- 单行文本溢出 -->
text-overflow: ellipsis;
white-space: nowrap;
overflow: hidden;
```

### 一行省略
```
<!-- 多行文本溢出 -->
display: -webkit-box !important;
overflow: hidden;
text-overflow: ellipsis;
word-break: break-all;
-webkit-box-orient: vertical;
-webkit-line-clamp: 2;
```

## 首字母大写
```
text-transform: capitalize;
```

## 输入框选择时无边框
```
outline: none;
```

## 