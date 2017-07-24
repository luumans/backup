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

| 值 | 描述 |
| -----|:---- |
| none | 默认。定义带有小写字母和大写字母的标准的文本。|
| capitalize | 文本中的每个单词以大写字母开头。|
| uppercase | 定义仅有大写字母。|
| lowercase | 定义无大写字母，仅有小写字母。|
| inherit | 规定应该从父元素继承 text-transform 属性的值。|

## will-change

[提高页面滚动、动画等渲染性能](http://www.zhangxinxu.com/wordpress/2015/11/css3-will-change-improve-paint/ "")


## 元素可以点透
```
pointer-events: none;
```

## 移动端手机input输入内容自动移动
该效果只限于IOS，ando
```
filter: blur(-3px);
```

## -webkit-text-size-adjust(失效)

1. 当样式表里font-size<12px时，中文版chrome浏览器里字体显示仍为12px，这时可以用 html{-webkit-text-size-adjust:none;}
1. -webkit-text-size-adjust放在body上会导致页面缩放失效
1. body会继承定义在html的样式
1. 用-webkit-text-size-adjust不要定义成可继承的或全局的

```
-webkit-text-size-adjust: none;
```

以显示 10px 的字为例
```
.some-small-font {
    display: inline-block; /* Or block */
    font-size: 12.5px;
    -webkit-transform: scale(0.8);
    transform: scale(0.8);
    position: relative;
    left: -12.5%;
    width: 125%;
}
```

## 输入框选择时无边框
```
outline: none;
```

## 可点击的元素时，覆盖显示的高亮颜色
```
-webkit-tap-highlight-color: rgba(0,0,0,0);
```

## 修改chrome记住密码后自动填充表单的背景颜色
```
input:-webkit-autofill, textarea:-webkit-autofill, select-webkit-autofill{
	background-color: #FFF;
	background-image: none;
	color: #000;
}
```

## 弹窗背景模糊
原理：使用高斯模糊，使得页面显示元素模糊，将样式加在body上，通过body的class实现的。row为指定要模糊的内容
```
body {
	-webkit-backface-visibility: hidden;
}
.modal-active .row {
	-webkit-filter: blur(3px);
	-moz-filter: blur(3px);
	-o-filter: blur(3px);
	-ms-filter: blur(3px);
	filter: blur(3px);
}
```
> - [CSS3 filter 模糊滤镜](http://mao.li/css3-blur-filter-pratice/ "描述")
> - [如何将网页CSS背景图高斯模糊且全屏显示](https://segmentfault.com/q/1010000000123341 "描述")


## 微信二维码无法识别
> - [微信内置浏览器 长按识别二维码 功能的两三个坑与解决方案](https://segmentfault.com/a/1190000002985815 "中国城投票活动页面")
> - [前端页面中 iOS 版微信长按识别二维码的bug 与解决](https://devework.com/weixin-qrcode-bug.html "描述")
网页中使用fixed，ios扫描会偏移到网页下部。

```
padding: size(240) 0 0 size(240) !important;
margin: size(-240) 0 0 size(-240) !important;
position: relative;z-index: 100;
-webkit-user-select: none;
```

## 背景bg设置
background: linear-gradient()

## 安卓微信overflow失效

## CSS clip-path
http://www.cnblogs.com/coco1s/p/6992177.html

## Web移动端Fixed布局的解决方案
http://efe.baidu.com/blog/mobile-fixed-layout/