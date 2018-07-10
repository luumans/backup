title: 移动端真机调试实战经验
date: 2017-04-19 14:11:20
description: 
categories:
- Tool
tags:
- Git
toc: true
author:
comments:
original:
permalink: 
---
　　** Git的使用技巧:**
<!-- more -->



# 前言
在开发中前端免不了要进行移动端的开发，然而在电脑上看的样式和手机上还是有一定的差距的，因为手机上有顶部的状态栏和底部的菜单栏，特别是在qq内置浏览器中打开，差距还是蛮大的，所以在chrom中模拟手机显示的情况虽然有一定的效果，但是还是不能完全模拟，我们还需要在真机环境下测试。本文介绍的调试方法有一下几种：
iphone+safari
android手机+pc
微信开发者工具
weinre
使用webstorm
使用Fiddle抓包

这几种方法基本说涵盖了我们平时开发中所遇到的各种情况，各种主流设备都可以覆盖。其中最方便快捷的是使用webstorm自带的服务器，只需要一键就可以~，但是这样只能预览，不能调试。我个人比较推荐的方法是iphone+safari或者安卓手机+pc的这种方式，比较简单方便快捷，然后根据具体的环境再选择更为合适的调试方法。目前我认为使用weinre+fiddle是万能的，没有什么调试不了了~但是需要学习的成本也是最高的~希望大家都能够写出漂亮的页面，不需要为调试发愁哈~
## iphone+safari
之前使用的是mac，所以一直都是用的iphone+safari模拟真机环境，这种方法简单明了，只需要简单的设置一下以后都不要设置，插上数据线，打开mac上的safari就可以了，(๑•̀ㅂ•́)و，✧，但是对设备有要求，必须是iphone+mac的组合
iphone上设置
设置 → Safari → 高级 → Web 检查器 → 开。
![](https://user-gold-cdn.xitu.io/2017/4/10/5634b9929be3e77d61b361d03efab6ea)这里写图片描述

pc端safari设置
Safari → 偏好设置 → 高级 → 在菜单栏中显示“开发”菜单
![](https://user-gold-cdn.xitu.io/2017/4/10/e8b0fccddf44a6d53ebe0170d4626b2b.jpg)这里写图片描述

设置完之后用数据线连接电脑，然后在iphone上用打开safari需要调试的网址，然后在pc端打开safari，最上面的菜单栏中的“开发”然后就可以看到有iphone设备的名称显示然后就可以看见你在iphone中的safari中打开了哪些网址，之后就和调试网页版的一样了

![](https://user-gold-cdn.xitu.io/2017/4/10/dd21015037825ccfb74c99eff58eaa17.jpg)

## android手机+pc
安卓手机只需要下载chrom浏览器，就可以再电脑上用chrom调试了，是不是很赞(づ￣3￣)づ╭
首先需要装chrom浏览器
打开手机的开发者模式，一般是：设置->关于手机->版本号连按5次，之后设置菜单中会多出一个开发人员选项，进入将其中的“usb调试”打开![](https://user-gold-cdn.xitu.io/2017/4/10/b03e8ca7e78f13c3c394147694a23c18.png!web)

将手机与电脑通过usb连接，弹出对话框“是否允许usb调试”，选择确定![](https://user-gold-cdn.xitu.io/2017/4/10/abd4ed097439a996e609bd44578216d6.png!web)

在手机chrom上打开要调试的页面
在电脑上打开chrom，新开一地址栏为chrome://inspect/
的页面,然后就可以调试了![](https://user-gold-cdn.xitu.io/2017/4/10/59da83f5822c8761cb397b1bdd152808.png!web)

点击inspect弹出chrom调试工具

微信开发者工具
由于不可描述原因，有些页面只在微信里面出错，并且好多涉及到了微信相关的接口必须要使用微信环境的，比如自定义分享
前期准备
这个使用起来很方便。（我记得之前使用的时候（2016年），你要调试的页面必须是你是管理员的微信公众号下面的js安全域名下的地址），但是刚刚下载一个新版本的开发者工具（v0.7.0），现在的时间是2017年3月28日，发现好像没有这个限制了。所以这样开发起来就方便多了。
官方文档&下载地址
[官方文档](https://link.juejin.im/?target=https%3A%2F%2Fmp.weixin.qq.com%2Fwiki%3Ft%3Dresource%2Fres_main%26amp%3Bid%3Dmp1455784140%26amp%3Btoken%3D%26amp%3Blang%3Dzh_CN)其中有下载地址

## 模拟微信环境调试
直接在地址栏输入地址就可以模拟微信环境调试，是不是很方便~这种方式可以满足大部分的需求
真机调试
在开发者工具中的移动调试中可以有详细的ios和安卓的调试方式，主要是使用代理，这里我没有调试成功，在手机微信中一直打不开网页，所以就不详细写了╥﹏╥...
---------------3.29更---------------------我找到设置代理之后打不开网页的原因了，在使用fiddler抓包的时候也遇到了同样的问题，原来这里需要下载认证在设置完当前网络的代理之后，在浏览器输入本机的ip地址和ip号，下载fiddler的证书
![](https://user-gold-cdn.xitu.io/2017/4/10/ea5c1485010b540f5c5746dcb11885ed)

然后根据提示一步一步安装就可以了。安装完证书就可以打开网页了
weinre
跟着我念三遍weinre大法好，weinre大法好，weinre大法好~之前介绍的几种方式或多或少都有一些条件限制，但是weinre没有啊喂！就比如我是ios+windows的组合，就不能使用iphone+safari和安卓+pc的方式调试，所以使用weinre就可以！！缺点就是配置起来有削微的麻烦~
什么是weinre
weinre是web inspector remote（远程web检查器）的缩写
安装weinre
目前安装weinre我了解到有两种方式：node和java两种方式
node方式安装weinre
首先确保你的电脑上有node环境，然后使用npm来安装windows下npm install weinre -g --registry=https://registry.npm.taobao.org
mac下sudo npm install weinre -g --registry=https://registry.npm.taobao.org

java环境下安装weinre
首先确认你电脑上装好的java环境，然后下载weinre的jar包，上百度云盘的链接：链接: [https://pan.baidu.com/s/1slRiOl3](https://link.juejin.im/?target=https%3A%2F%2Fpan.baidu.com%2Fs%2F1slRiOl3) 密码: dsmp

## 运行weinre
node环境下
weinre --httpPort 8081 --boundHost -all-
8081
是调试服务器运行的端口号，boundHost
是调试服务器绑定的ip地址或域名，默认是localhost，设置为-all-是为了在本地能使用localhost打开，在移动设备或本地环境用ip地址打开weinre调试工具
java环境下
在weinre所在文件夹的地址栏输入代码：java -jar weinre.jar --httpPort 8081 --boundHost -all-

![](https://user-gold-cdn.xitu.io/2017/4/10/f12671df3da397d2d2d4b90352866ef4)

开始调试
设置好端口之后我们在本地打开http://192.168.0.126:8081
然后就可以看见weinre的基本信息
![](https://user-gold-cdn.xitu.io/2017/4/10/0c71a5a5cddaffb1a31a1a9d2daefa8c.gif)

之后我们需要在需要调试的页面上加上一段script标签<script src="http://192.168.0.126:8081/target/target-script-min.js#anonymous"></script>
需要改为你自己的ip地址
ip的查询方式
在cmd输入ipconfig
，然后ipv4中后面跟的就是本机的ip地址
手机打开需要调试的链接
在staticWebDir目录下
本地的源文件貌似只能在staticWebDir目录下才可以访问到（这是因为在没有使用任何服务器的情况下，weinre自带有服务器，所以只能放在默认的根目录下），将你的源文件放在staticWebDir目录下，staticWebDir的目录是你安装weinre的根目录，我的是：C:\Users\supfn\AppData\Roaming\npm\node_modules\weinre\web
，然后手机访问:http://192.168.0.126/contact_page/index.html
，然后在电脑上打开刚刚的页面http://196.168.0.126:8081

![](https://user-gold-cdn.xitu.io/2017/4/10/a38f38a25e5c5d424bc79007984843bc.png)

点击debug client user interface
之后出现![](https://user-gold-cdn.xitu.io/2017/4/10/686360e5676fa97b2e172043704298c7.png)

，点击蓝色的链接，变为绿色的之后就说明链接成功了。在后面的elements
和其他的tag就可以进行调试
在xampp下
因为公司的项目是在xampp下的，已经配置好了apache，可以直接在平时的项目前加上本地的ip，在手机上访问就好。项目存放的地址是xampp\htdocs\app
修改配置：C:\Windows\System32\drivers\etc\hosts
文件下最后一行127.0.0.1 localhost ltrip.com fzc.com m.fzc.com m.ltrip.com
然后在C:\xampp\apache\conf\extra
文件里面修改
<VirtualHost *:80> DocumentRoot "C:\xampp\htdocs\ltrip" ServerName ltrip.com ServerAlias <Directory "C:\xampp\htdocs\ltrip"> Options FollowSymLinks ExecCGI AllowOverride All Order allow,deny Allow from all Require all granted </Directory></VirtualHost>

其中的ServerName ltrip.com
中的ltrip.com
就代替了"C:\xampp\htdocs\ltrip"
这个路径，所以就不需要放在staticWebDir目录下了，这样手机打开的地址就变成了:http://192.168.0.168/ltrip.com

使用webstorm
在最先开始使用weinre的时候，一直卡在一个地方，就是手机访问的地址问题，在看教程的时候我就卡在不知道怎么输入手机打开的网址，因为我是自己写的一个简单的html的demo，在本地打开的地址是使用本地的绝对路径比如file:///C:/Users/supfn/Desktop/contact_page/index.html
这样子的，在手机肯定访问不到我的电脑上的路径。这里是需要在本地搭建一个服务器，这样才能在手机访问到你电脑上的资源，通过服务器其他人也可以访问你电脑上的资源，常见的服务器有apache，使用Java的还可以用tomcat。这些使用起来都比较麻烦，这里推荐一个简单的方式，使用webstorm。webstorm集成了debugger服务器，所以可以直接在你项目html页面的右上角点击浏览器的图标，在对应浏览器打开项目，然后将地址栏上的localhost改为你的ip地址，手机访问这个地址就可以了~简直不要太方便！！所以webstorm真的是web开发利器，而不止是一个编辑器
使用Fiddle抓包
如果是要调试线上代码的话经常是无法再页面中直接加入script标签的，然后我们可以利用fiddler为页面设置断点，然后注入js代码，在run就可以了~fiddler是用过改写http代理，让数据从它这通过，来监控截取到的数据。在打开fiddler的时候，就已经自动设置好了浏览器的代理了，关闭的时候，它又把代理还原了
下载fiddler
Fiddler 下载地址 ：[https://www.telerik.com/download/fiddler](https://link.juejin.im/?target=https%3A%2F%2Fwww.telerik.com%2Fdownload%2Ffiddler)Fiddler 离线下载地址：[http://pan.baidu.com/s/1i3NvE8P](https://link.juejin.im/?target=http%3A%2F%2Fpan.baidu.com%2Fs%2F1i3NvE8P) 密码：ozem
使用fiddler抓取数据包
在手机上设置同一个局域网上的代理，代理服务器设置为电脑的ip地址，端口为8888在fiddler上，点击菜单栏中的 [Tools] –> [Fiddler Options]
![](https://user-gold-cdn.xitu.io/2017/4/10/dbef5c27072d4ed9caa86d73bbab8e7e)

点击 [Connections] ，设置代理端口是8888， 勾选 Allow remote computers to connect， 点击OK![](https://user-gold-cdn.xitu.io/2017/4/10/44b4e15c76a89426f9a3651874af2981)

使用weinre与fiddler组合
我们要实现的目标就是要调试线上的代码，使用fiddler在代码中注入weinre需要加上的script标签在完成配置之后打开要调试的链接，然后在fiddler中设置断点我们在fiddler中打下页面断点,bpafter + 想要打断点的网址
![](https://user-gold-cdn.xitu.io/2017/4/10/ab4655c2c3014124a5ba321377c1832f.png!web)

再次访问该网站，发现本条请求被block住了![](https://user-gold-cdn.xitu.io/2017/4/10/c4d25cadaa92486846378762dd773d42)这里写图片描述

然后在右边加上weinre需要的script标签<script src="http://192.168.0.126:8081/target/target-script-min.js#anonymous"></script>
然后点击右边代码上面绿色的run to completion就可以看到注入js的效果了，之后我们就可以在weinre中调试了~

# 参考文章
[Fiddler 抓包工具总结](https://link.juejin.im/?target=http%3A%2F%2Fblog.csdn.net%2Fqq_21445563%2Farticle%2Fdetails%2F51017605)
[移动端调试工具weinre安装教程（java版）](https://link.juejin.im/?target=http%3A%2F%2Fwww.cnblogs.com%2Fjhyxk2007%2Fp%2F5089997.html)
[聊一聊移动调试那些事儿](https://link.juejin.im/?target=http%3A%2F%2Fwww.tuicool.com%2Farticles%2FMbYz6fA)