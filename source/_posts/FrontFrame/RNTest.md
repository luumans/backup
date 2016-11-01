title: React Native API
date: 2016-12-26 18:29:00
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

## JSX实战

Reactnative中没有DOM的概念，只有组件的概念，所以我们HTML标签、DOM操作是无效的。但是组件的生命周期、JSX语法、事件绑定、自定义属性等，ReactNative与React.js是一样的。

### 封装Box组件

```
	/**
	 * Sample React Native App
	 * https://github.com/facebook/react-native
	 */
	'use strict';
	import React, { Component } from 'react';
	import {
	  AppRegistry,
	  StyleSheet,
	  Text,
	  View
	} from 'react-native';

	class Box extends Component{
	  render(){
	    return (
	        <View style={[BoxStyles.box,BoxStyles[this.props.width],BoxStyles[this.props.height]]}>
	          <View  style={[BoxStyles.top,BoxStyles.height50,BoxStyles[this.props.classBg]]}><Text>top</Text></View>
	          <View style={[BoxStyles[this.props.childName]]}>
	            <View style={[BoxStyles.left,BoxStyles[this.props.classBg]]}><Text>left</Text></View>
	            {this.props.children}
	            <View style={[BoxStyles.right,BoxStyles[this.props.classBg]]}><Text>right</Text></View>
	          </View>
	          <View style={[BoxStyles.bottom,BoxStyles.height50,BoxStyles[this.props.classBg]]}><Text>bottom</Text></View>
	          <View style={[BoxStyles.label]}><Text>{this.props.boxName}</Text></View>
	        </View>
	    )
	  }
	}

	class MargginBox extends Component{
	  render(){
	    return (
	        <View style={[BoxStyles.margginBox]}>
	          <Box  childName="borderBox"  height="height400" width="width400" boxName="margin" classBg="bgred">{this.props.children}</Box>
	        </View>
	    )
	  }
	}

	class BorderBox extends Component{
	  render(){
	    return (
	        <Box childName="paddingBox"  height="height300" width="width300" boxName="border" classBg="bggreen" >{this.props.children}</Box>
	    )
	  }
	}

	class PaddingBox extends Component{
	  render(){
	    return (
	        <Box childName="elementBox"  height="height200" width="width200" boxName="padding" classBg="bgyellow" >{this.props.children}</Box>
	    )
	  }
	}

	class ElementBox extends Component{
	  render(){
	    return (
	        <View style={[BoxStyles.box,BoxStyles.height100]}>
	          <View style={[BoxStyles.measureBox]}>
	            <View style={[BoxStyles.right]}><Text>height</Text></View>
	          </View>
	          <View style={[BoxStyles.bottom,BoxStyles.height50]} ><Text>width</Text></View>
	          <View style={[BoxStyles.label]}><Text>element</Text></View>
	          <View style={[BoxStyles.widthdashed]}></View>
	          <View style={[BoxStyles.heightdashed]}></View>
	        </View>
	    )
	  }
	}

	class luumans extends Component {
	  render(){
	    return (
	        <MargginBox>
	          <BorderBox>
	            <PaddingBox>
	              <ElementBox>
	              </ElementBox>
	            </PaddingBox>
	          </BorderBox>
	        </MargginBox>
	    )
	  }
	}

	const BoxStyles = StyleSheet.create({
	  height50:{
	    height: 50,
	  },
	  height400:{
	    height: 400,
	  },
	  height300:{
	    height: 300,
	  },
	  height200:{
	    height: 200,
	  },
	  height100:{
	    height: 100,
	  },
	  width400:{
	    width: 400,
	  },
	  width300:{
	    width: 300,
	  },
	  width200:{
	    width: 200,
	  },
	  width100:{
	    width: 100,
	  },
	  bgred: {
	    backgroundColor:'#6AC5AC',
	  },
	  bggreen: {
	    backgroundColor: '#414142',
	  },
	  bgyellow: {
	    backgroundColor: '#D64078',
	  },
	  box: {
	    flexDirection: 'column',
	    flex: 1,
	    position: 'relative',
	  },
	  label: {
	    top: 0,
	    left: 0,
	    paddingTop: 0,
	    paddingRight: 3,
	    paddingBottom: 3,
	    paddingLeft: 0,
	    position: 'absolute',
	    backgroundColor: '#FDC72F',
	  },
	  top: {
	    justifyContent: 'center',
	    alignItems: 'center',
	  },
	  bottom: {
	    justifyContent: 'center',
	    alignItems: 'center',
	  },
	  right: {
	    width: 50,
	    justifyContent: 'space-around',
	    alignItems: 'center',
	  },
	  left: {
	    width: 50,
	    justifyContent: 'space-around',
	    alignItems: 'center',
	  },
	  heightdashed: {
	    bottom: 0,
	    top: 0,
	    right: 20,
	    borderLeftWidth: 1,
	    position: 'absolute',
	    borderLeftColor: '#FDC72F',
	  },
	  widthdashed: {
	    bottom: 25,
	    left: 0,
	    right: 0,
	    borderTopWidth: 1,
	    position: 'absolute',
	    borderTopColor: '#FDC72F',
	  },
	  yellow: {
	    color: '#FDC72F',
	    fontWeight:'900',
	  },
	  white: {
	    color: 'white',
	    fontWeight:'900',
	  },
	  margginBox:{
	    position: 'absolute',
	    top: 100,
	    paddingLeft:7,
	    paddingRight:7,
	  },
	  borderBox:{
	    flex: 1,
	    justifyContent: 'space-between',
	    flexDirection: 'row',
	  },
	  paddingBox:{
	    flex: 1,
	    justifyContent: 'space-between',
	    flexDirection: 'row',
	  },
	  elementBox:{
	    flex: 1,
	    justifyContent: 'space-between',
	    flexDirection: 'row',
	  },
	  measureBox:{
	    flex: 1,
	    flexDirection: 'row',
	    justifyContent: 'flex-end',
	    alignItems:'flex-end',
	  },
	  container: {
	    flex: 1,
	    justifyContent: 'center',
	    alignItems: 'center',
	    backgroundColor: '#F5FCFF',
	  },
	  welcome: {
	    fontSize: 20,
	    textAlign: 'center',
	    margin: 10,
	  },
	  instructions: {
	    textAlign: 'center',
	    color: '#333333',
	    marginBottom: 5,
	  },
	});

	AppRegistry.registerComponent('luumans', () => luumans);
```




