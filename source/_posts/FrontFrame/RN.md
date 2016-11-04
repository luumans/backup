title: React Native初探
date: 2016-12-25 18:29:00
description: 
categories:
- FrontFrame
tags:
- React
toc: true
author:
comments:
original:
permalink: 
---

　　**自用笔记：**本文属于自用笔记，不做详解，仅供参考。在此记录自己已理解并开始遵循的前端代码规范。What How Why
<!-- more -->
## React Native初探
<!-- learn once,write everywhere! -->
ReactNative是Facebook在2015年React开发者大会上公开的应用开发框架，一个可以用React开发原生应用的框架。

### 技术背景
FaceBook => HTML5、NativeApp
HybridApp => Native + Web 混合模式

### 特点：
JSX语法（扩展的JS语法）、组件化模式、Virtual Dom、DataBinding单向数据流、可以实现Chrome的调试

### 基本模式：
每个React应用可视为组件的组合，而每个React组件由属性（Property）和状态（state）来配置，当状态发生变化时更新UI，组件的结构是由虚拟的DOM来维护，确保了实际更新的DOM只包括真正产生了状态变化的部分。

### 同类型的代码：
GoogleSky、Titanium、NativeScript（太重）、鸟巢（支付宝）、BeeFrameWork
综合起来：强大的社区，简单的学习，简单的开发、简单的应用。


### 跨平台开发框架

优点；
1. 跨平台、兼容web、ios、android三大主流平台
1. React调用原生控件，性能优于H5框架
1. 更好的手势识别
1. 实时部署更新，再也不用担心应用市场审核缓慢

设计理念：既拥有Native的用户体验，又保留React的开发效率！

Facebook官方使用React Native开发的应用：Groups、Ads Manager、F8、Adverts Manger、天猫IPad、Chinese Flashcards

### ReactNative提供了那些能力
1. 基于原生UI组件
1. 手势识别
1. 基于FlexBox的CSS布局模式
1. 跨平台开发
1.  基于React、jsx组件化开发模式

## IOS环境搭建

| 环境依赖： |
| -----|
| OSX    |
| Xcode    |
| Node    |
| sublime    |

### 安装Homebrew：
Homebrew, Mac系统的包管理器，用于安装NodeJS和一些其他必需的工具软件。通过homebrew安装Node、watchman、flow

