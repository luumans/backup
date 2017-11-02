title: 浏览器常见Bug——Canvas
date: 2017-07-12 18:29:00
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


# 生成图片Canvas
## toDataURL
> Uncaught (in promise) DOMException: Failed to execute 'toDataURL' on 'HTMLCanvasElement': Tainted canvases may not be exported.

问题原因：
Canvas为了安全性考虑，当绘制了外部图片后它会变成只可写不可读的状态，getImageData、toDataURL之类的试图读取数据的方法全都无法使用。理论上开启了CORS的资源应该被允许读取，只是IMG元素发起的请求默认并不带Origin字段，没能应用上CORS。

> request Headers请求头Origin:

origin主要是用来说明最初请求是从哪里发起的；
origin只用于Post请求，而Referer则用于所有类型的请求；
origin的方式比Referer更安全点吧。
- [基于 canvas 实现的一个截图小 demo](https://github.com/Aaaaaaaty/Blog/issues/5)
- [CORS与Canvas图片toDataURL](https://www.web-tinker.com/article/20687.html "")

## Access-Control-Allow-Origin
> Access to Image at 'http://wx4.sinaimg.cn/mw690/4b4d632fgy1fieo66xwy4j20io0goq46.jpg' from origin 'http://172.16.20.115:8780' has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource. Origin 'http://172.16.20.115:8780' is therefore not allowed access.

CORS解决


[前端实现'截图'效果的几种方式](https://www.w3ctrain.com/2017/07/24/gen-image-fe/)


[html2canvas html截图插件图片放大清晰度解决方案，支持任意放大倍数，解决原插件图片偏移问题](https://juejin.im/entry/59ae0e4c5188252423586470?utm_source=gold_browser_extension)
[html2canvas 将代码转为图片](https://www.h5jun.com/post/convert-code-to-image-via-html2canvas.html "")