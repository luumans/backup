title: 移动端Iphone手机audio自动播放
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
**自用笔记：**由于Iphone设备的安全问题，默认音频自动播放事不会自动播放。需要触发事件，才可以播放。
<!-- more -->

# 微信
``` javascript
<audio id="media" class="audio" loop="" src="http://wx.qimi.com/html/1210/161128164201/resource/assets/mp3_9cb1475.mp3" autoplay="" preload=""></audio>

document.addEventListener('WeixinJSBridgeReady', function () { 
  document.getElementById('media').play();
}, false);
```

# safer

由于 iOS Safari 限制不允许 audio autoplay, 必须用户主动交互(例如 click)后才能播放 audio, 因此我们通过一个用户交互事件来主动 play 一下 audio.

这个坑相信大家都已经踩过了, 在 iOS 9 没出现以前, 这样的 hack 方案还是妥妥的.
但 iOS 9 出现后, 发现这个方案"失效"了.

没有办法, 看来是时候升级一下 hack 方案了, 于是仔细看了下 audio 的事件.

对于能够自动播放时事件的顺序如下
loadstart -> loadedmetadata -> loadeddata -> canplay -> play -> playing

对于不能自动播放时触发的事件因系统版本不同而不同
* iPhone5 iOS 7.0.6 loadstart
* iPhone6s iOS 9.1 loadstart -> loadedmetadata -> loadeddata -> canplay

最终发现相比原来的 hack 方案, 对于 iOS 9 还需要额外的 load 一下, 否则直接 play 不能让 audio 开始播放.
    audioEl.load(); // iOS 9
    audioEl.play(); // iOS 7/8 仅需要 play 一下


``` javascript
;(function() {
	// 微信自动播放
  var audioEl = document.getElementById('media');
	document.addEventListener("WeixinJSBridgeReady", function () { 
    audioEl.play();
	}, false);
  function log(info) {
    // console.log(info);
  }
  function forceSafariPlayAudio() {
		log('forceSafariPlayAudio')
    audioEl.load();
    // iOS 9  还需要额外的 load 一下, 否则直接 play 无效
    audioEl.play();
    // iOS 7/8 仅需要 play 一下
  }
  // 可以自动播放时正确的事件顺序是
  // loadstart -> loadedmetadata -> loadeddata -> canplay -> play -> playing
  // 不能自动播放时触发的事件是
  // iPhone5 iOS 7.0.6 loadstart
  // iPhone6s/7/8P iOS 9.1  loadstart -> loadedmetadata -> loadeddata -> canplay -> forceSafariPlayAudio
  audioEl.addEventListener('loadstart', function() {
    log('loadstart');
  }, false);
  audioEl.addEventListener('loadeddata', function() {
    log('loadeddata');
  }, false);
  audioEl.addEventListener('loadedmetadata', function() {
    log('loadedmetadata');
  }, false);
  audioEl.addEventListener('canplay', function() {
    log('canplay');
  }, false);
  audioEl.addEventListener('play', function() {
    log('play');
    // 当 audio 能够播放后, 移除这个事件
    window.removeEventListener('touchstart', forceSafariPlayAudio, false);
  }, false);
  audioEl.addEventListener('playing', function() {
    log('playing');
  }, false);
  audioEl.addEventListener('pause', function() {
    log('pause');
  }, false);
  // 由于 iOS Safari 限制不允许 audio autoplay, 必须用户主动交互(例如 click)后才能播放 audio,
  // 因此我们通过一个用户交互事件来主动 play 一下 audio.
  window.addEventListener('touchstart', forceSafariPlayAudio, false);
  // audioEl.src = '';
})();
```
``` javascript
;(function() {
  var audioEl = document.getElementById('media');
  function forceSafariPlayAudio() {
		log('forceSafariPlayAudio')
    audioEl.load();
    audioEl.play();
  }
  audioEl.addEventListener('play', function() {
    window.removeEventListener('touchstart', forceSafariPlayAudio, false);
  }, false);
  window.addEventListener('touchstart', forceSafariPlayAudio, false);
})();
```

<!-- https://www.cnblogs.com/interdrp/p/4211883.html -->