#### 安装[Homebrew](http://brew.sh/ "Homebrew官网")：
```bash
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

注：在Max OS X 10.11（El Capitan)版本中，homebrew在安装软件时可能会碰到/usr/local目录不可写的权限问题。可以使用下面的命令修复：

```bash
sudo chown -R `whoami` /usr/local
```

#### 查看是否安装homebrew
```
brew -v
```

### Node
```bash
brew install node
```

### Watchman
Watchman是由Facebook提供的监视文件系统变更的工具。安装此工具可以提高开发时的性能（packager可以快速捕捉文件的变化从而实现实时刷新）。
```bash
brew install watchman 检测文件变化
```

### Flow
Flow是一个静态的JS类型检查工具。译注：你在很多示例中看到的奇奇怪怪的冒号问号，以及方法参数中像类型一样的写法，都是属于这个flow工具的语法。这一语法并不属于ES标准，只是Facebook自家的代码规范。所以新手可以直接跳过（即不需要安装这一工具，也不建议去费力学习flow相关语法）。
```bash
brew install flow 检测js语法
```

```bash
clear 清理屏幕
```


### React Native
React Native的命令行工具用于执行创建、初始化、更新项目、运行打包服务（packager）等任务
```bash
npm install -g react-native-cli 全局安装react native
```
如果你看到EACCES: permission denied这样的权限报错，那么请参照上文的homebrew译注，修复/usr/local目录的所有权：
```bash
sudo chown -R `whoami` /usr/local
```




## Android环境搭建（Window下）
| 环境依赖： |
| -----|
| Git    |
| Node    |
| Python 2    |
| Android Studio    |
| react-native-cli    |
| Microsoft C++ 环境    |
| android 6.0 真机   |

### 安装java JDk
从Java官网下载[Java SE Development Kit 7 Downloads](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)并安装。请注意选择x86还是x64版本。推荐将JDK的bin目录加入系统PATH环境变量。（安装JDK、JRE）

#### 配置环境变量：
> 系统变量→新建 JAVA_HOME 变量
	
```bash
C:\Program Files\Java\jdk1.7.0_79
```
> 系统变量→寻找 Path 变量→编辑
	
```bash
%JAVA_HOME%\bin;%JAVA_HOME%\jre\bin;
```
> 系统变量→新建 CLASSPATH 变量→编辑
	
```bash
.;%JAVA_HOME%\lib;%JAVA_HOME%\lib\tools.jar
```
> java -version 查看Java版本


### 安装Python 2
从官网下载并安装[python 2.7.x](http://www.liaoxuefeng.com/wiki/001374738125095c955c1e6d8bb493182103fac9270762a000 "Python 2.7教程 - 廖雪峰的官方网站")（3.x版本不行）

### Android Studio
[Android Studio](http://www.android-studio.org/index.php/download)






<!-- 
### Genymontion模拟器
比起Android Studio自带的原装模拟器，Genymotion是一个性能更好的选择，但它只对个人用户免费。
[注册](https://www.genymotion.com/account/create/ "luuman")
[Download ](https://www.genymotion.com/download/ "")

安装Virtual Box
[Download ](https://www.virtualbox.org/wiki/Downloads "") 
-->
#### 设置SDK Manager
安装完成后，在Android Studio的欢迎界面中选择Configure | SDK Manager

>在SDK Tools窗口中，选择右下角“Show Package Details”，然后在Android SDK Build Tools中
勾选Android SDK Build-Tools 23.0.1。（必须是这个版本）

>在SDK Platforms窗口中，选择Show Package Details，然后在Android 6.0 (Marshmallow)中勾选
	Google APIs
	Intel x86 Atom System Image
	Intel x86 Atom_64 System Image
	Google APIs Intel x86 Atom_64 System Image。


### 安装React Native
React Native的命令行工具用于执行创建、初始化、更新项目、运行打包服务（packager）等任务
```bash
npm install -g react-native-cli 全局安装react native
```
注：如果你看到EACCES: permission denied这样的权限报错，那么请参照上文的homebrew译注，修复/usr/local目录的所有权：
```bash
sudo chown -R `whoami` /usr/local
```

### 创建项目
```bash
react-native init [DocName] 通过react-native创建项目luuman
```

#### 设置cnpm
```bash
alias cnpm="npm --registry=https://registry.npm.taobao.org \
--cache=$HOME/.npm/.cache/cnpm \
--disturl=https://npm.taobao.org/dist \
--userconfig=$HOME/.cnpmrc"
```

#### 加载相关模块依赖
```bash
cnpm install
```
#### 修改项目
现在你已经成功运行了项目，我们可以开始尝试动手改一改了：

> 使用你喜欢的编辑器打开index.ios.js并随便改上几行。
> 在iOS Emulator中按下Commond-R就可以刷新APP并看到你的最新修改！

#### 运行项目
> IOS：用Xcode打开运行
找到ios文件夹中Xcode启动图标。
Cmd + R 实现刷新
Cmd + D 弹出菜单

> Android：react-native run-android
通过终端进入文件夹，执行 react-native run-android，开启nodejs服务器进行编译。
首次运行需要等待数分钟并从网上下载gradle依赖。（这个过程屏幕上可能出现很多小数点，表示下载进度。这个时间可能耗时很久，也可能会不停报错链接超时、连接中断等等——取决于你的网络状况和墙的不特定阻断。总之要顺利下载，请使用稳定有效的科学上网工具。）
如果apk安装运行出现报错，请检查上文中安装SDK的环节里所有依赖是否都已装全，platform-tools是否已经设到了PATH环境变量中，运行adb devices能否看到设备。

至此，应该能看到APP红屏报错，这是正常的，我们还需要让app能够正确访问pc端的packager服务。

摇晃设备或按Menu键（Bluestacks模拟器按键盘上的菜单键，通常在右Ctrl的左边 或者左Windows键旁边），可以打开调试菜单，点击Dev Settings，选Debug server host for device，输入你的正在运行packager的那台电脑的局域网IP加:8081（同时要保证手机和电脑在同一网段，且没有防火墙阻拦），再按back键返回，再按Menu键，在调试菜单中选择Reload JS，就应该可以看到运行的结果了。如果真实设备白屏但没有弹出任何报错，可以在安全中心里看看是不是应用的“悬浮窗”的权限被禁止了。

注：真机安装（打开开发者调试工具、文件传输）

#### 启动Node服务
```bash
react-native start
```

### 相关资料：
[搭建开发环境](http://reactnative.cn/docs/0.31/getting-started.html "")
[在Windows下搭建React Native Android开发环境](http://reactnative.cn/post/10 "")
[在Windows下搭建React Native Android开发环境](http://bbs.reactnative.cn/topic/10/%E5%9C%A8windows%E4%B8%8B%E6%90%AD%E5%BB%BAreact-native-android%E5%BC%80%E5%8F%91%E7%8E%AF%E5%A2%83 "")
[在设备上运行](http://reactnative.cn/docs/0.31/running-on-device-android.html#content "")

## 项目结构

```
ReactNative     (项目名称)
|–node_modules                  node模块
    |–react-native              ReactNative引用工程文件
