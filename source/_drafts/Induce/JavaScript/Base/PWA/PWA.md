title: PWA简介 - 下一代 Web 应用？
date: 2017-12-25 18:29:00
description:
categories:
- PWA
tags:
- PWA
toc: true
author:
comments:
original:
permalink:

---
PWA做为一门Google推出的WEB端的新技术，好处不言而喻，但目前对于相关方面的知识不是很丰富，这里我推出一下这方面的入门教程系列，提供PWA方面学习。
<!-- more -->
# 前言

## Web 与原生应用

Mobile Web | Apps
-------|------
13% | 87%

- [如何看待 Progressive Web Apps 的发展前景？](https://www.zhihu.com/question/46690207 "")
- [google I/O 2016](https://www.youtube.com/watch?v=cmGr0RszHc8 "Progressive Web Apps")
- []( "")
- [下一代 Web 应用模型 — Progressive Web App](https://zhuanlan.zhihu.com/p/25167289 "")
- [改造你的网站，变身 PWA](https://segmentfault.com/a/1190000008880637 "")
- []( "")
- [PWA 入门: 写个非常简单的 PWA 页面](https://zhuanlan.zhihu.com/p/25459319 "")
- [An minimal example for PWA](https://github.com/minimal-xyz/minimal-pwa "PWA 入门: 写个非常简单的 PWA 页面")
- [你的首个 Progressive Web App](http://blog.csdn.net/liangyihuai/article/details/54948153 "")
- [认识Progressive Web App](http://blog.csdn.net/zdyueguanyun/article/details/54907002 "")
- []( "")
- []( "")

/Users/luuman/Coding/Index/minimal-pwa/sw.js
/Users/luuman/Coding/Gulp/ServiceWorker/dev/sw-demo-cache.js

applicationCache
/Users/luuman/Coding/Index/Game/applicationCache/app.manifest
## 兼容性
目前基本只有chrome支持。


# PWA

PWA全称Progressive Web App，直译是渐进式WEB应用。
是 Google 在 2015 年提出，2016年6月才推广的项目。是结合了一系列现代Web技术的组合，在网页应用中实现和原生应用相近的用户体验。

所谓的P（Progressive）这里有两层含义，一方面是渐进增强，让WEB APP的体验和功能能够用渐进增强的方式来更接近原生APP的体验及功能；另一方面是指下一代WEB技术，PWA并不是描述一个技术，而是一些技术的合集。



# Push Notification 推送通知

Push 和 Notification是两个不同的功能，涉及到两个API，但是它们之前有依赖关系。
