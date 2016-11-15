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


```
import React, { Component } from 'react';
import {
  AppRegistry,
  StyleSheet,
  Text,
  ListView,
  Image,
  TouchableHighlight,
  View
} from 'react-native';
class ListViewDemo extends Component {
  constructor(props) {
	super(props);
	const ds = new ListView.DataSource({rowHasChanged: (r1, r2) => r1 !== r2});
	this.state = {
	  dataSource: ds.cloneWithRows(this._genRows(-1))
	};
  }
  _genRows(flag){
		const dataBlob = [];
		for(let i = 0 ; i< 88 ; i ++ ){
		  if(i == flag){
			dataBlob.push("非著名程序员+我被打了"+i);
		  }else{
			 dataBlob.push("非著名程序员"+i);
		  }
		}
		return dataBlob;
	}
  render() {
	return (
		<ListView
		  dataSource={this.state.dataSource}
		  renderRow={this._renderRow.bind(this)}
		/>
	);
  }
  _renderRow (rowData,sectionID, rowID) {
	return (
		<TouchableHighlight onPress={() => {
		  this._pressRow(rowData,rowID);
		}}
		underlayColor='red'
		>
		  <View>
			<View style={styles.row}>
			  <Image style={{width:40,height:40}} source={require('./Thumbnails/head.jpg')}/>
			  <Text style={{flex:1,fontSize:20,marginLeft:20}}>
				{rowData}
			  </Text>
			</View>
		  </View>
		</TouchableHighlight>
	);
   }
  _pressRow(rowData,rowID){
		alert(rowData);
		this.setState({dataSource: this.state.dataSource.cloneWithRows(
		this._genRows(rowID)
	)});
	}
}
const styles = StyleSheet.create({
  container: {
	flex: 1,
	justifyContent: 'center',
	alignItems: 'center',
	backgroundColor: '#F5FCFF',
  },
  row: {
	flexDirection: 'row',
	justifyContent: 'center',
	alignItems:'center',
	padding: 10,
  },
});
AppRegistry.registerComponent('ListViewDemo', () => ListViewDemo);
```
