title: 浏览器UA
date: 2017-02-27 18:29:00
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
## Navigator
对象包含有关浏览器的信息

### userAgent
返回由客户机发送服务器的 user-agent 头部的值
```
> navigator.userAgent

"Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/56.0.2924.87 Safari/537.36"
```
function parseUA(){
  let u = navigator.userAgent.toLowerCase() || window.navigator.userAgent.toLowerCase()
  return {
    ios: !!u.match(/\(i[^;]+;( U;)? CPU.+Mac OS X/),
    android: u.indexOf('Android') > -1 || u.indexOf('Linux') > -1,
    Mobile: /(Mobile)/i.test(u),
    MobileAll: u.indexOf('Android') > -1 || u.indexOf('iPhone') > -1 || u.indexOf('SymbianOS') > -1 || u.indexOf('Windows Phone') > -1 || u.indexOf('iPad') > -1 || u.indexOf('iPod') > -1,
    wPhone: /(Windows Phone|windows[\s+]phone)/i.test(u),
    PC: u.indexOf('Win') > -1 || u.indexOf('Mac') > -1 || u.indexOf('Linux') > -1,
    weixin: u.indexOf('MicroMessenger') > -1,
    ykly: u.indexOf('ykly') > -1,
    yIos: u.indexOf('ykly_ios_app') > -1,
    yAndroid: u.indexOf('ykly_android_app') > -1
  }
}
var ua = parseUA();
console.log(ua)
```

### 判断浏览器为移动端
```
<script type="text/javascript">
    browserRedirect();  
    function browserRedirect(){
        var sUA = navigator.userAgent.toLowerCase();
        var bIsIpad = sUA.match(/ipad/i) == 'ipad';
        var bIsIphoneOs =  sUA.match(/iphone os/i) == 'iphone os';
        var bIsMidp = sUA.match(/midp/i) == 'midp';
        var bIsUc7 = sUA.match(/rv:1.2.3.4/i) == 'rv:1.2.3.4';
        var bIsUc = sUA.match(/ucweb/i) == 'ucweb';
        var bIsAndroid = sUA.match(/android/i) == 'android';
        var bIsCE = sUA.match(/windows ce/i) == 'windows ce';
        var bIsWM = sUA.match(/windows mobile/i) == 'windows mobile';

        if(bIsIpad || bIsIphoneOs || bIsMidp || bIsUc7 || bIsUc || bIsAndroid || bIsCE || bIsWM){
            console.log('phone');
        }else{
            console.log('PC');
            return 1;
        }
    }
</script>
```
navigator.userAgent.match(/ipad/i) == 'ipad'