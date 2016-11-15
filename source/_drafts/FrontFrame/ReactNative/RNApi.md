title: React Native API
date: 2016-12-27 18:29:00
description: 
categories:
- FrontFrame
tags:
- ReactNative
toc: true
author:
comments:
original:
permalink: 
---

　　**自用笔记：**本文属于自用笔记，不做详解，仅供参考。在此记录自己已理解并开始遵循的前端代码规范。What How Why
<!-- more -->

## AppRegistry
AppRegistry模式是React Native中最基本的模块，也是最常用的模块。
AppRegistry模块是React Native应用JavaScript运行的入口。应用的跟组件应用使用AppRegistry.registerComponent进行注册自己。然后原生系统就可以进行加载运行bundle文件包，最后就会可以调用AppRegistry.runApplication进行运行起来应用。
当一个视图被摧毁的时候，为了结束应用可以调用AppRegistry.unmountApplictionComponentAtRootTag方法。其中该方法中的参数和runApplication中的要一样，该规则一定要遵守哦~

注：AppRegister模块需要在其他模块导入之前尽可能早点被导入进来来让JS环境可以正常运行。

### 属性方法
#### registerConfig(config:Array<AppConfig>)  static 静态方法, 进行注册配置信息

#### registerComponent(appKey:string,getComponentFunc:ComponentProvider)  static静态方法，进行注册组件

```
AppRegistry.registerComponent('luumans', () => luumans);
```

#### registerRunnable(appKey:string,func:Function)  static静态方法 ，进行注册线程

```
AppRegistry.registerRunnable('wxs',function(){
    console.log('was');
})
alert(AppRegistry.getAppKeys());
```

#### registerAppKeys()  static静态方法，进行获取所有组件的keys值

#### runApplication(appKey:string,appParameters:any)  static静态方法, 进行运行应用

#### unmountApplicationComponentAtRootTag()  static静态方法，结束应用

## PixelRatio
PixelRatio类提供了访问设备的像素密度的方法。

### 使用情景

```
	'use strict';
	import React, { Component } from 'react';
	import {
	  AppRegistry,
	  StyleSheet,
	  Text,
	  PixelRatio,
	  View,
	} from 'react-native';

	class luumans extends Component {
	  render(){
	    return (
	      <View style={styles.flexs}>
	        <View style={styles.container}>
	          <View style = {[styles.item,styles.center]}><Text style={styles.title}>酒店</Text>
	          </View>
	          <View style = {[styles.item,styles.lineLeftRight]}>
	            <View style={[styles.flexs,styles.center,styles.lineCenter]}><Text style={styles.title}>海外酒店</Text></View>
	            <View style={[styles.flexs,styles.center]}><Text style={styles.title}>特色酒店</Text></View>
	          </View>
	          <View style = {styles.item}>
	            <View style={[styles.flexs,styles.center,styles.lineCenter]}><Text style={styles.title}>团购</Text></View>
	            <View style={[styles.flexs,styles.center]}><Text style={styles.title}>客栈、公寓</Text></View>
	          </View>
	        </View>
	      </View>
	    )
	  }
	}

	const styles = StyleSheet.create({
	  container:{
	    flexDirection: 'row',
	    height: 80,
	    borderRadius: 5,
	    marginTop: 200,
	    marginLeft: 5,
	    marginRight: 5,
	    backgroundColor: '#FF0067',
	  },
	  item:{
	    flex: 1,
	    height: 80,
	  },
	  title:{
	    fontSize: 16,
	    fontWeight: 'bold',
	    color: '#FFF',
	  },
	  center:{
	    justifyContent: 'center',
	    alignItems: 'center',
	  },
	  flexs:{
	    flex: 1,
	  },
	  lineCenter: {
	    borderBottomWidth: 1/PixelRatio.get(),
	    borderColor: '#FFF',
	  },
	  lineLeftRight: {
	    borderLeftWidth: 1/PixelRatio.get(),
	    borderRightWidth: 1/PixelRatio.get(),
	    borderColor: '#FFF',
	  },
	});

	AppRegistry.registerComponent('luumans', () => luumans);
```

```
var image = getImage({
  width: PixelRatio.getPixelSizeForLayoutSize(200),
  height: PixelRatio.getPixelSizeForLayoutSize(100),
});
<Image source={image} style={{width: 200, height: 100}} />
```

```
	'use strict';
	  import React, { Component } from 'react';
	  import {
	    AppRegistry,
	    StyleSheet,
	    Text,
	    PixelRatio,
	    Image,
	    View,
	  } from 'react-native';

	  class luumans extends Component {
	    render(){
	      var image = ({
	        uri: 'http://7u2psp.com2.z0.glb.qiniucdn.com/56ceb9c406041348950.jpg?imageView2/1/w/'+ 1600 * PixelRatio.get() +'/h/' + 1000 * PixelRatio.get() +'/q/100'
	      });
	      return (
	        <View>
	          <Image source={{uri: 'http://7u2psp.com2.z0.glb.qiniucdn.com/56ceb9c406041348950.jpg?imageView2/1/w/90/h/90/q/100'}} style={{width: 90,height: 90}} />
	          <Image source={{uri: 'http://7u2psp.com2.z0.glb.qiniucdn.com/56ceb9c406041348950.jpg?imageView2/1/w/90/h/90/q/100'}} style={{height: 90}} />
	          <Image source={{uri: 'http://7u2psp.com2.z0.glb.qiniucdn.com/56ceb9c406041348950.jpg?imageView2/1/w/480/h/90/q/100'}} style={{height: 30 * PixelRatio.get()}} />
	          <Image source={image} style={{height: 100 * PixelRatio.get()}} />
	        </View>
	      );
	    }
	  }

	  AppRegistry.registerComponent('luumans', () => luumans);
```

### 属性方法

#### get() 返回设备的像素密度

```
PixelRatio.get() === 1
	mdpi Android 设备 (160 dpi)
PixelRatio.get() === 1.5
	hdpi Android 设备 (240 dpi)
PixelRatio.get() === 2
	iPhone 4, 4S
	iPhone 5, 5c, 5s
	iPhone 6
	xhdpi Android 设备 (320 dpi)
PixelRatio.get() === 3
	iPhone 6 plus
	xxhdpi Android 设备 (480 dpi)
PixelRatio.get() === 3.5
	Nexus 6
```

#### getFontScale() 返回字体大小缩放比例
#### getPixelSizeForLayoutSize() 将一个布局尺寸(dp)转换为像素尺寸(px)
#### startDetecting() 本函数在移动设备上没有作用。
[]( "")
