title: React Native 基础
date: 2016-12-26 10:29:00
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
React Native看起来和react.js很像，最近也有不少框架开源，Weex基于Vue.js。ReactNative通过自定义组件，来实现接近原生操作，实现开平台效果。

官方文档地址：
[Props](https://facebook.github.io/react-native/docs/props.html "")
[state](https://facebook.github.io/react-native/docs/state.html "")
[style](https://facebook.github.io/react-native/docs/style.html "")

## props 属性
大多数组件在创建的时候就可以用各种参数来进行定制。用于定制的这些参数就称为props（属性）。所谓props，就是属性传递，而且是单向传递的。属性多的时候，可以传递一个对象，这是es6中的语法。

### 案例

```
	import React, { Component } from 'react';
	import { AppRegistry, Image } from 'react-native';
	class luumans extends Component {
	  render() {
		let pic = {
		  uri: 'https://upload.wikimedia.org/wikipedia/commons/d/de/Bananavarieties.jpg'
		};
		return (
		  <Image source={pic} style={{width: 193, height: 110}}/>
		);
	  }
	}
	AppRegistry.registerComponent('luumans', () => luumans);
```
当我们使用Image组件，可以使用source的props属性uri来控制显示什么图片。

注意：
注意{pic}外围有一层括号，我们需要用括号来把pic这个变量嵌入到JSX语句中。我们可以把任意合法的JavaScript表达式通过括号嵌入到JSX语句中。

为了更好的说明props的用法和概念，我把上面的例子又修改了一下，我的这个例子只是为了更好的说明props属性的用法，不建议大家这么使用，毕竟image是现成的基础组件。

### 官网自定义属性

```
	import React, { Component } from 'react';
	import {
	  AppRegistry,
	  StyleSheet,
	  Text,
	  Image,
	  View
	} from 'react-native';
	class Img extends Component {
	  render() {
		return (
		  <Image source={this.props.url} style={{width: 120, height: 80}}/>
		);
	  }
	}
	class UrlProps extends Component {
	  render() {
		let pic = {
		  uri: 'https://upload.wikimedia.org/wikipedia/commons/d/de/Bananavarieties.jpg'
		};
		return (
		  <View style={{padding: 10}}>
			<Img url ={pic}/>
		  </View>
		);
	  }
	}
	AppRegistry.registerComponent('UrlProps', () => UrlProps);
```

自己定义了个自定义组件Img，定义了个image的属性，通过单向数据传递实现。在自定义的Img组件中的，Image组件中引用了我们定义的image属性。这样一对比，可能大家就更能清楚的理解了props的用法了。说白了就是定制属性，然后传值。

注意：image是小些的， 大些的Image是官方图片基础组件。

### 自定义属性

```
	import React, { Component } from 'react';
	import { AppRegistry, Text, View } from 'react-native';
	class Greeting extends Component {
	  render() {
		return (
		  <Text>Hello {this.props.name}!</Text>
		);
	  }
	}
	class LotsOfGreetings extends Component {
	  render() {
		return (
		  <View style={{alignItems: 'center'}}>
			<Greeting name='Rexxar' />
			<Greeting name='Jaina' />
			<Greeting name='Valeera' />
		  </View>
		);
	  }
	}
	AppRegistry.registerComponent('LotsOfGreetings', () => LotsOfGreetings);
```

意思就是：自定义了一个名为Greeting的组件，然后，属性名为name，传不同的name值，在Text显示不同的名字。

## state

React靠一个state来维护状态，当state发生变化则更新DOM。控制一个组件，一般有两种数据类型，一种是props，一种是state。props是在父组件中设置，一旦指定，它的生命周期是不可以改变的。对于组件中数据的变化，我们是通过state来控制的。
一般情况下，我们初始化state状态，是在constructor构造函数中，然后如果需要改变时，我们可以调用setState方法。官方给的例子时一个闪烁的文字的例子，看看官网的例子是如何制作文字闪烁效果的。

### 定时刷新

```
	import React, { Component } from 'react';
	import { AppRegistry, Text, View } from 'react-native';
	class RTitle extends Component {
	  constructor(props) {
		super(props);
		this.state = { showText: true };
		// 每1000毫秒对showText状态做一次取反操作
		setInterval(() => {
		  this.setState({ showText: !this.state.showText });
		}, 1000);
	  }
	  render() {
		// 根据当前showText的值决定是否显示text内容
		let display = this.state.showText ? this.props.text : ' ';
		return (
		  <Text>{display}</Text>
		);
	  }
	}
	class ReloadTitle extends Component {
	  render() {
		return (
		  <View>
			<RTitle text='I love to RTitle' />
			<RTitle text='Yes RTitleing is so great' />
			<RTitle text='Why did they ever take this out of HTML' />
			<RTitle text='Look at me look at me look at me' />
		  </View>
		);
	  }
	}
	AppRegistry.registerComponent('ReloadTitle', () => ReloadTitle);
```

自定义了一个RTitle组件，在构造函数中初始化了state，然后写了一个定时器，每个1秒改变一次状态，然后setState,然后在渲染render()方法中，判断状态的变化，如果是true，显示文字，false显示空。这样一闪一闪的效果就出来了。
然后我们在ReloadTitle中使用RTitle组件，并传入我们需要的文字内容即可。
其实在实际开发中，我们不需要设置定时器来改变状态，一般情况下，我们都是在获取到服务器的数据时或者用户输入时，更新状态去显示最新的数据。这是我们就是通过setState来做到的。

## StyleSheet
在React Native中，你并不需要学习什么特殊的语法来定义样式。我们仍然是使用JavaScript来写样式。所有的核心组件都接受名为style的属性。这些样式名基本上是遵循了web上的CSS的命名，只是按照JS的语法要求使用了驼峰命名法，例如将background-color改为backgroundColor。

```
const [name] = StyleSheet.create({
  [name]: {
  },
});
```

### 样式引入
1. 外联
```
<View style = {styles.item}></View>
```
1. 内联
```
<View style = {{flex: 1,height: 80,borderWidth: 1,borderColor: '#000',}}></View>
```
1. 多个样式
```
<View style = {[styles.item,styles.items,{flex: 1,height: 80,borderWidth: 1,borderColor: '#000',}]}></View>
```

### TitleStyle

```
	import React, { Component } from 'react';
	import { AppRegistry, StyleSheet, Text, View } from 'react-native';
	class TitleStyle extends Component {
	  render() {
	    return (
	      <View>
	        <Text style={styles.red}>just red</Text>
	        <Text style={styles.bigblue}>just bigblue</Text>
	        <Text style={[styles.bigblue, styles.red]}>bigblue, then red</Text>
	        <Text style={[styles.red, styles.bigblue]}>red, then bigblue</Text>
	        <Text style={{color:'red' , fontSize:30}}>
	          Style 
	          <Text style={{color:'blue'}}>
	            Title
	          </Text>
	        </Text>
	      </View>
	    );
	  }
	}
	const styles = StyleSheet.create({
	  bigblue: {
	  color: 'blue',
	  fontWeight: 'bold',
	  fontSize: 30,
	  },
	  red: {
	  color: 'red',
	  },
	});
	AppRegistry.registerComponent('TitleStyle', () => TitleStyle);
```