|–app                           app页面
    |–index.android.js          android工程备份
    |–index.ios.js              ios工程备份
|–index.android.js              android工程（开发文件）
|–index.ios.js                  ios工程（开发文件）
|–android              android项目
|–ios                  ios项目
    |–*.xcodeproj               Xcode启动文件
|–package.json         工程信息数据
```
注：android与ios有什么区别？
关于android与ios开发，大部分只要将开发好的文件相互拷贝，修改android与ios独有的部分控件即可。整体的逻辑思路保持一致即可。

### 设备调试工具
摇晃设备或按Menu键

| chance | 选项 |
| -----| -----|
| Reload    | 刷新 |
| Debug Js Remotely    | 远程调试js |
| Enable Live Reload    | 启动实时刷新 |
| Enable Hot Reloading    | 启动热刷新 |
| Toggle Inspector    | 标签调试 |
| Show Perf Monitor    | 显示性能监视器 |
| Capture Heap    |  |
| Start/Stop Sampling Profiler    | 启动/停止检测器 |
| Dev Settings    | 设备设置 |


#### Debug Js Remotely   js远程调试

此时，会打开页面调试Tab页面[Tab页面](http://localhost:8081/debugger-ui "")，可以用浏览器访问[android](http://localhost:8081/index.android.bundle?platform=android "link")看看是否可以看到打包后的脚本（看到很长的js代码就对了）。第一次访问通常需要十几秒，并且在packager的命令行可以看到形如[====]的进度条。

如果你遇到了ERROR Watcher took too long to load的报错，请尝试修改node_modules/react-native/packager/react-packager/src/FileWatcher/index.js，将其中的MAX_WAIT_TIME 从25000改为更大的值（单位是毫秒）

#### Enable Live Reload   启动实时刷新

#### Enable Hot Reloading   启动热刷新
[React Native 热加载（Hot Reload）原理简介](http://mp.weixin.qq.com/s?__biz=MzAwMTYwNzE2Mg==&mid=2651036597&idx=1&sn=8169e1d806ebece54403ff6902b05e36#rd&utm_source=tuicool&utm_medium=referral "")

#### Toggle Inspector   标签调试

#### Show Perf Monitor   显示性能监视器

#### Capture Heap   

#### Start/Stop Sampling Profiler   启动/停止检测器

#### Dev Settings   设备设置
[]( "")
Bebugging 调试
Debug server host & port for device 调试服务器主机和端口

```
adb devices 查询设备ID

adb reverse tcp:8081 tcp:8081
```


#### 提示信息
应用内的错误与警告提示（红屏和黄屏）#红屏或黄屏提示都只会在开发版本中显示，正式的离线包中是不会显示的。

[React Native调试技巧与心得](http://blog.csdn.net/quanqinyang/article/details/52215652 "")

#### 简单的列表Demo

```
	import React, { Component } from 'react';
	import {
	  AppRegistry,
	  StyleSheet,
	  Text,
	  View,
	  ScrollView,
	  Image,
	} from 'react-native';

	export default class luumans extends Component {
	  render() {
	    return (
	      <ScrollView style={styles.container}>
	        <Image
	          source={{uri: 'http://jiuye-res.jikexueyuan.com/zhiye/showcase/attach-/20161013/2a7bf0a0-d94d-40d4-a244-20e5a5e359e6.jpg'}}
	          style={styles.images}
	        />
	        <Text style={styles.title}>『微信小程序』从基础到实战</Text>
	        <Text style={styles.teacher}>勾股</Text>
	        <Text style={styles.time}>2013-07-11</Text>
	        <Image
	          source={{uri: 'http://jiuye-res.jikexueyuan.com/zhiye/showcase/attach-59b4a27d-e431-4f49-aa25-6b94cccd8229.jpg'}}
	          style={styles.images}
	        />
	        <Text style={styles.title}>基于Go语言的短链接服务实战</Text>
	        <Text style={styles.teacher}>小鱼</Text>
	        <Text style={styles.time}>2013-07-11</Text>
	        <Image
	          source={{uri: 'http://jiuye-res.jikexueyuan.com/zhiye/showcase/attach-0da69660-4fcc-45d1-9b84-88271851f57f.jpg'}}
	          style={styles.images}
	        />
	        <Text style={styles.title}>基于Python的静态爬虫实战</Text>
	        <Text style={styles.teacher}>飞雪</Text>
	        <Text style={styles.time}>2013-07-11</Text>
	      </ScrollView>
	    );
	  }
	}

	const styles = StyleSheet.create({
	  container: {
	    flex: 1,
	    backgroundColor: '#F2F2F2',
	    margin: 5,
	    borderWidth: 1,
	    borderColor: '#d2d2d2',
	  },
	  title: {
	    fontSize: 15,
	    marginLeft: 10,
	    color: '#333333',
	    textAlign: 'left',
	  },
	  images: {
	    height: 200,
	    margin: 10,
	  },
	  teacher: {
	    fontSize: 13,
	    marginLeft: 10,
	    color: '#525252',
	    textAlign: 'left',
	  },
	  time: {
	    fontSize: 13,
	    marginLeft: 10,
	    color: '#2d854a',
	    textAlign: 'left',
	  },
	});


	AppRegistry.registerComponent('luumans', () => luumans);
