title: JavaScript 保存页面为图片
date: 2018-02-27 18:29:00
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
众所周知，WEB设备鱼龙混杂。兼容性是最为头痛的问题！今天我们想用canvas保存HTML页面为图片，踩坑之旅由此开始。
<!-- more -->

# 需求
1. 将HTML页面导出Canvas
1. 将Canvas保存到本地

# 方案

1. [html2canvas.js](https://github.com/niklasvh/html2canvas "")：可将 htmldom 转为 canvas 元素。
1. canvasAPI：toDataUrl() 可将 canvas 转为 base64 格式
1. 创建 a[download] 标签触发 click 事件实现下载

## 二维码
### qrcode.min.js
``` javascript
let qrcode = new window.QRCode(document.getElementById('qrcode'), {
  text: 'http:// jindo.dev.naver.com/collie',
  width: 428,
  height: 428,
  colorDark: '#000',
  colorLight: '#FFF',
  correctLevel: window.QRCode.CorrectLevel.H
})
```

## html2canvas
js将遍历加载页面的`DOM`节点，收集所有元素的信息，然后用这些信息来呈现页面。换句话说，实际上这个库并不是真的对页面进行截图，而是基于从 DOM 读取的元素及属性来一点点的绘制 canvas。

注意：
它只能正确地呈现它理解的元素和属性，这意味着有许多 CSS 属性不起作用。
目前官方提供的版本有很多，正式版本是v0.4.1 - 7.9.2013，最新版本是v0.5.0-beta4，那对于我们开发来说如果不是玩新特性什么的一般还是会选择正式版，结果第一个坑就掉进去爬了半天。

### 原理
所以基本可以猜到整个工作流程应该是：

1. 通过获取element，递归处理每个节点，记录这个节点应该怎么画。（比如div就画边框和背景，文字就画文字等等）
1. 考虑节点的层级问题。比如很多布局相关样式属性： z-index、float、position 等的影响。
1. 从低层级开始画到 canvas 上，逐渐向上画。层级高的覆盖层级低的（和浏览器本身的渲染流程很像）。


### v0.4.1
``` javascript
html2canvas(element, {
  onrendered: function(canvas) {
    // 现在你已经拿到了canvas DOM元素    
  }
});
```
创建Canvas
``` javascript
export const addCanvas = (DivId, scale, callback) => {
  let w = document.getElementById(DivId).scrollWidth
  let h = document.getElementById(DivId).scrollHeight
  let canvas = document.createElement('canvas')
  let context = canvas.getContext('2d')
  canvas.width = w * scale
  canvas.height = h * scale
  canvas.style.width = w + 'px'
  canvas.style.height = h + 'px'
  context.scale(scale, scale)
  return canvas
}
```

``` javascript
let canvas = addCanvas(DivId, scale, callback)
window.html2canvas(document.getElementById(DivId), {
  canvas: canvas,
  onrendered: function (canvas) {
    let image = canvas.toDataURL('image/png')
    callback(image)
  }
})
```

#### 问题
##### 图片模糊
因为开发的时候是用 chrome 模拟器生成 canvas 后没有发现有模糊的地方，但是用 PC 代理手机请求开发资源时,发现画面的模糊感非常明显。
容易想到，可能是移动端像素密度计算的问题。

解决办法：addCanvas
简单粗暴将，Canvas放大，通过更大的图片压缩提高分辨率。

##### 图片无法超过屏幕大小
DivId的CSS样式要设置position: absolute;

##### 不支持圆角
border-radius: 50%;

##### 虚线

##### 图片不显示
图片资源跨域问题
``` javascript
/**
 * 图片转base64格式
 */
img2base64(url, crossOrigin) {
  return new Promise(resolve => {
    const img = new Image();
    img.onload = () => {
      const c = document.createElement('canvas');
      c.width = img.naturalWidth;
      c.height = img.naturalHeight;
      const cxt = c.getContext('2d');
      cxt.drawImage(img, 0, 0);
      // 得到图片的base64编码数据
      resolve(c.toDataURL('image/png'));
    };
    crossOrigin && img.setAttribute('crossOrigin', crossOrigin);
    img.src = url;
  });
}
```

``` javascript
优化使用Promise
/**
 * 在本地进行文件保存
 * @param  {String} imgurl     图片链接
 * @param  {String} filename 文件名
 */
export const getBase64Image = (imgurl) => {
  let image = new Image()
  let isLoad = false
  image.crossOrigin = ''
  image.src = imgurl
  image.onload = function () {
    isLoad = true
  }
  if (isLoad) {
    // console.log(isLoad)
    return getCanvas(image)
  } else {
    // console.log(isLoad)
  }
}
```

### v0.5.0
``` javascript
html2canvas(element, options).then(canvas => {
  // 现在你已经拿到了canvas DOM元素    
});
```

### 兼容性
Firefox 3.5+
Google Chrome
Opera 12+
IE9+
Safari 6+


[implementation of screenshot feedback](http://tech.colla.me/en/show/screenshot_feedback_implementation "")
[一次 H5 「保存页面为图片」 的踩坑之旅](https://juejin.im/post/5a17c5e26fb9a04527254689 "")
