title: React简单环境搭建
date: 2017-02-14 18:29:00
description: 
categories:
- React
tags:
- React
toc: true
author: 
comments:
original:
permalink: 
---
## 简单Demo

```
<!DOCTYPE html>
<html>
<head>
	<meta charset="UTF-8" />
	<title>Hello React!</title>
	<script src="http://static.runoob.com/assets/react/react-0.14.7/build/react.min.js"></script>
	<script src="http://static.runoob.com/assets/react/react-0.14.7/build/react-dom.min.js"></script>
	<script src="http://static.runoob.com/assets/react/browser.min.js"></script>
</head>
<body>
	<div id="example"></div>
	<script type="text/babel">
		ReactDOM.render(
			<h1>Hello, world!</h1>,
			document.getElementById('example')
		);
	</script>
</body>
</html>
```

### 引入依赖
实例中我们引入了三个库： react.min.js 、react-dom.min.js 和 browser.min.js：
react.min.js - React 的核心库
react-dom.min.js - 提供与 DOM 相关的功能
browser.min.js - 用于将 JSX 语法转为 JavaScript 语法

### React代码

	<script type="text/babel"></script>

<!-- more -->


## 使用npm React

### 安装全局依赖
使用Webpack搭建react环境，
```
$ npm install babel -g
$ npm install webpack -g
<!-- $ npm install webpack-dev-server --save-dev   # 或者  npm install webpack -g -->
```

### 创建根目录
创建一个根目录，目录名为：reactApp，再使用 npm init 初始化，生成 package.json 文件：

```
创建目录
$ mkdir reactApp
进入目录
$ cd reactApp/
安装package.json
$ npm init
	name: (reactApp) react-demo
	version: (1.0.0) 
	description: react demo
	entry point: (index.js) 
	test command: 
	git repository: 
	keywords: 
	author: 
	license: (ISC) 
About to write to /Users/tianqixin/www/reactApp/package.json:
{
	"name": "react-demo",
	"version": "1.0.0",
	"description": "react demo",
	"main": "index.js",
	"scripts": {
		"start": "webpack-dev-server --colors",
		"test": "echo \"Error: no test specified\" && exit 1"
	},
	"repository": {
		"type": "git",
		"url": "git@github.com:luuman/react-page.git"
	},
	"keywords": [
		"React"
	],
	"author": "luuman",
	"license": "ISC"
}
```
### 添加依赖包及插件
因为我们要使用 React, 所以我们需要先安装它，--save 命令用于将dependencies包添加至 package.json 文件。
```
$ npm install react --save
$ npm install react-dom --save
```

>同时我们也要安装一些 babel 插件

```
$ npm install babel-core --save
$ npm install babel-loader --save
$ npm install babel-preset-react --save
$ npm install babel-preset-es2015 --save
```
### 引入依赖
```
--save-dev
devDependencies里面的插件只用于开发环境，不用于生产环境

--save
dependencies是需要发布到生产环境的。
```

### 创建文件
接下来我们创建一些必要文件：
```
$ touch build/index.html
$ touch app/app.js
$ touch webpack.config.js
```

### 设置编译器，服务器，载入器
>webpack.config.js

```
const path = require('path');
const BrowserSyncPlugin = require('browser-sync-webpack-plugin');

module.exports = {
    entry: path.resolve(__dirname,'app/app.js'),
    output: {
        path: path.resolve(__dirname,'build'),
        filename: 'bundle.js'
    },
    devServer: {
       inline: true,
       port: 7777
    },
    module: {
        loaders: [{
            test: /\.jsx?$/,
            exclude: /node_modules/,
            loader: 'babel-loader',
            query: {
                presets: ['es2015','react']
            }
        },]
    }
};
```
### webpack config
#### plugins
#### entry入口文件
指定打包的入口文件 main.js。

#### output配置打包
配置打包结果，path定义了输出的文件夹，filename则定义了打包结果文件的名称。

#### devServer服务器端口
设置服务器端口号为 7777，端口后你可以自己设定 。

#### module加载器
module.loaders 是最关键的一块配置。它告知 webpack 每一种文件都需要使用什么加载器来处理：
```
module: {
    //加载器配置
    loaders: [
        //.css 文件使用 style-loader 和 css-loader 来处理
        { test: /\.css$/, loader: 'style-loader!css-loader' },

        //.js 文件使用 jsx-loader 来编译处理
        { test: /\.js$/, loader: 'jsx-loader?harmony' },

        //.scss 文件使用 style-loader、css-loader 和 sass-loader 来编译处理
        { test: /\.scss$/, loader: 'style!css!sass?sourceMap'},

        //图片文件使用 url-loader 来处理，小于8kb的直接转为base64
        { test: /\.(png|jpg)$/, loader: 'url-loader?limit=8192'}
    ]
}
```
如上，"-"loader其实是可以省略不写的，多个loader之间用“!”连接起来。
注意所有的加载器都需要通过 npm 来加载，并建议查阅它们对应的 readme 来看看如何使用。
拿最后一个 url-loader 来说，它会将样式中引用到的图片转为模块来处理，使用该加载器需要先进行安装：
npm install url-loader -save-dev
配置信息的参数“?limit=8192”表示将所有小于8kb的图片都转为base64形式（其实应该说超过8kb的才使用 url-loader 来映射到文件，否则转为data url形式）。