```
列表控件Listview：

## Flexbox布局
![](http://www.th7.cn/d/file/p/2016/08/30/532c0a8f1bc8b3d4037a61d7efc61d36.jpg)
### 什么事Flexbox
Flexbox是css 3中引入的布局模型“弹性盒子模型”，通过弹性的方式来对齐和分布容器中的内容空间，使其能够适应不同屏幕的宽度。React Native中Flexbox是这个规范的子集。

### 解决问题
浮动布局
不同宽度屏幕的适配
宽度自动分配
水平垂直居中

### Flexbox模型
![](http://img.caibaojian.com/uploads/2014/05/flexbox.png)
一系列属性的集合：
基本上，项目将制定了以下任一主轴（从 main-start 到 main-end）或十字轴（(从 cross-start 到 cross-end）。

### Flexbox属性讲解

| 容器属性 | 元素属性 |
|   -----  |   -----  |
| flexDirection     | flex |
| flexWrap          | alignSelf |
| alignltems        | |
| justifyContent    | |

#### [flexDirection](http://caibaojian.com/demo/flexbox/flex-direction.html "演示")(适用于父类容器的元素上)
![](http://ww1.sinaimg.cn/mw690/660d0cdfjw1etlhxise4kj218g0xc0vf.jpg)
```bash
flexDirection : row | row-reverse | column | column-reverse（好像不支持）
row：横向布局
column：纵向布局（默认）
```

> row：横向布局

```javascript
	import React, { Component } from 'react';
	import {
	  AppRegistry,
	  StyleSheet,
	  Text,
	  View,
	} from 'react-native';

	export default class luumans extends Component {
	  render() {
	    return (
	      <View style={styles.flexContainer}>
	        <View style={styles.cell}>
	          <Text style={styles.welcome}>Row1</Text>
	        </View>
	        <View style={styles.cell}>
	          <Text style={styles.welcome}>Row2</Text>
	        </View>
	        <View style={styles.cell}>
	          <Text style={styles.welcome}>Row3</Text>
	        </View>
	      </View>
	    );
	  }
	}

	const styles = StyleSheet.create({
	  flexContainer: {
	    flexDirection: 'row',
	    backgroundColor: '#aaaaaa',
	  },
	  cell: {
	    height: 50,
	  },
	  welcome: {
	    fontSize: 14,
	    textAlign: 'center',
	    width: 80,
	    margin: 10,
	    height: 30,
	    color: '#FFF',
	    backgroundColor: '#000',
	  },
	});

	AppRegistry.registerComponent('luumans', () => luumans);
