




	├── build/               # Webpack 配置目录
		├── dev-client.js            # npm run dev
		├── prod.js                  # npm run build
		├── webpack.base.conf.js     # 基础配置 全局路径原型函数
		├── webpack.dev.conf.js      # dev执行
		└── webpack.prod.conf.js     # build执行
	├── config/               # Webpack 配置目录
		├── dev.env.js                   # 端口与路径配置
		├── index.js                     # 端口与路径配置
		├── pro.env.js                   # 端口与路径配置
	├── dist/                # build 生成的生产环境下的项目


### 运行项目
npm run dev 
npm run build
npm run 

### 配置代理




### 插件的安装
插件安装：
--save-dev
--save
。这两个有一定的区别，我们都知道package.json  中有一个 “dependencies” 和 “devDependencies” 的。dependencies 是用在开发完上线模式的，就是有些东西你上线以后还需要依赖的，比如juqery , 我们这里的vue 和 babel-runtime（Babel 转码器 可以将ES6 转为ES5 ）， 而devDependencies 则是在开发模式执行的，比如我们如果需要安装一个node-sass 等等。有的时候看到package.json中安装的模块版本号前面有一个波浪线。例如: ~1.2.3 这里表示安装1.2.x以上版本。但是不安装1.3以上。

>以npm安装msbuild为例：

	npm install msbuild

会把msbuild包，安装到node_modules目录中，不会修改package.json，之后运行npm install命令时，不会自动安装msbuild

	npm install --save

会把msbuild包安装到`node_modules`目录中，会在package.json的dependencies属性下添加msbuild，之后运行npm install命令时，会自动安装msbuild到`node_modules`目录中，之后运行npm install --production或者注明NODE_ENV变量值为production时，会自动安装msbuild到`node_modules`目录中

	npm install --save-dev

会把msbuild包安装到`node_modules`目录中，会在package.json的devDependencies属性下添加msbuild，之后运行npm install命令时，会自动安装msbuild到`node_modules`目录中，之后运行npm install --production或者注明`NODE_ENV`变量值为production时，不会自动安装msbuild到node_modules目录中

>使用原则：

运行时需要用到的包使用--save，否则使用--save-dev。





- []( "")
- []( "")
- []( "")
- []( "")
- []( "")
- []( "")
- []( "")
- []( "")
- []( "")