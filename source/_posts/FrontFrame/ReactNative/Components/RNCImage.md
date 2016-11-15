title: React Native Image
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
看看官网的案例，提供两种来源的图片：本地、服务器。<!-- 当然它也支持在android中显 示GIF 和 WebP 图片，方式如下： -->

```
	import React, { Component } from 'react';
	import { AppRegistry, View, Image } from 'react-native';

	class DisplayAnImage extends Component {
	  render() {
	    return (
	      <View>
	        <Image
	          source={require('./img/favicon.png')}
	        />
	        <Image
	          style={{width: 50, height: 50}}
	          source={{uri: 'https://facebook.github.io/react/img/logo_og.png'}}
	        />
	      </View>
	    );
	  }
	}

	// App registration and rendering
	AppRegistry.registerComponent('DisplayAnImage', () => DisplayAnImage);
```

### 图片样式

关于图片样式的设计，介于前端开发我们会考虑适配问题，通过flex-rem来进行适配。但是做了demo发现以下特点：
1. Image组件必须设置图片高度，否则不会显示。本地图片可以不设置尺寸，会按照图片的大小显示，服务器上的图片必须设置图片高度。
1. 通过在两台Android手机测试发现，宽度可以不设置。高度不够，图片不会压扁，系统将截取中间部分，多余的图片将不会显示。（但是，官网提供的demo却不尽人意，原理是什么）
1. 想要通过PixelRatio来控制显示的大小，但是同样的屏幕设备设备像素相同，但是分辨率却不同1.5/3
1. React Native 布局样式的单位是不是 pt、px，而是 dp。

```
	'use strict';
	import React, { Component } from 'react';
	import {
	  AppRegistry,
	  StyleSheet,
	  Text,
	  PixelRatio,
	  Image,
	  ScrollView,
	  View,
	} from 'react-native';

	class StyleImg extends Component {
	  render(){
	    var url = ({
	      uri: 'http://7u2psp.com2.z0.glb.qiniucdn.com/576d0f99f1f58390384.jpg?imageView2/1/w/'+ 1000 * PixelRatio.get() +'/h/' + 1000 * PixelRatio.get() +'/q/100'
	    });
	    var image = ({
	      uri: 'http://7u2psp.com2.z0.glb.qiniucdn.com/576d0f99f1f58390384.jpg?imageView2/1/w/'+ 1000 * PixelRatio.get() +'/h/' + 1000 * PixelRatio.get() +'/q/100'
	    });
	    return (
	      <ScrollView>
	        <View style = {styles.pad}>
	          <Text style = {styles.pad}>图片裁剪是否压扁(同一片源)</Text>
	          <Image source={url} style={{width: 180,height: 240}} />
	          <Image source={url} style={{width: 180,height: 180}} />
	        </View>

	        <Text style = {styles.pad}>设备适配</Text>
	        <Text>PixelRatio：{PixelRatio.get()}</Text>
	        <Text style = {styles.pad}>高度{300 * PixelRatio.get()}</Text>
	        <Text>想用服务器的截图功能，但是好像rn实现了这样的功能。</Text>
	        <Image source={image} style={{height: 300 * PixelRatio.get()}} />

	        <Text style = {styles.pad}>PixelRatio 设备适配</Text>
	        <Text>width: 160 * {PixelRatio.get()} , height: 213 * {PixelRatio.get()}</Text>
	        <Image source={url} style={{width: 160 * PixelRatio.get(),height: 213 * PixelRatio.get()}} />
	      </ScrollView>
	    );
	  }
	}

	const styles = StyleSheet.create({
	  pad:{
	    padding: 10,
	  },
	});

	AppRegistry.registerComponent('StyleImg', () => StyleImg);
```