```

> column：纵向布局（默认）

```javascript
	import React, { Component } from 'react';
	import {
	  AppRegistry,
	  StyleSheet,
	  Text,
	  View,
	} from 'react-native';

	export default class luumans extends Component {
	  render() {
	    return (
	      <View style={styles.flexContainer}>
	        <View style={styles.cell}>
	          <Text style={styles.welcome}>Row1</Text>
	        </View>
	        <View style={styles.cell}>
	          <Text style={styles.welcome}>Row2</Text>
	        </View>
	        <View style={styles.cell}>
	          <Text style={styles.welcome}>Row3</Text>
	        </View>
	      </View>
	    );
	  }
	}

	const styles = StyleSheet.create({
	  flexContainer: {
	    flexDirection: 'column',
	    backgroundColor: '#aaaaaa',
	  },
	  cell: {
	    height: 50,
	  },
	  welcome: {
	    fontSize: 20,
	    textAlign: 'center',
	    margin: 10,
	    color: '#FFF',
	    backgroundColor: '#000',
	  },
	});

	AppRegistry.registerComponent('luumans', () => luumans);
```

#### [flex-wrap](http://caibaojian.com/demo/flexbox/flex-wrap.html "演示")
![](http://ww1.sinaimg.cn/mw690/660d0cdfjw1etlhxjtkfwj218g0xcwhp.jpg)
```bash
flex-wrap: nowrap | wrap

nowrap：当子元素溢出父容器时不换行。
wrap：当子元素溢出父容器时自动换行。
```

> wrap：当子元素溢出父容器时自动换行。

```javascript
	import React, { Component } from 'react';
	import {
	  AppRegistry,
	  StyleSheet,
	  Text,
	  View,
	} from 'react-native';

	export default class luumans extends Component {
	  render() {
	    return (
	      <View style={styles.flexContainer}>
	          <Text style={styles.welcome}>Row1</Text>
	          <Text style={styles.welcome}>Row2</Text>
	          <Text style={styles.welcome}>Row3</Text>
	          <Text style={styles.welcome}>Row3</Text>
	          <Text style={styles.welcome}>Row3</Text>
	          <Text style={styles.welcome}>Row3</Text>
	          <Text style={styles.welcome}>Row3</Text>
	          <Text style={styles.welcome}>Row3</Text>
	          <Text style={styles.welcome}>Row3</Text>
	      </View>
	    );
	  }
	}

	const styles = StyleSheet.create({
	  flexContainer: {
	    flexDirection: 'row',
	    flexWrap: 'wrap',
	    backgroundColor: '#aaaaaa',
	  },
	  
	  welcome: {
	    fontSize: 14,
	    textAlign: 'center',
	    width: 80,
	    margin: 10,
	    height: 30,
	    color: '#FFF',
	    backgroundColor: '#000',
	  },
	});

	AppRegistry.registerComponent('luumans', () => luumans);
```

#### [alignltems](http://caibaojian.com/demo/flexbox/align-items.html "演示")
![](http://ww4.sinaimg.cn/mw690/660d0cdfjw1etlhxkrusyj218g0xcwhn.jpg)
```bash
align-items: flex-start | flex-end | center | stretch

flex-start：弹性盒子元素的侧轴（纵轴）起始位置的边界紧靠住该行的侧轴起始边界。
flex-end：弹性盒子元素的侧轴（纵轴）起始位置的边界紧靠住该行的侧轴结束边界。
center：弹性盒子元素在该行的侧轴（纵轴）上居中放置。（如果该行的尺寸小于弹性盒子元素的尺寸，则会向两个方向溢出相同的长度）。
stretch：如果指定侧轴大小的属性值为'auto'，则其值会使项目的边距盒的尺寸尽可能接近所在行的尺寸，但同时会遵照'min/max-width/height'属性的限制。
```

> center：

```javascript
	import React, { Component } from 'react';
	import {
	  AppRegistry,
	  StyleSheet,
	  Text,
	  View,
	} from 'react-native';

	export default class luumans extends Component {
	  render() {
	    return (
	      <View style={styles.flexContainer}>
	          <Text style={styles.welcome}>Row1</Text>
	          <Text style={styles.welcome2}>Row2</Text>
	          <Text style={styles.welcome3}>Row3</Text>
	      </View>
	    );
	  }
	}

	const styles = StyleSheet.create({
	  flexContainer: {
	    flexDirection: 'row',
	    flexWrap: 'wrap',
	    backgroundColor: '#aaaaaa',
	    alignItems: 'center',
	  },
	  
	  welcome: {
	    fontSize: 14,
	    textAlign: 'center',
	    width: 80,
	    margin: 10,
	    height: 30,
	    color: '#FFF',
	    backgroundColor: '#000',
	  },
	  welcome2: {
	    fontSize: 14,
	    textAlign: 'center',
	    width: 80,
	    margin: 10,
	    height: 40,
	    color: '#FFF',
	    backgroundColor: '#000',
	  },
	  welcome3: {
	    fontSize: 14,
	    textAlign: 'center',
	    width: 80,
	    margin: 10,
	    height: 50,
	    color: '#FFF',
	    backgroundColor: '#000',
	  },
	});

	AppRegistry.registerComponent('luumans', () => luumans);
