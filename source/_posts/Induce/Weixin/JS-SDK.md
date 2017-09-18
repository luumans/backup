title: WeChat SDK
date: 2017-07-25 18:29:00
description: 
categories:
- FrontFrame
tags:
- WeChatSDK
toc: true
author:
comments:
original:
permalink: 
---
　　微信作为大佬，使用他的SDK有些什么需要注意的地方！有哪些容易出错的地方。
<!-- more -->



# 微信支付

## [H5支付](https://pay.weixin.qq.com/wiki/doc/api/H5.php?chapter=15_1)
[微信官方体验链接](http://wxpay.wxutil.com/mch/pay/h5.v2.php)
通过微信H5支付可以实现在非微信浏览器（如QQ浏览器、谷歌浏览器、Safari等）中使用微信支付的场景。
### 接口流程图
![](https://pay.weixin.qq.com/wiki/doc/api/img/chapter15_1.png)
[获取跳转链接](https://wx.tenpay.com/cgi-bin/mmpayweb-bin/checkmweb?prepay_id=wx2017081615035708f69384050585043920&package=784262796)
### [常见问题](https://pay.weixin.qq.com/wiki/doc/api/H5.php?chapter=15_4)

## [JSDK支付](https://pay.weixin.qq.com/wiki/doc/api/jsapi.php?chapter=7_1)
用户通过消息或扫描二维码在微信内打开网页时，可以调用微信支付完成下单购买的流程。
![](https://pay.weixin.qq.com/wiki/doc/api/img/chapter7_4_1.png)

### 微信内H5调起支付
```
function onBridgeReady(){
   WeixinJSBridge.invoke(
       'getBrandWCPayRequest', {
           // 公众号名称，由商户传入
           "appId":"wx2421b1c4370ec43b",
           // 时间戳，自1970年以来的秒数
           "timeStamp":"1395712654",
           // 随机串
           "nonceStr":"e61463f8efa94090b1f366cccfbbb444",
           "package":"prepay_id=u802345jgfjsdfgsdg888",
           // 微信签名方式：
           "signType":"MD5",
           // 微信签名
           "paySign":"70EA570631E4BB79628FBCA90534C63FF7FADD89"
       },
       function(res){
           if(res.err_msg == "get_brand_wcpay_request:ok" ) {}     // 使用以上方式判断前端返回,微信团队郑重提示：res.err_msg将在用户支付成功后返回    ok，但并不保证它绝对可靠。 
		}
   );
}
if (typeof WeixinJSBridge == "undefined"){
   if( document.addEventListener ){
       document.addEventListener('WeixinJSBridgeReady', onBridgeReady, false);
   }else if (document.attachEvent){
       document.attachEvent('WeixinJSBridgeReady', onBridgeReady); 
       document.attachEvent('onWeixinJSBridgeReady', onBridgeReady);
   }
}else{
   onBridgeReady();
}
```

```
wx.chooseWXPay({
    // 支付签名时间戳，注意微信jssdk中的所有使用timestamp字段均为小写。但最新版的支付后台生成签名使用的timeStamp字段名需大写其中的S字符
    timestamp: 0,
    // 支付签名随机串，不长于 32 位
    nonceStr: '',
    // 统一支付接口返回的prepay_id参数值，提交格式如：prepay_id=***）
    package: '',
    // 签名方式，默认为'SHA1'，使用新版支付需传入'MD5'
    signType: '',
    // 支付签名
    paySign: '',
	// 支付成功后的回调函数
    success: function (res) {
    }
});
```

## [小程序支付](https://pay.weixin.qq.com/wiki/doc/api/wxa/wxa_api.php?chapter=7_3&index=1)


# 注意事项

## 微信分享出去的链接被，打开后自动添加参数
使用微信出去的页面，在微信中打开时就会显示。出文章在什么终端中打开的参数，添加在链接上。
朋友圈   from=timeline&isappinstalled=0
微信群   from=groupmessage&isappinstalled=0
好友分享 from=singlemessage&isappinstalled=0

解决办法：在链接上添加？分享后打开后，微信会将？去除。
url => url? => url

vue:
/#/ => /?*/ => /#/


[]()
