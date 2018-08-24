title: 微信小程序开发环境搭建
date: 2018-08-25 18:29:00
description: 
categories:
- WeChat
tags:
- WeChatApp
toc: true
author:
comments:
original:
permalink: 
---
　　**自用笔记：**最近微信小程序，这么一火，确实让不少人重视了前端的发展。信息的实效性，信息的快速发展，要求我们也要不断的学习新鲜的知识。虽然被称为很烂俗的东西，但是市场毕竟还是有的。
微信小程序的最用，其实，之前我参见一些HTML5大会，就有所耳闻，那是叫做：“流应用”。页面通过模块加载，实现页面的“闪开”。确实感慨微信的力量，短短几天时间就被炒得这么火。
<!-- more -->

# 注册
[注册地址](https://mp.weixin.qq.com/wxopen/waregister?action=step1 "")

# 目录结构

```
WeApp     (项目名称)
|–image              图片资源
|–utils              存放utils文件，可require引入
    |–*.js                页面逻辑
|–page               页面文件
    |–*.wxml              页面结构
    |–*.wxs               小程序的一套脚本语言
    |–*.wxss              样式表文件
    |–*.js                脚本文件
    |–*.json              配置文件
|–app.js        脚本代码
|–app.json      全局配置
|–app.wxss      公共样式表
```

## 小程序的启动 app.js
App() 函数用来注册一个小程序。接受一个 object 参数，其指定小程序的生命周期函数等。
开发者可以添加任意的函数或数据到 Object 参数中，用 this 可以访问

> 小程序

wxa4cf31b073aae45f

> 小游戏

wxa4d10240e0af756f
wxb17d48711010ff79


全局定义

```
getUserInfo:function(cb){
  var that = this
  if(this.globalData.userInfo){
    typeof cb == "function" && cb(this.globalData.userInfo)
  }else{
    //调用登录接口
    wx.login({
      success: function () {
        wx.getUserInfo({
          success: function (res) {
            that.globalData.userInfo = res.userInfo
            typeof cb == "function" && cb(that.globalData.userInfo)
          }
        })
      }
    })
  }
},
```

getApp() 函数，可以获取到小程序实例。

```
var app = getApp()
//调用应用实例的方法获取全局数据
app.getUserInfo(function(userInfo){
  //更新数据
  that.setData({
    userInfo:userInfo
  })
})
```

### 小程序配置 app.json
```
{
  "networkTimeout":[
    "pages/index/index",
    "pages/logs/logs"
  ],
  "window":{
    <!-- 导航栏背景颜色，如 #000000 -->
    "navigationBarBackgroundColor": "#000000",
    <!-- 导航栏标题颜色，仅支持 black / white -->
    "navigationBarTextStyle": "black",
    <!-- 导航栏标题文字内容 -->
    "navigationBarTitleText": "导航栏标题文字内容",
    <!-- default 导航栏样式， custom 自定义导航栏 -->
    "navigationStyle": "default",
    <!-- 窗口的背景色 -->
    "backgroundColor": "#ffffff",
    <!-- dark  下拉 loading 的样式，仅支持 dark / light -->
    "backgroundTextStyle": "dark",
    <!-- 顶部窗口的背景色 -->
    "backgroundColorTop": "#ffffff",
    <!-- 底部窗口的背景色 -->
    "backgroundColorBottom": "#ffffff",
    <!-- 是否全局开启下拉刷新。Page.onPullDownRefresh -->
    "enablePullDownRefresh": false,
    <!-- 页面上拉触底事件触发时距页面底部距离，单位为px。Page.onReachButom -->
    "onReachBottomDistance": "50",
    <!-- 页面整体不能上下滚动 -->
    "disableScroll": false
  },
  "tabBar": {
    <!-- 文字默认颜色 -->
    "color": "#000000",
    <!-- 文字选中时的颜色 -->
    "selectedColor": "#000000",
    <!-- 背景色 -->
    "backgroundColor": "#000000",
    <!-- tabbar上边框的颜色， 仅支持 black / white -->
    "borderStyle": "#black",
    <!-- tabBar的位置，仅支持 bottom / top -->
    "position": "bottom",
    "list": [
      {
        "pagePath": "pages/index/index",
        "text": "首页",
        <!-- 选中时的图片路径 81px * 81px 40kb -->
        "selectedIconPath": "pages/index/index",
        <!-- 图片路径 -->
        "iconPath": "首页"
      }
    ]
  },
  "networkTimeout": {
    <!-- wx.request 的超时时间，单位毫秒。 -->
    "request": "60000",
    <!-- wx.connectSocket 的超时时间，单位毫秒。 -->
    "connectSocket": "60000",
    <!-- wx.uploadFile 的超时时间，单位毫秒。 -->
    "uploadFile": "60000",
    <!-- wx.downloadFile 的超时时间，单位毫秒。 -->
    "downloadFile": "60000"
  },
  <!-- 启用分包加载时，声明项目分包结构 -->
  "subPackages": [
      {
        "root": "packageA",
        "pages": [
          "pages/cat",
          "pages/dog"
        ]
      }, {
        "root": "packageB",
        "pages": [
          "pages/apple",
          "pages/banana"
        ]
      }
    ]
  <!-- Worker 处理多线程任务 -->
  "workers": true,
  <!-- 后台音乐播放 -->
  "requiredBackgroundModes": true,
  <!-- 启用插件功能页 -->
  "functionalPages": true,
  <!-- 开发者工具中开启 debug 模式 -->
  "debug": true
}
```






## 注释：
1. 代码不可以多逗号，直接报错。

## WXSS
### rpx
rpx（responsive pixel）: 可以根据屏幕宽度进行自适应。规定屏幕宽为750rpx。如在 iPhone6 上，屏幕宽度为375px，共有750个物理像素，则750rpx = 375px = 750物理像素，1rpx = 0.5px = 1物理像素。
rem（root em）: 规定屏幕宽度为20rem；1rem = (750/20)rpx

### 样式导入
使用@import语句可以导入外联样式表，@import后跟需要导入的外联样式表的相对路径，用;表示语句结束。
> app.wxss 中的样式为全局样式、

```
/** common.wxss **/
.small-p {
  padding:5px;
}
```

```
/** app.wxss **/
@import "common.wxss";
.middle-p {
  padding:15px;
}
```

### 内联样式
框架组件上支持使用 style、class 属性来控制组件的样式。

style：静态的样式统一写到 class 中。style 接收动态的样式，在运行时会进行解析，请尽量避免将静态的样式写进 style 中，以免影响渲染速度。
```
<view style="color:{{color}};" />
```

class：用于指定样式规则，其属性值是样式规则中类选择器名(样式类名)的集合，样式类名不需要带上.，样式类名之间用空格分隔。
```
<view class="normal_view" />
```
图片资源暂时只支持jpg、png
### 选择器

| 选择器 | 样例 | 样例描述 |
| -----| -----| -----|
|.class  |	.intro	| 选择所有拥有class="intro"的组件 |
|#id     |	#firstname	| 选择拥有id="firstname"的组件 |
|element | page、view	|	选择所有view组件 |
|element,element |	view,checkbox	 | 选择所有文档的view组件和所有的 checkbox 组件 |
|::after  |	view::after| 	在view组件后边插入内容 |
|::before |	view::before| 	在view组件前边插入内容 |

## DeBug：
1. 代码不可以多逗号，直接报错。

### 多余文字隐藏
```
不可以使用text，用view。
overflow:hidden;
text-overflow:ellipsis;
white-space:nowrap;
```
[让你的「微信小程序」运行在Chrome浏览器上，让我们使用WebStorm](http://mp.weixin.qq.com/s?__biz=MjM5Mjg4NDMwMA==&mid=2652974133&idx=1&sn=3b67419e8ac0bb8262ca4c1e3cdabb35#rd "")

项目：
[微信小应用-小程序-demo-仿芒果TV](https://github.com/web-Marker/wechat-Development "")
[微信小程序资源汇总整理](https://github.com/Aufree/awesome-wechat-weapp "")
[wechat app for dribbble](https://github.com/nicesu/wechat-dribbble "")

[微信小程序开发资源汇总](https://github.com/justjavac/awesome-wechat-weapp "")

[首个微信小程序开发教程](http://gold.xitu.io/entry/57e34d6bd2030900691e9ad7/view "")
[微信小程序：不得不学的 js 技巧之关键字 this](http://gold.xitu.io/post/57e48c0075c4cd2d9f3485f1?utm_source=gold_browser_extension "")
[ 全球首个微信应用号开发教程第一弹！通宵吐血赶稿，每日更新！](http://www.diycode.cc/topics/311 "")
[[博卡君]微信应用号「小程序」开发教程首发第二弹！](http://mp.weixin.qq.com/s?__biz=MzIyMDM2Mjg2Nw==&mid=2247484440&idx=1&sn=d612c592324a20b4240de0a0b98feee5&chksm=97cc6504a0bbec1297b5116dbc051dbcb8b7a6165fd3392dabd1a3ef0a38e8b452a64092f8af&scene=1&srcid=0923opn6Lqe2D6yaowz4RNco#rd "")
[微信小程序开发资源汇总](https://github.com/justjavac/awesome-wechat-weapp "")
[首个微信小程序开发教程！](http://gold.xitu.io/entry/57e34d6bd2030900691e9ad7 "")
[   ](https://zhuanlan.zhihu.com/p/22574282 "")
[微信小程序剖析【下】：运行机制](http://mp.weixin.qq.com/s?__biz=MjM5Mjg4NDMwMA==&mid=2652974093&idx=1&sn=0570a243304ea8bb7d1b636624886fb1#rd "")
[微信小程序，一个有局限的类似 React Native 轮子！](http://www.jianshu.com/p/060c6f3dd4e# "")
[微信小程序Doc](http://wxopen.notedown.cn/ "")

注意：
1. 找不到所要替换的文件
问题原因：开发工具版本不正确，老版本不支持
解决方案：确保下载的程序版本在0.9.092100以上

1. Failed to load resource: net::ERR_NAME_NOT_RESOLVED http://1709827360.appservice.open.weixin.qq.com/appservice
问题原因：通常是由于系统设置了代理如Shadowsocks等。
解决方案：关闭代理，或者依次点击工具栏“动作”-"设置"，选择“不使用任何代理，勾选后直连网络”。

1. 修复asdebug.js报错
问题原因：TypeError: Cannot read property 'MaxRequestConcurrent' of undefined
解决方案：替换 /Resources/app.nw/app/dist/weapp/appservice/asdebug.js

1. 扫码登录失败
问题原因：please bind your wechat account to the appid first
解决方案：先使用0.7版本的进行扫码登陆，登陆成功后，再用0.9的版本打开就直接进入了。
0.7版本地址：http://dldir1.qq.com/WechatWebDev/release/0.7.0/wechat_web_devtools_0.7.0.dmg

# 环境搭建：
人类的智慧还是伟大的，短短的几天时间，微信小程序就破解出来了。我也来上手试试。(现在都不需要破解了，API接口很少)

![demo的动态图](http://img.blog.csdn.net/20160923011211416)

下载：
[【应用号】IDE + 破解 + Demo](https://github.com/gavinkwoe/weapp-ide-crack)

由于新版本的微信开发程序，取消了登录的界面。所以我们要先用0.7版本的登录后，覆盖安装0.9版本的程序。

分别找到下面三个目录替换对应文件即可
进入程序目录后，替换以下文件（只需要替换0.9版本里的，0.7版本用来登陆）：

Windows：
\package.nw\app\dist\components\create\createstep.js
\package.nw\app\dist\stroes\projectStores.js
\package.nw\app\dist\weapp\appservice\asdebug.js

Mac：
/Resources/app.nw/app/dist/components/create/createstep.js
/Resources/app.nw/app/dist/stroes/projectStores.js
/Resources/app.nw/app/dist/weapp/appservice/asdebug.js

# 组件

## swiper

```
<swiper class="swiper-box" indicator-dots="{{swiper.indicatorDots}}" autoplay="{{swiper.autoplay}}" interval="{{swiper.interval}}" duration="{{swiper.duration}}" vertical="{{swiper.vertical}}">
	<block wx:for="{{imgUrls}}">
    <swiper-item>
    	<navigator url="{{pageKey}}">
			<image src="{{item}}" class="slide-image" width="355" height="150"/>
    	</navigator>
    </swiper-item>
  </block>
</swiper>
```

```
swiper: {
  indicatorDots: false,
  autoplay: false,
  interval: 5000,
  duration: 1000,
  vertical: false
},
imgUrls: [
  'http://img02.tooopen.com/images/20150928/tooopen_sy_143912755726.jpg',
  'http://img06.tooopen.com/images/20160818/tooopen_sy_175866434296.jpg',
  'http://img06.tooopen.com/images/20160818/tooopen_sy_175833047715.jpg'
],
```

```
.swiper-box{
	height: 300rpx;
}
```
## navigator
```
url="redirect?title=redirect" 页面重定向
```

# API:

## setNavigationBarTitle 导航栏标题文字内容
	wx.setNavigationBarTitle({
	  title: options.title
	})
由此可见，这些页面设置都可以通过wx进行设置。

## wx.request
```
	wx.request({
	  url: 'http://m.maoyan.com/movie/list.json',
	  data: {
	    offset: 0,
	    type: 'hot',
	    limit: 10
	  },
	  header: {
	      'Content-Type': 'application/json'
	  },
	  success: function(res) {
	    console.log(res.data)
	    that.setData({
	      films: res.data.data.movies
	    })
	  }
	})

```
## 数据请求
```
	fetch('https://api.douban.com/v2/movie/subject/' + id).then(function(response){
	  if(response.status !== 200){
	      console.log("error："+ response.status);
	      return;
	  }
	  response.json().then(function(data){
	      that.setData({
	        film: data
	      })
	  });
	})

```
接口：

# 问题
## 获取用户信息wx.getUserInfo用法
```
<button size="mini" wx:if="{{!hasUserInfo && canIUse}}" open-type="getUserInfo" bindgetuserinfo="getUserInfo"> 登录12 </button>
<block wx:else>
  <image class="userinfo-avatar" src="{{userInfo.avatarUrl}}" background-size="cover"></image>
  <text class="userinfo-nickname">{{userInfo.nickName}}</text>
</block>

<!-- 判断是否可以使用 -->
canIUse: wx.canIUse('button.open-type.getUserInfo')
<!-- 直接判断是版本是否大于2.0.7 -->

getUserInfo: function(e) {
  app.globalData.userInfo = e.detail.userInfo
  this.setData({
    userInfo: e.detail.userInfo,
    hasUserInfo: true
  })
}
```

可以多次调出，不像默认的授权界面，用户取消后就不再出现
```
wx.canIUse('button.open-type.getUserInfo')
```


# 发布

# 案例
## 淘宝开放平台
[淘宝开放平台](http://open.taobao.com/)

## 优酷开放云平台
[优酷开放云平台](http://doc.open.youku.com/)

## 百度地图开放平台
[百度地图开放平台](http://lbsyun.baidu.com/index.php?title=%E9%A6%96%E9%A1%B5)

## 百度拼音
[百度拼音](http://olime.baidu.com/py?py=ab&rn=0&pn=20&ol=1&t=1319602514208)

## 知乎数据API
[知乎数据](https://github.com/shanelau/zhihu)

## 知乎日报 API 分析
[知乎日报](https://github.com/izzyleung/ZhihuDailyPurify/wiki/%E7%9F%A5%E4%B9%8E%E6%97%A5%E6%8A%A5-API-%E5%88%86%E6%9E%90)

## 知乎banner
[知乎banner](https://www.zhihu.com/node/Banner?loc=home_up)

## 豆瓣API
[豆瓣](https://developers.douban.com/wiki/?title=guide)

## 爱搜索，爱生活，基于豆瓣API & Angular开发的web App（by vczero）
[爱搜索，爱生活，基于豆瓣](http://www.tuicool.com/articles/BzUfYvY)

## GithubApi
[Github](https://developer.github.com/v3/)

## 去哪儿网火车票
[去哪儿网火车票](http://apistore.baidu.com/apiworks/servicedetail/697.html)

## 芒果TV
[芒果](http://m.api.hunantv.com/channel/getDetail)

## dribbble
[dribbble](http://developer.dribbble.com/)

## 自用acfun解析api接口
[自用acfun解析api接口](http://www.iippcc.com/zi-yong-acfunjie-xi-apijie-kou.html)

## 猫眼热门电影列表
[猫眼热门电影列表](http://www.jianshu.com/p/9855610eb1d4)