```

> wrap：当子元素溢出父容器时自动换行。

```javascript
```

> wrap：当子元素溢出父容器时自动换行。

```javascript
```
#### [flex-flow](http://caibaojian.com/demo/flexbox/flex-flow.html "演示")
![]()

#### [justify-content](http://caibaojian.com/demo/flexbox/justify-content.html "演示")
![](http://ww3.sinaimg.cn/mw690/660d0cdfjw1etlhxjven9j218g0xcgp4.jpg)


#### [align-content](http://caibaojian.com/demo/flexbox/align-content.html "演示")
![](http://ww4.sinaimg.cn/mw690/660d0cdfjw1etlhxl3605j218g0xcwip.jpg)

#### [order](http://caibaojian.com/demo/flexbox/order.html "演示")
![]()

#### [flex-grow](http://caibaojian.com/demo/flexbox/flex-grow.html "演示")
![]()

#### [flex-shrink](http://caibaojian.com/demo/flexbox/flex-shrink.html "演示")

#### [flex-basis](http://caibaojian.com/demo/flexbox/flex-basis.html "演示")

#### [flex](http://caibaojian.com/demo/flexbox/flex.html "演示")

#### [align-self](http://caibaojian.com/demo/flexbox/align-self.html "演示")

#### [flexbox3](http://caibaojian.com/demo/flexbox/flexbox3.html "演示")

#### [flexbox4](http://caibaojian.com/demo/flexbox/flexbox4.html "演示")

#### [flexbox5](http://caibaojian.com/demo/flexbox/flexbox5.html "演示")
```bash

