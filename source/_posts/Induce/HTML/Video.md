title: Mobile Video
date: 2017-03-25 18:29:00
description: 
categories:
- HTML
tags:
- Video
toc: true
author:
comments:
original:
permalink: 
---
　　**自用笔记：**今天我们就来说一说，移动端视频Video的使用兼容问题。略测试了一下，移动端是个重灾区。
<!-- more -->
# 基础知识

属性  | 描述
------------- | -------------
autoplay  | 自动开始播放，不会停下来等着数据载入结束。
preload  | 视频预加载
controls  | 出现控制条
loop  | 循环播放
src  | 视频URL
poster  | 用于在用户播放或者跳帧之前展示
width  | 指定视频宽度（通常在css中指定）
height  | 指定视频高度（通常在css中指定）
buffered  | 读取到哪段时间范围内的媒体被缓存了
height  | 


属性  | 描述
------------- | -------------
play | 视频开始播放触发的事件（触发此事件，但是视频不一定可以播放）
playing | 视频可以播放触发的事件
timeupdate | 音频/视频（audio/video）的播放位置发生改变时触发
pause | 视频停止播放触发的事件
ended | 视频播放结束或中断触发的事件
durationchange
progress

# 监听事件

## ended
```
this.$refs.video.addEventListener('ended', () => {
  console.log('ended')
})
```

# 兼容问题

## 视频截图
没有设置poster时，由于移动端设备多样，部分浏览器不支持

## 是否显示播放按钮
移动端

## Android微信端使用的是微信自带的播放器插件
x5-video-player-type="h5"，播放时不适用微信的播放器。

## iOS全屏播放
没有添加playsinline【webkit-playsinline】，点击播放会弹出iphone自带的播放器，全屏播放。

# 扩展阅读
- [mozilla <video>](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/video "")
- [HTML的媒体支持:audio和video元素](https://developer.mozilla.org/en-US/docs/Web/HTML/Supported_media_formats "")
- []( "")
- [html5--移动端视频video的android兼容，去除播放控件、全屏等](https://segmentfault.com/a/1190000006857675#articleHeader7 "")