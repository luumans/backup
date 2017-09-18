title: 浏览器常见Bug——CSS
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

# 微信二维码无法识别
> - [微信内置浏览器 长按识别二维码 功能的两三个坑与解决方案](https://segmentfault.com/a/1190000002985815 "中国城投票活动页面")
> - [前端页面中 iOS 版微信长按识别二维码的bug 与解决](https://devework.com/weixin-qrcode-bug.html "描述")
网页中使用fixed，ios扫描会偏移到网页下部。

```
padding: 240px 0 0 240px !important;
margin: -240px 0 0 -240px !important;
position: relative;z-index: 100;
-webkit-user-select: none;
```