```
![]()

![]()


### 相关资料：
[facebook/react-native](https://github.com/facebook/react-native "A framework for building native apps with React.")
[A Complete Guide to Flexbox](https://css-tricks.com/snippets/css/a-guide-to-flexbox/ "")
[react-native 之布局篇](https://github.com/tmallfe/tmallfe.github.io/issues/19 "")
[flexbox-CSS3弹性盒模型flexbox完整版教程](http://caibaojian.com/flexbox-guide.html "")
[React-Native之flexbox布局篇](http://blog.csdn.net/u014486880/article/details/51385688 "")
[React Native专题](http://www.lcode.org/react-native/ "qing")
[React Native专题](http://godcoder.me/categories/%E6%8A%80%E6%9C%AF%E5%8D%9A%E5%AE%A2/React-Native/ "非著名程序员")

[Flexbox in the CSS specifications](http://www.w3.org/TR/css3-flexbox/)
[Flexbox at MDN](https://developer.mozilla.org/en-US/docs/CSS/Tutorials/Using_CSS_flexible_boxes)
[Flexbox at Opera](http://dev.opera.com/articles/view/flexbox-basics/)
[Diving into Flexbox by Bocoup](http://weblog.bocoup.com/dive-into-flexbox/)
[Mixing syntaxes for best browser support on CSS-Tricks](http://css-tricks.com/using-flexbox/)
[Flexbox by Raphael Goetter (FR)](http://www.alsacreations.com/tuto/lire/1493-css3-flexbox-layout-module.html)
[Flexplorer by Bennett Feely](http://bennettfeely.com/flexplorer/)
[http://devbryce.com/site/flexbox/](http://devbryce.com/site/flexbox/)
[http://css.doyoe.com/properties/flex/index.htm](http://css.doyoe.com/properties/flex/index.htm)
[http://css-tricks.com/snippets/css/a-guide-to-flexbox/](http://css-tricks.com/snippets/css/a-guide-to-flexbox/)
[样式测试](http://facebook.github.io/react-native/docs/style.html "")



[]( "")


问题：首页白屏
[ReactNative安卓首屏白屏优化](https://segmentfault.com/a/1190000004743424 "")

## react-native学习列表
收集了react-native一些学习资源，列表会继续更新，大家有好的资源欢迎Pull Requests！

### 官方文档
[React Native](http://facebook.github.io/react-native/ "English")
[React Native 中文网](http://reactnative.cn/ "最专业的翻译，最及时的资讯，最火爆的社区")
[官方视频](https://www.youtube.com/watch?v=KVZ-P-ZI6W4 "")
[react-native学习列表](https://github.com/joggerplus/ReactNativeRollingExamples/blob/master/react-native_Study_List.md "")
[React-Native入门指南](https://github.com/vczero/react-native-lesson "")
[整理了一份React-Native学习指南](http://www.tuicool.com/articles/zaInUbA "")
[深入浅出 React Native：使用 JavaScript 构建原生应用](http://www.cocoachina.com/ios/20150408/11513.html "")


[React Native系列文章](http://segmentfault.com/blog/cnsnake11_react_native "segmentfault")
[React Native系列文章](http://gold.xitu.io/#/tag/React%20Native "掘金")
[React Native中文社区](http://bbs.reactnative.cn/ "")
[天猫前端](https://github.com/tmallfe/tmallfe.github.io/issues "")


[React-native组件库](https://js.coach/react-native/ "")
[React Native Modules](http://reactnativemodules.com/ "")

#### 库
[react-native-desktop](https://github.com/ptmt/react-native-desktop "通过React Native构建macOS app") 
[react-native-code-push](https://github.com/Microsoft/react-native-code-push "微软出的热更新平台") 
[react-native-invoke](https://github.com/wix/react-native-invoke "从JS调用native的代码而不需要任何的封装") 

### 相关书籍
[ECMAScript 6入门](http://es6.ruanyifeng.com/ "阮一峰")
[React/React Native 的ES5 ES6写法对照表](http://bbs.reactnative.cn/topic/15/react-react-native-%E7%9A%84es5-es6%E5%86%99%E6%B3%95%E5%AF%B9%E7%85%A7%E8%A1%A8 "")

###教程
[awesome-react-native](https://github.com/jondot/awesome-react-native "")
[Airbnb JavaScript Style Guide 中文版](https://github.com/sivan/javascript-style-guide "")
[React 入门实例教程-阮一峰](http://www.ruanyifeng.com/blog/2015/03/react.html "")
[ReactJS中文文档](http://reactjs.cn/ "")
[react-native-guide](https://github.com/ele828/react-native-guide ":React Native指南汇集了react-native学习资源与各类开源app")
[React-Native-lesson](https://github.com/vczero/react-native-lesson ":React Native入门指南")
[ReactNativeDemo](https://github.com/WildDylan/ReactNativeDemo "")
[npm模块管理器](http://javascript.ruanyifeng.com/nodejs/npm.html "")
[快速入门-Grunt中文网](http://www.gruntjs.net/getting-started "")
[Redux 中文文档](http://camsong.github.io/redux-in-chinese/ "")
[reactjs.cn - Flux应用架构](http://reactjs.cn/react/docs/flux-overview.html "")
[cnsnake11研究react-native的blog](https://github.com/cnsnake11/blog "")
[Facebook F8App-ReactNative项目源码分析系列](http://www.jianshu.com/p/28e9c7957d0c "")
[React Native 学习笔记](https://github.com/Kennytian/learning-react-native "")
[构建 F8 App / React Native 开发指南](http://f8-app.liaohuqiu.net/ "")
[React Native：移动开发时代的巴别塔 - 专题分享](https://github.com/Code-T/salon-resources/tree/master/%E5%8C%97%E4%BA%AC%202016:05:28 "")

### 文章
[一个完整的Flexbox指南](http://www.w3cplus.com/css3/a-guide-to-flexbox.html "")
[组件的详细说明和生命周期（Component Specs and Lifecycle）](http://reactjs.cn/react/docs/component-specs.html "")
[JSX 中的 If-Else](http://reactjs.cn/react/tips/if-else-in-JSX.html"")

[组件间的通信](http://reactjs.cn/react/tips/communicate-between-components.html "")
[mozilla-闭包](https://developer.mozilla.org/cn/docs/Web/JavaScript/Closures "")
[npm的package.json中文文档](https://github.com/ericdum/mujiang.info/issues/6 "")
[快来使用ECMAScript 2015吧](http://bluereader.org/article/73541139 "")
[React-Native学习技术的三部曲](http://lijianfei.sinaapp.com/?p=888 "关于组件生命周期的讲得特别到位") 
[新手理解Navigator的教程](http://bbs.reactnative.cn/topic/20/%E6%96%B0%E6%89%8B%E7%90%86%E8%A7%A3navigator%E7%9A%84%E6%95%99%E7%A8%8B "对于Navigator讲解的特别详细")
[React/React Native 的ES5 ES6写法对照表](http://bbs.reactnative.cn/topic/15/eact-react-native-%E7%9A%84es5-es6%E5%86%99%E6%B3%95%E5%AF%B9%E7%85%A7%E8%A1%A8 "")

[一个“三端”开发者眼中的React Native](http://f2e.souche.com/blog/-ge-san-duan-kai-fa-zhe-yan-zhong-de-react-native/ "对react-native从各个层面有一个比较深入的见解")
[“指尖上的魔法” -- 谈谈React-Native中的手势](https://github.com/jabez128/jabez128.github.io/issues/1 "")
[ReactNative的组件架构设计](http://segmentfault.com/a/1190000004161358 "包括Flux、Reflux、Redux") 
[在react-native中使用redux](http://www.jianshu.com/p/2c43860b0532 "")
[怎么样桥接一个objective-c的视图组件](http://browniefed.com/blog/react-native-how-to-bridge-an-objective-c-view-component/ "")

#### 事件
[安卓Back键的处理·基本+高级篇](http://bbs.reactnative.cn/topic/480/%E5%AE%89%E5%8D%93back%E9%94%AE%E7%9A%84%E5%A4%84%E7%90%86-%E5%9F%BA%E6%9C%AC-%E9%AB%98%E7%BA%A7%E7%AF%87 "")

#### 音视频相机
[React Native 实现二维码扫描](http://gold.xitu.io/post/581755be2f301e005ce78a18?utm_source=gold_browser_extension "二维码扫描组件")
[react-native-barcodescanner](https://github.com/ideacreation/react-native-barcodescanner "二维码扫描组件")
[react-native-camera](https://github.com/lwansbrough/react-native-camera "相机组件")  
[react-native-image-picker](https://github.com/marcshilling/react-native-image-picker "可以从相机或者相册选择图片")  

#### 图形动画

#### 视图
[react-native-button](https://github.com/ide/react-native-button "按钮，因为react-native没有提供button") 
[react-native-scrollable-tab-view](https://github.com/skv-headless/react-native-scrollable-tab-view "滑动的tab视图") 

#### listview
[react-native-sglistview](https://github.com/sghiassy/react-native-sglistview "性能优化的listview") 
[react-native-tableview](https://github.com/aksonov/react-native-tableview "桥接了原生的UITableView") 

### 项目

#### Demo
[HTML5 CSS3 code sample](https://github.com/lixinso/html5 "")
[react-native-hybrid-app-examples](https://github.com/dsibiski/react-native-hybrid-app-examples "iOS原生项目集成react-native的示例项目") 
[react-native-redux-demo](https://github.com/ninty90/react-native-redux-demo "react-native使用redux的demo，结合这篇文章看效果更好，") 
[react-native中使用redux](http://www.jianshu.com/p/2c43860b0532 "")

#### Design
[React-Native-Gank](https://github.com/Bob1993/React-Native-Gank "低俗无聊的扎设计，看看代码德勒")
[f8app](https://github.com/fbsamples/f8app "facebook 官方f8 app")
[react-native-todo](https://github.com/joemaddalone/react-native-todo "一个简单的to do 应用程序 jast IOS") 
[noder-react-native](https://github.com/soliury/noder-react-native "Noder-cnodejs客户端") 
[FinanceReactNative](https://github.com/7kfpun/FinanceReactNative "Finance - 股票报价app")
[react-native-nw-react-calculator](https://github.com/benoitvallon/react-native-nw-react-calculator "iOS/Android、Web、桌面多端的计算器app")

#### Mode
[react-native-swiper](https://github.com/leecade/react-native-swiper "The best Swiper component for React Native.")

### 工具
[开源的react-native IDE](https://github.com/decosoftware/deco-ide "")
[rnpm](https://github.com/rnpm/rnpm "React Native的包管理器")
[react-native-vector-icons](https://github.com/oblador/react-native-vector-icons "为React Native集成了很多icon")
[redux](https://github.com/reactjs/redux "Redux 就是用来确保 state 变化的可预测性，仓库readme中的代码很简洁的描述了redux的内容")
[react-redux](https://github.com/reactjs/react-redux "官方的React绑定redux")
[redux-thunk](https://github.com/gaearon/redux-thunk "redux的thunk中间件")
[redux-persist](https://github.com/rt2zz/redux-persist "")
[alt](https://github.com/goatslacker/alt "flux的实现")


[携程技术中心React Native Meetup活动经验分享](http://blog.csdn.net/liu__520/article/details/52903667 "")


[]( "")
