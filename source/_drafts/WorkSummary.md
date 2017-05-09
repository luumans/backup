title: 工作纪要
date:
description: 
categories:
- Drafts
tags:
- Drafts
toc: true
author:
comments:
original:
permalink: 
---
　　**工作纪要：**用于反思自己在日常工作中所犯下的错误，时刻提醒自己。
<!-- more -->

## 2017年
### 1-1 标题
> - [内容](链接 "描述")
```
```

### 5-9 懒加载bug
vue-lazyload必须使用网络连接图片，仅仅保存本地图片链接字段。使用import引入文件路径

### 5-7 周报挨批
一直以为周报，只是个形式，没想到今天又挨批了。

### 5-7 微信二维码无法识别
> - [微信内置浏览器 长按识别二维码 功能的两三个坑与解决方案](https://segmentfault.com/a/1190000002985815 "中国城投票活动页面")
> - [前端页面中 iOS 版微信长按识别二维码的bug 与解决](https://devework.com/weixin-qrcode-bug.html "描述")
网页中使用fixed，ios扫描会偏移到网页下部。

```
padding: size(240) 0 0 size(240) !important;
margin: size(-240) 0 0 size(-240) !important;
position: relative;z-index: 100;
-webkit-user-select: none;
```

### 5-6 Gitment
多说停止服务了，转用Gitment不解释。
> - [Gitment：使用 GitHub Issues 搭建评论系统](https://imsun.net/posts/gitment-introduction/ "")

### 5-4 微信浏览器竟然没有300ms延迟

### 4-26 多种弹窗层的图层

### 4-20 活动微信分享
问题：页面之间共用同一个分享，将分享引入到app.vue中加载实现分享同步，但是随之而来的是，需求要求只有某个页面分享可以最为有效分享，我都解决方法：通过对这样页面中添加window方法来控制页面方法回调本地定义的请求，此时就看出http请求的缺陷。
> - [Active](链接 "中国城投票活动页面")

### 4-15 分享到微信
问题：只有部分浏览器，可以分享UC、QQ浏览器[nativeShare.js](https://blog.wangjunfeng.com/archives/618 "")

### 4-15 状态码的设置
问题： 多出使用状态码，变动需要修改多出

### 4-14 [房间里的大象](https://github.com/thzt/book-excerpt/issues/15 "Niclas Hedhman")

### 4-14 WeixinJSDK
问题：引入微信JSDK，配置参数，成功回调出现问题，由于是多层函数嵌套，this指向windows，所以使用了全局方法去回调本地方法。
优化：将微信JSDK分享拆分，进行引用传递参数控制不同的操作。


### 4-12 vue props
问题： 获取方式有别于react子页面，好像无法使用

### 4-11 迅雷产品
迅雷的web效果确实不错[迅雷产品](http://dl.xunlei.com/)

### 3-13 开发弹窗组件
问题：使用SVN没有填写提交描述信息

### 3-13 开发弹窗组件
问题： 有部分知识不会

### 3-13 初步完成方块转盘
组件内容：<slot></slot>，接收父级组件内容部分。
中英文切换：il8n国际标准，使用$t('Name')

### 2-4 标题
> - [投简历]( "时不时的会受到拒绝的，内心却是有点难受。但站在他们的角度，有很正确，毕竟人家要招聘那些上来就可以工作的。自己还有很多的不足之处。")
胜利和败北都要品尝、经历过四处逃窜的辛酸、痛苦和悲伤的回忆、这样才能独当一面、就算痛哭流涕也没关系！一定要闯过这一关。


## 2016年
### 10-21 tipuyou-RN
> - [创建接口白名单](D:\GitLab\php\tipuyou\core\OpenSociax\Api.class.php "记得加白名单，否则需要验证")
> - [单引号与双引号问题](D:\GitLab\php\tipuyou\addons\api_v4\TravelPhotoApi.class.php "单引号只会解析为字符串，双引号遇到变量则会解析变量，双引号中有单引号则要{}")

### 10-20 tipuyou-RN
> - [appIndex](http://v2.jingqubao.com/index.php?app=w3g&mod=Weixin&act=index "API接口调用出错，index_tips.js")
> - [景点详情数据](/api_v4/Scenic/get_scenic_info?rid=65&version=ios_3.9.0 "错误的认为应用数据格式处理现实，业务与格式没有分开")