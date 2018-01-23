title: JavaScript 借助Service Worker和cacheStorage缓存及离线开发
date: 2017-12-25 18:29:00
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
对于web应用，掉线不能使用是理所当然的，绝不会有哪个开发人员会因为网页在没网的时候打不开被测试MM提bug，或者被用户投诉，所以，我们的web页面不支持离线完全不会有什么影响。但如果我们希望支持离线，会发现，我投入的精力和成本啊还真不小，就说一点，html5 manifest缓存技术需要服务端配合直接就让成本蹭蹭蹭的上去了，因为已经不仅仅是前端这一个工种的工作了，很可能就需要跨团队协作，这事情立马一下子就啰嗦了，开发时候啰嗦，以后维护也啰嗦。
而支持离线的收益，我估算了下，比支持无障碍访问收益还要低。所以，站在商业的角度讲，如果一项技术成本比较高，收益比较小，是并不建议实际应用的。开发人员喜欢在重要的项目中玩些时髦的酷酷的新技术自嗨是可以理解的，但前提是收益明显。如果只是自嗨，留下烂摊，有必要警惕了，业务意识和职业素养说明还有待提高。
<!-- more -->

# 应用缓存（已废弃）
HTML5的应用缓存（[application cache](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Using_the_application_cache "")）
AppCache就是从浏览器的缓存中分出一块缓存区，使用描述文件（manifest file），列出要下载和缓存的资源。

## 缺点
1. html5 manifest也可以纯前端！
1. 投入产出比有些低，

## name.manifest
``` javascript
CACHE MANIFEST 
<!-- 文件标识  -->

#VERSION 1.0  
<!-- 版本号，只是一行注释，但改变可以更新缓存 -->
  
CACHE:
<!-- 表示要缓存的文件 -->
index.html  
./js/jquery.js  
./css/style.css  
  
NETWORK:
<!-- 表示需要连接服务器的文件 -->
./upload/  
  
FAILBACK:
<!-- 表示当没有响应时的替代方案 -->
./proxy/ proxy.html  
```

## html
``` javascript
<html manifest="name.appcache">
```

## status属性
1. 0：无缓存
1. 1：限制
1. 2：检查中
1. 3：下载中
1. 4：更新完成
1. 5：废弃

window.applicationCache.UNCACHED => 0
window.applicationCache.IDLE => 1
window.applicationCache.CHECKING => 2
window.applicationCache.DOWNLOADING => 3
window.applicationCache.UPDATEREADY => 4
window.applicationCache.OBSOLETE => 5

## 事件
1. checking：在浏览器应用缓存查找更新时触发
1. error：在检查更新或下载资源期间发生错误触发
1. noupdate：在检查描述文件发现文件变化时触发
1. downloading：在开始下载应用荤菜资源时触发
1. progress：在文件下载应用缓存的过程中持续不断的触发
1. updateready：在页面新的应用缓存下载完毕，并且可以通过swapCache()使用时触发
1. cached：在应用缓存完整可用时触发

### updateready
``` javascript
EventUtil.addHandler(applicationCache, "updateready", function(){
	applicationCache.swapCache();
});

window.applicationCache.addEventListener('updateready', function(e) {
    if (window.applicationCache.status == window.applicationCache.UPDATEREADY) {
        window.applicationCache.swapCache();
        if (confirm('A new version of this site is available. Load it?')) {
                window.location.reload();
        }
    }
});
```

window.applicationCache.update();
浏览器还会尝试去请求 /app.manifest 文件，如果请求成功，就会拿新旧两个版本的 manifest 去对比，如果发现文件内容有更改，则会按照新版 manifest 中列出的文件重新请求一遍资源，并更新到 HAC 里。如果这时有一个文件访问出错，就会导致 HAC 停止更新。但是默认 chrome 会限制 5MB 的缓存大小，如果是 chrome 应用，并想要更多缓存空间的话，则需要声明 unlimitedStorage。
在浏览器内也可以手动的运行 applicationCache.update() 去触发检查。然后通过 applicationCache.status 去判断是否需要更新 HAC。

- [HTML Application Cache 离线应用](https://segmentfault.com/a/1190000005370294 "")
- [什么是Application Cache  API？](https://www.cnblogs.com/blackbird/archive/2012/06/12/2546751.html "")

## 兼容
支持HTML5应用缓存的浏览器有Firefox3+、Safari4+、Opera10.6、Chrome、iOS3.2+版Safari、Android版WebKit。Firefox4之前版本中调用swapCache()会抛出错误。

# Service Worker

## 

## 兼容性
桌面端Chrome和Firefox可用，IE不可用。移动端Chrome可用，以后估计会快速支持。

## 和PWA技术的关系
PWA全称为“Progressive Web Apps”，渐进式网页应用。功效显著，收益明显，

### PWA的核心技术包括

1. Web App Manifest – 在主屏幕添加app图标，定义手机标题栏颜色之类
1. Service Worker – 缓存，离线开发，以及地理位置信息处理等
1. App Shell – 先显示APP的主结构，再填充主数据，更快显示更好体验
1. Push Notification – 消息推送，之前有写过“简单了解HTML5中的Web Notification桌面通知”

有此可见，Service Worker仅仅是PWA技术中的一部分，但是又独立于PWA。也就是虽然PWA技术面向移动端，但是并不影响我们在桌面端渐进使用Service Worker，考虑到自己厂子用户70%~80%都是Chrome内核，收益或许会比预期的要高。

## Service Worker更多的应用场景
Service Worker除了可以缓存和离线开发，其可以应用的场景还有很多，举几个例子（参考自[MDN](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API "")）

1. 后台数据同步
1. 响应来自其它源的资源请求，
1. 集中接收计算成本高的数据更新，比如地理位置和陀螺仪信息，这样多个页面就可以利用同一组数据
1. 在客户端进行CoffeeScript，LESS，CJS/AMD等模块编译和依赖管理（用于开发目的）
1. 后台服务钩子
1. 自定义模板用于特定URL模式
1. 性能增强，比如预取用户可能需要的资源，比如相册中的后面数张图片


- [借助Service Worker和cacheStorage缓存及离线开发](http://www.zhangxinxu.com/wordpress/2017/07/service-worker-cachestorage-offline-develop/comment-page-1/#comment-362906 "")
- [Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API "")

- []( "")

