title: 前端网站SEO执行策略
date: 2018-03-27 19:29:00
description: 随着移动设备的普及以及微信庞大的用户量，移动端的需求也随之爆发式增长，平时我们使用 Chrome 进行手机模拟页面开发，但模拟终究是模拟，不可避免的还是需要真机调试，下面就来讲讲几种调试方案，希望能对你有所帮助。
categories:
- Chrome
tags:
- Chrome
toc: true
author: Luuman
comments:
original:
permalink: 
---
随着移动设备的普及以及微信庞大的用户量，移动端的需求也随之爆发式增长，平时我们使用 Chrome 进行手机模拟页面开发，但模拟终究是模拟，不可避免的还是需要真机调试，下面就来讲讲几种调试方案，希望能对你有所帮助。
<!-- more -->



# iPhone

## iPhone + Safari
使用 Lightning 数据线将 iPhone 与 Mac 相连
### iPhone（iOS 6 +）
1. iPhone 开启 Web 检查器（设置 -> Safari -> 高级 -> 开启 Web 检查器）
1. iPhone 使用 Safari 浏览器打开要调试的页面

### Safari
1. Mac 打开 Safari 浏览器调试（菜单栏 —> 开发 -> iPhone 设备名 -> 选择调试页面）
1. 如果你的菜单栏没有“开发”选项，可以到左上角 Safari -> 偏好设置 -> 高级 -> 在菜单栏中显示“开发”菜单。

# Android
## Android + Chrome
使用 USB 数据线将手机与电脑相连
### Android （4.0 +）
手机进入开发者模式，勾选 USB 调试，并允许调试

### Chrome
1. 电脑打开 Chrome 浏览器，在地址栏输入：chrome://inspect/#devices 并勾选 Discover USB devices 选项

<!-- https://juejin.im/entry/58b7b35c570c350062028e02 -->