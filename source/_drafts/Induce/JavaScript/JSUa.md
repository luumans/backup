title: 浏览器版本
date: 2016-02-27 18:29:00
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
function parseUA() {
    var u = navigator.userAgent;
    var u2 = navigator.userAgent.toLowerCase();
    return { //移动终端浏览器版本信息
        trident: u.indexOf('Trident') > -1, //IE内核
        presto: u.indexOf('Presto') > -1, //opera内核
        webKit: u.indexOf('AppleWebKit') > -1, //苹果、谷歌内核
        gecko: u.indexOf('Gecko') > -1 && u.indexOf('KHTML') == -1, //火狐内核
        mobile: !!u.match(/AppleWebKit.*Mobile.*/), //是否为移动终端
        ios: !!u.match(/\(i[^;]+;( U;)? CPU.+Mac OS X/), //ios终端
        android: u.indexOf('Android') > -1 || u.indexOf('Linux') > -1, //android终端或uc浏览器
        iPhone: u.indexOf('iPhone') > -1, //是否为iPhone或者QQHD浏览器
        iPad: u.indexOf('iPad') > -1, //是否iPad
        webApp: u.indexOf('Safari') == -1, //是否web应该程序，没有头部与底部
        iosv: u.substr(u.indexOf('iPhone OS') + 9, 3),
        weixin: u2.match(/MicroMessenger/i) == "micromessenger",
        ali: u.indexOf('AliApp') > -1,
    };
}
var ua = parseUA();
if (ua.weixin) {
    location.href = './blank.html';
}
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