#### resolve其它解决方案配置
```
resolve: {
    //查找module的话从这里开始查找
    root: 'E:/github/flux-example/src', //绝对路径
    //自动扩展文件后缀名，意味着我们require模块可以省略不写后缀名
    extensions: ['', '.js', '.json', '.scss'],
    //模块别名定义，方便后续直接引用别名，无须多写长长的地址
    alias: {
        AppStore : 'js/stores/AppStores.js',//后续直接 require('AppStore') 即可
        ActionType : 'js/actions/ActionType.js',
        AppAction : 'js/actions/AppAction.js'
    }
}
```

#### 执行代码
现在打开 package.json 文件，找到 "scripts" 中的 "test" "echo \"Error: no test specified\" && exit 1" 使用以下代码替换："start": "webpack-dev-server --hot"替换后的 package.json 文件 内容如下：
```
$ cat package.json

{
  "scripts": {
    "start": "webpack-dev-server --colors",
    "build": "webpack"
  },
}
```

npm run start 命令来启动服务。
--hot 命令会在文件变化后重新载入，这样我们就不需要在代码修改后重新刷新浏览器就能看到变化。

### index.html
设置 div id = "app" 为我们应用的根元素，并引入 index.js 脚本文件。
```
<!DOCTYPE html>
<head>
	<meta charset="UTF-8">
	<title>Hacker News Front Page</title>
</head>
<body>
	<div id="example"></div>
	<script src="./bundle.js"></script>  
</body>
</html>
```

### 组件
>DivElement.js

```
//DivElement
import React from 'react';

class MyComponent extends React.Component{
    render() {
        return <div className="MyComponent">arr</div>;
    }
}
export default class DivElement extends React.Component{
    render() {
        return <MyComponent />;
    }
}
```

我们需要引入组件并将其渲染到根元素DivElement上，这样我们才可以在浏览器上看到它。
>app.js

```
//app.js
import React from 'react';
import ReactDOM from 'react-dom';
import DivElement from './DivElement.js';
const mountNode = document.getElementById('example');

ReactDOM.render(<DivElement />, mountNode);
```
注意：
如果想要组件可以在任何的应用中使用，需要在创建后使用 export 将其导出，在使用组件的文件使用 import 将其导入。

### 运行服务
完成以上配置后，我们即可运行该服务：

	$ npm run start
	执行webpack-dev-server --colors启动服务，开启高亮

通过浏览器访问 http://localhost:7777/，输出结果如下：









### browser-sync-webpack

    $ npm install browser-sync --save-dev
    $ npm install browser-sync-webpack --save-dev


[browser-sync-webpack](http://npm.taobao.org/package/browser-sync-webpack "")

const BrowserSyncPlugin = require('browser-sync-webpack');
new BrowserSyncPlugin({
  host: 'localhost',
  port: 3001,
  browsers: [],
  server: { baseDir: [ '__dirname' ] }
})
#### 首次尝试失败！不知为何无法显示页面




var ExtractTextPlugin = require('extract-text-webpack-plugin');
var WebpackNotifierPlugin = require('webpack-notifier');
var precss = require('precss');
var autoprefixer = require('autoprefixer');
var path = require('path');

var sassLoaders = [
  'css-loader',
  'postcss-loader',
  'sass-loader?includePaths[]=' + path.resolve(__dirname, './sass')
];

module.exports = {
  entry: './babel/index.js',
  output: {
    path: 'js',
    filename: 'index.js'
  },
  sassLoader: {
    includePaths: path.resolve(__dirname, './sass')
  },
  externals: {
    'TweenLite': 'TweenLite'
  },
  plugins: [
    new WebpackNotifierPlugin({
      title: 'Webpack',
      alwaysNotify: true
    }),
    new ExtractTextPlugin('../css/style.css'),
    new BrowserSyncPlugin({
      host: 'localhost',
      port: 3000,
      browsers: [],
      server: { baseDir: [ './' ] }
    })
  ],
  module: {
    loaders: [
      {
        test: /\.js$/,
        exclude: /node_modules/,
        loader: 'babel',
        query: {
          presets: ['es2015']
        }
      },
      {
        test: /\.scss$/,
        loader: ExtractTextPlugin.extract('style-loader', sassLoaders.join('!'))
      }
    ]
  },
  postcss: function() {
    return [
      precss,
      autoprefixer({ browsers: ['last 2 versions'] })
    ];
  }
};


