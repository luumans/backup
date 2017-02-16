title: React初探
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
## 概况
React 起源于 Facebook 的内部F8项目，用来架设 Instagram 的网站，并于 2013 年 5 月开源。
React主要用于构建UI，很多人认为 React 是 MVC 中的 V（视图）。
React 拥有较高的性能，代码逻辑非常简单，越来越多的人已开始关注和使用它。E6语法。

### React 特点
1. 声明式设计 −React采用声明范式，可以轻松描述应用。
1. 高效 −React通过对DOM的模拟，最大限度地减少与DOM的交互。
1. 灵活 −React可以与已知的库或框架很好地配合。
1. JSX − JSX 是 JavaScript 语法的扩展。React 开发不一定使用 JSX ，但我们建议使用它。
1. 组件 − 通过 React 构建组件，使得代码更加容易得到复用，能够很好的应用在大项目的开发中。
1. 单向响应的数据流 − React 实现了单向响应的数据流，从而减少了重复代码，这也是它为什么比传统数据绑定更简单。

- [官网地址](http://facebook.github.io/react/)
<!-- more -->

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
```
<script type="text/babel"></script>
```

## React JSX语法

React 使用 JSX 来替代常规的 JavaScript。JSX 是一个看起来很像 XML 的 JavaScript 语法扩展。我们不需要一定使用 JSX，但它有以下优点：

>JSX 执行更快，因为它在编译为 JavaScript 代码后进行了优化。
它是类型安全的，在编译过程中就能发现错误。
使用 JSX 编写模板更加简单快速。

### 简单嵌套元素
JSX 看起来类似 HTML ，我们可以看下实例:
ReactDOM.render方法接受两个参数：
一个虚拟 DOM 节点和一个真实 DOM 节点，作用是将虚拟 DOM 挂载到真实 DOM。

#### 实例：Hello, world!
```
ReactDOM.render(content,element);

ReactDOM.render(<h1>Hello, world!</h1>,document.getElementById('example'));
```
[index1](demo/index1.html )

### 复杂嵌套元素
我们可以在以上代码中嵌套多个 HTML 标签，需要使用一个 div 元素包裹它，实例中的 p 元素添加了自定义属性 data-myattribute，添加自定义属性需要使用 data- 前缀。

#### 实例：文字
```
ReactDOM.render(
    <div>
        <h1>菜鸟教程</h1>
        <h2>欢迎学习 React</h2>
        <p data-myattribute = "somevalue">这是一个很不错的 JavaScript 库!</p>
    </div>,
    mountNode
);
```
[index2](demo/index2.html )

### JavaScript 表达式
我们可以在 JSX 中使用 JavaScript 表达式。表达式写在花括号 {} 中。实例如下：

#### 实例：计算
```
ReactDOM.render(
    <div><h1>{1+1}</h1></div>,mountNode
);
```
[index3](demo/index3.html )

> 判断语句

在 JSX 中不能使用 if else 语句，但可以使用 conditional (三元运算) 表达式来替代。以下实例中如果变量 i 等于 1 浏览器将输出 true, 如果修改 i 的值，则会输出 false.

#### 实例：判断
```
const i = 1;
ReactDOM.render(
    <div>
        <h1>{i == 1 ? 'True!' : 'False'}</h1>
    </div>,
    mountNode
);
```
[index4](demo/index4.html )

### 样式
React 推荐使用内联样式。我们可以使用 camelCase 语法来设置内联样式. React 会在指定元素数字后自动添加 px 。以下实例演示了为 h1 元素添加 myStyle 内联样式：

#### 实例：CSS样式
```
const myStyle = {
    fontSize: 100,
    lineHeight: '30px',
    color: '#FF0000'
};

ReactDOM.render(
    <h1 style = {myStyle}>菜鸟教程</h1>,mountNode
);

ReactDOM.render(<h1 style = {{fontSize: 100,lineHeight: '30px',color: '#FF0000'}}>菜鸟教程</h1>,mountNode);

ReactDOM.render(<h1 className = 'class_name'>菜鸟教程</h1>,mountNode);
```
[index5](demo/index5.html )

### 注释
注释需要写在花括号中，实例如下：

### 实例：注释
```
ReactDOM.render(
    <div>
    <h1>菜鸟教程</h1>
    {/*注释...*/}
     </div>,mountNode
);
```

## React.Component组件
### 基础语法
### HTML 标签 vs. React 组件

React 可以渲染 HTML 标签 (strings) 或 React 组件 (classes)。要渲染 HTML 标签，只需在 JSX 里使用小写字母的标签名。
要渲染 React 组件，只需创建一个大写字母开头的本地变量。

#### 实例：创建组件
```
class DivElement extends React.Component{
    render() {
        return (
            <div className="foo">arr</div>
        );
    }
}
ReactDOM.render(<DivElement />, mountNode);
```

#### 实例：组件嵌套
```
class MyComponent extends React.Component{
    render() {
        return <div className="MyComponent">arr</div>;
    }
}
class DivElement extends React.Component{
    render() {
        return <MyComponent />;
    }
}
ReactDOM.render(<DivElement />, mountNode);
```
React 的 JSX 使用大、小写的约定来区分本地组件的类和 HTML 标签。

>注意:
由于 JSX 就是 JavaScript，一些标识符像 class 和 for 不建议作为 XML 属性名。作为替代，React DOM 使用 className 和 htmlFor 来做对应的属性。



#### 实例：组件语法
```
class HelloMessage extends React.Component{
    render() {
        return <div className="HelloMessage">arr</div>;
    }
}

class HelloMessage extends React.Component{
    render() {
        return (
            <div className="HelloMessage">arr</div>
        );
    }
}
```


React.Component方法用于生成一个组件类 HelloMessage。<HelloMessage /> 实例组件类并输出信息。
>注意：原生 HTML 元素名以小写字母开头，而自定义的 React 类名以大写字母开头，比如 HelloMessage 不能写成 helloMessage。除此之外还需要注意组件类只能包含一个顶层标签，否则也会报错。
如果我们需要向组件传递参数，可以使用 this.props 对象,实例如下：

#### 实例：获取父元素的值
```
class DivElement extends React.Component{
    render() {
        return (
            <div className="foo">{this.props.name}</div>
        );
    }
}
ReactDOM.render(<DivElement name="Runoob" />, mountNode);
```
以上实例中 name 属性通过 this.props.name 来获取（自身的数字）。
注意，在添加属性时， class 属性需要写成 className ，for 属性需要写成 htmlFor ，这是因为 class 和 for 是 JavaScript 的保留字。

### 复合组件
通过创建多个组件来合成一个组件，即把组件的不同功能点进行分离。
以下实例我们实现了输出网站名字和网址的组件：

#### 实例：链接
```
class WebSite extends React.Component{
    render() {
        return (
            <div className={this.props.name}><Name name={this.props.name} /><Link site={this.props.site} /></div>
        );
    }
}
class Name extends React.Component{
    render() {
        return (
            <h1>{this.props.name}</h1>
        );
    }
}
class Link extends React.Component{
    render() {
        return (
            <a href={this.props.site}>{this.props.site}</a>
        );
    }
}
ReactDOM.render(<WebSite name="菜鸟教程" site=" http://www.runoob.com" />, mountNode);
```

## React State(状态)
把组件看成是一个状态机（State Machines）。通过与用户的交互，实现不同状态，然后渲染 UI，让用户界面和数据保持一致。React 里，只需更新组件的 state，然后根据新的 state 重新渲染用户界面（不要操作 DOM）。

### constructor()初始状态

#### 实例：点击喜欢&不喜欢
```
class LikeButton extends React.Component{
    constructor() {
        super();
        this.state ={liked: false};
    }
    handleClick() {
        this.setState({
            liked: !this.state.liked
        });
    }
    render() {
        let text = this.state.liked ? '喜欢' : '不喜欢';
        return (
            <p onClick={this.handleClick.bind(this)}>你<b>{text}</b>我。点我切换状态。</p>
        );
    }
};
ReactDOM.render(<LikeButton />, mountNode);
```

```
handleClick = ()=>{
    this.setState({
        liked: !this.state.liked
    });
}
```
constructor是组件的构造函数，会在创建实例时自动调用。
...args表示组件参数，super(...args)是 ES6 规定的写法。
this.state对象用来存放内部状态，这里是定义初始状态，也就是一个对象，这个对象可以通过 this.state 属性读取。当用户点击组件，导致状态变化，this.setState 方法就修改状态值，每次修改以后，自动调用 this.render 方法，再次渲染组件。
onClick={this.handleClick.bind(this)}绑定事件，返回参数。
e.target.value绑定事件后的返回值。

#### 实例：输入文字实时显示
```
class MyTitle extends React.Component{
    constructor() {
        super();
        this.state ={name: 'can you speek English!'};
    }
    handleChange(e) {
        let name = e.target.value;
        this.setState({
            name: name
        });
    }
    render() {
        return (
            <div>
                <input type="text" onChange={this.handleChange.bind(this)} />
                <p>luuman,{this.state.name}</p>
            </div>
        );
    }
}
ReactDOM.render(<MyTitle />, mountNode);
```

## React Props
props通过组件获取数据
### 基础语法
#### 实例：数据传递
```
class HelloMessage extends React.Component{
	render(){
		return <h1>Hello {this.props.name}</h1>;
	}
}

ReactDOM.render(
	<HelloMessage name="Runoob" />,mountNode
);
```
实例中 name 属性通过 this.props.name 来获取。

### defaultProps默认值
默认Props：你可以通过defaultProps()方法为props设置默认值，实例如下：
```
class HelloMessage extends React.Component{
    render(){
        return <h1>Hello {this.props.name}</h1>;
    }
}
HelloMessage.defaultProps = {
    name: 'Runoob'
}
ReactDOM.render(<HelloMessage />,mountNode);
```

```
class WebSite extends React.Component{
    render() {
        return (
            <div className={this.props.name}><Name name={this.props.name} /><Link site={this.props.site} /></div>
        );
    }
}
WebSite.defaultProps ={
	name: "菜鸟教程",
	site: "http://www.runoob.com"
}
class Name extends React.Component{
    render() {
        return (
            <h1>{this.props.name}</h1>
        );
    }
}
class Link extends React.Component{
    render() {
        return (
            <a href={this.props.site}>{this.props.site}</a>
        );
    }
}
ReactDOM.render(<WebSite />, mountNode);
```

### this.props.children

#### 实例：点击次数
```
class NotesList extends React.Component{
    render(){
        return(
            <ol>{
                React.Children.map(this.props.children,function(child){
                    console.log(child);
                    return <li>{child}</li>
                })
            }</ol>
        );
    }
}

ReactDOM.render(
    <NotesList>
        <span>hello</span>
        <span>world</span>
        <span>world</span>
        <span>world</span>
    </NotesList>,
    mountNode
);
```

### PropTypes验证
Props 使用propTypes，它可以保证我们的应用组件被正确使用，React.PropTypes 提供很多验证器 (validator) 来验证传入数据是否有效。当向 props 传入无效数据时，JavaScript 控制台会抛出警告。

#### 实例：判断组件属性title是否为字符串：
```
const name = 123;
console.log(name);
class HelloMessage extends React.Component{
    render(){
        return <h1>Hello {this.props.title}</h1>;
    }
}
HelloMessage.propTypes = {
    title: React.PropTypes.string
}
ReactDOM.render(<HelloMessage title={name} />,mountNode);
```
>如果 title 使用数字变量，控制台会出现以下错误信息：

```
Warning: Failed prop type: Invalid prop `title` of type `number` supplied to `HelloMessage`, expected `string`.
```

### PropTypes属性值
```
.propTypes = {
	// 可以声明 prop 为指定的 JS 基本数据类型，默认情况，这些数据是可选的
	optionalArray: React.PropTypes.array,
	optionalBool: React.PropTypes.bool,
	optionalFunc: React.PropTypes.func,
	optionalNumber: React.PropTypes.number,
	optionalObject: React.PropTypes.object,
	optionalString: React.PropTypes.string,
	optionalSymbol: React.PropTypes.symbol,

	// 可以被渲染的对象 numbers, strings, elements 或 array
	optionalNode: React.PropTypes.node,

	//  React 元素
	optionalElement: React.PropTypes.element,

	// 用 JS 的 instanceof 操作符声明 prop 为类的实例。
	optionalMessage: React.PropTypes.instanceOf(Message),

	// 用 enum 来限制 prop 只接受指定的值。
	optionalEnum: React.PropTypes.oneOf(['News', 'Photos']),

	// 可以是多个对象类型中的一个
	optionalUnion: React.PropTypes.oneOfType([
		React.PropTypes.string,
		React.PropTypes.number,
		React.PropTypes.instanceOf(Message)
	]),

	// 指定类型组成的数组
	optionalArrayOf: React.PropTypes.arrayOf(React.PropTypes.number),

	// 指定类型的属性构成的对象
	optionalObjectOf: React.PropTypes.objectOf(React.PropTypes.number),

	// 特定 shape 参数的对象
	optionalObjectWithShape: React.PropTypes.shape({
		color: React.PropTypes.string,
		fontSize: React.PropTypes.number
	}),

	// 任意类型加上 `isRequired` 来使 prop 不可空。
	requiredFunc: React.PropTypes.func.isRequired,

	// 不可空的任意类型
	requiredAny: React.PropTypes.any.isRequired,

	// 自定义验证器。如果验证失败需要返回一个 Error 对象。不要直接使用 `console.warn` 或抛异常，因为这样 `oneOfType` 会失效。
	customProp(props, propName, componentName) {
		if (!/matchme/.test(props[propName])) {
		    return new Error(
				'Invalid prop `' + propName + '` supplied to' +
				' `' + componentName + '`. Validation failed.'
		    );
		}
	},
	customArrayProp: React.PropTypes.arrayOf(
		function(propValue, key, componentName, location, propFullName) {
			if (!/matchme/.test(propValue[key])) {
			    return new Error(
					'Invalid prop `' + propFullName + '` supplied to' +
					' `' + componentName + '`. Validation failed.'
			    );
			}
		}
	)
}
```

### state和props区别
在于props是不可变的，而子组件只能通过props来获取数据。
而state可以根据与用户交互来改变。这就是为什么有些容器组件需要定义state来更新和修改数据。

以下实例演示了如何在应用中组合使用state和props。我们可以在父组件中设置state，并通过在子组件上使用props将其传递到子组件上。在render函数中,我们设置name和site来获取父组件传递过来的数据。

#### 实例：链接
```
class WebSite extends React.Component{
	constructor(props) {
		super(props);
		this.state = {
			name: "菜鸟教程",
			site: "http://www.runoob.com"
		};
	}
	render(){
		return <div><Name name={this.state.name} /><Link site={this.state.site} /></div>
	}
}
class Name extends React.Component{
	render(){
		return <h1>{this.props.name}</h1>
	}
}
class Link extends React.Component{
	render(){
		return <a href={this.props.site}>{this.props.site}</a>
	}
}

ReactDOM.render(<WebSite />,mountNode);
```

















































## React 组件 API
在本章节中我们将讨论 React 组件 API。
### 基础语法

### mixins去重
```
const ExampleMixin = {
    componentDidMount(){
        // bind some event listeners here
    }
    componentWillUnmount(){
        // unbind those events here!
    }
}
class ExampleComponent extends React.Component{
	mixins: [ExampleMixin];
	render(){}
}
class AnotherComponent extends React.Component{
	mixins: [ExampleMixin];
	render(){}
}
```
<!-- 设置状态:setState
setState(object nextState[, function callback])
参数说明
nextState，将要设置的新状态，该状态会和当前的state合并
callback，可选参数，回调函数。该函数会在setState设置成功，且组件重新渲染后调用。
合并nextState和当前state，并重新渲染组件。setState是React事件处理函数中和请求回调函数中触发UI更新的主要方法。
关于setState
不能在组件内部通过this.state修改状态，因为该状态会在调用setState()后被替换。
setState()并不会立即改变this.state，而是创建一个即将处理的state。setState()并不一定是同步的，为了提升性能React会批量执行state和DOM渲染。
setState()总是会触发一次组件重绘，除非在shouldComponentUpdate()中实现了一些条件渲染逻辑。 -->

#### 实例：点击次数
```
class Counter extends React.Component{
	constructor(){
		super();
		this.state = {
			clickCount: 0
		};
	}
	handleClick(){
		this.setState({
			clickCount: this.state.clickCount +1
		});
	}
	render(){
		return <h2 onClick={this.handleClick.bind(this)}>点我！点击次数为: {this.state.clickCount}</h2>;
	}
}
ReactDOM.render(<Counter />,mountNode);
```
<!-- 
实例中通过点击 h2 标签来使得点击计数器加 1。
替换状态：replaceState
replaceState(object nextState[, function callback])
nextState，将要设置的新状态，该状态会替换当前的state。
callback，可选参数，回调函数。该函数会在replaceState设置成功，且组件重新渲染后调用。
replaceState()方法与setState()类似，但是方法只会保留nextState中状态，原state不在nextState中的状态都会被删除。
设置属性：setProps
setProps(object nextProps[, function callback])
nextProps，将要设置的新属性，该状态会和当前的props合并
callback，可选参数，回调函数。该函数会在setProps设置成功，且组件重新渲染后调用。
设置组件属性，并重新渲染组件。
props相当于组件的数据流，它总是会从父组件向下传递至所有的子组件中。当和一个外部的JavaScript应用集成时，我们可能会需要向组件传递数据或通知ReactDOM.render()组件需要重新渲染，可以使用setProps()。
更新组件，我可以在节点上再次调用ReactDOM.render()，也可以通过setProps()方法改变组件属性，触发组件重新渲染。
替换属性：replaceProps
replaceProps(object nextProps[, function callback])
nextProps，将要设置的新属性，该属性会替换当前的props。
callback，可选参数，回调函数。该函数会在replaceProps设置成功，且组件重新渲染后调用。
replaceProps()方法与setProps类似，但它会删除原有
props
强制更新：forceUpdate
forceUpdate([function callback])
参数说明
callback，可选参数，回调函数。该函数会在组件render()方法调用后调用。
forceUpdate()方法会使组件调用自身的render()方法重新渲染组件，组件的子组件也会调用自己的render()。但是，组件重新渲染时，依然会读取this.props和this.state，如果状态没有改变，那么React只会更新DOM。
forceUpdate()方法适用于this.props和this.state之外的组件重绘（如：修改了this.state后），通过该方法通知React需要调用render()
一般来说，应该尽量避免使用forceUpdate()，而仅从this.props和this.state中读取状态并由React触发render()调用。
获取DOM节点：findDOMNode
DOMElement findDOMNode()
返回值：DOM元素DOMElement
如果组件已经挂载到DOM中，该方法返回对应的本地浏览器 DOM 元素。当render返回null 或 false时，this.findDOMNode()也会返回null。从DOM 中读取值的时候，该方法很有用，如：获取表单字段的值和做一些 DOM 操作。
判断组件挂载状态：isMounted
bool isMounted()
返回值：true或false，表示组件是否已挂载到DOM中
isMounted()方法用于判断组件是否已挂载到DOM中。可以使用该方法保证了setState()和forceUpdate()在异步场景下的调用不会出错。
本文参考：http://itbilu.com/javascript/react/EkACBdqKe.html -->


## React 组件生命周期
>组件的生命周期可分成三个状态：Mounting、Updating、Unmounting
### Mounting：已插入真实 DOM

#### constructor()
#### componentWillMount()
在渲染前调用,在客户端也在服务端。

#### render()
在渲染时调用

#### componentDidMount()
在第一次渲染后调用，只在客户端。之后组件已经生成了对应的DOM结构，可以通过this.getDOMNode()来进行访问。 
如果你想和其他JavaScript框架一起使用，可以在这个方法中调用setTimeout, setInterval或者发送AJAX请求等操作(防止异部操作阻塞UI)。

### Updating：正在被重新渲染

#### componentWillReceiveProps()
在组件接收到一个新的prop时被调用。这个方法在初始化render时不会被调用。

#### shouldComponentUpdate()
返回一个布尔值。在组件接收到新的props或者state时被调用。在初始化时或者使用forceUpdate时不被调用。 
可以在你确认不需要更新组件时使用。
	
#### componentWillUpdate()
在组件接收到新的props或者state但还没有render时被调用。在初始化时不会被调用。

#### render()
#### componentDidUpdate()
在组件完成更新后立即调用。在初始化时不会被调用。

### Unmounting：已移出真实 DOM

#### componentWillUnmount()
在组件从 DOM 中移除的时候立刻被调用。

#### 实例：定时器，每隔100毫秒重新设置组件的透明度，并重新渲染
```
class Hello extends React.Component{
	constructor() {
		super();
		this.state = {
			opacity: 1.0
		}
	}
	componentDidMount(){
		this.timer = setInterval(function(){
			let opacity = this.state.opacity;
			opacity -= .05;
			if(opacity < .1){
				opacity = 1.0;
			}
			this.setState({
				opacity: opacity
			})

		}.bind(this),100)
	}
	render(){
		return(
			<div style={{opacity: this.state.opacity}}>
				Hello {this.props.name}
			</div>
		)
	}
}

ReactDOM.render(<Hello name="world" />,mountNode);
```


#### 实例：点击效果
以下实例初始化 state ， setNewnumber 用于更新 state。所有生命周期在 Content 组件中。
```
class Button extends React.Component{
	constructor() {
		super();
		this.state = {
			data:0
		}
	}
	setNewNumber(){
		this.setState({
			data: this.state.data + 1
		})
	}
	render(){
		return(
			<div>
				<button onClick={this.setNewNumber.bind(this)}>INCREMENT</button>
				<Content myNumber={this.state.data}></Content>
			</div>
		)
	}
}
class Content extends React.Component{
	componentWillMount(){
		console.log('Component WILL MOUNT!')
	}
	componentDidMount(){
		console.log('Component DID MOUNT!')
	}
	componentWillReceiveProps(newProps) {
		console.log('Component WILL RECEIVE PROPS!')
	}
	shouldComponentUpdate(newProps, newState) {
		return true;
	}
	componentWillUpdate(nextProps, nextState) {
		console.log('Component WILL UPDATE!');
	}
	componentDidUpdate(prevProps, prevState) {
		console.log('Component DID UPDATE!')
	}
	componentWillUnmount(){
		console.log('Component WILL UNMOUNT!')
	}
	render(){
		return(
			<div>
				<h3>{this.props.myNumber}</h3>
			</div>
		)
	}
}

ReactDOM.render(<Button />,mountNode);
```

#### 实例：统计时间
```
class Timer extends React.Component{
  constructor(props) {
    super(props);
    this.state = {secondsElapsed: 0};
  }
  tick(){
    this.setState((prevState) => ({
      secondsElapsed: prevState.secondsElapsed + 1
    }));
  }
  componentDidMount(){
    this.interval = setInterval(() => this.tick(), 1000);
  }
  componentWillUnmount(){
    clearInterval(this.interval);
  }
  render(){
    return (
      <div>Seconds Elapsed: {this.state.secondsElapsed}</div>
    );
  }
}

ReactDOM.render(<Timer />, mountNode);
```


## Lists and Keys列表遍历
JSX 允许在模板中插入数组，数组会自动展开所有成员：

```
const arr = [
    <h1>菜鸟教程</h1>,
    <h2>学的不仅是技术，更是梦想！</h2>,
];
ReactDOM.render(
    <div>{arr}</div>,mountNode
);
```
[index6](demo/index6.html )

### Array.map
```
const numbers = [1, 2, 3, 4, 5];
const doubled = numbers.map((number) => number * 2);
console.log(doubled);
```

```
const numbers = [1, 2, 3, 4, 5];
const listItems = numbers.map((number,key) =>
	<li key={key}>{number}</li>
);
console.log(listItems);
ReactDOM.render(<ul>{listItems}</ul>,mountNode);

const listItems = numbers.map(function(number,keys){
	return(
		<li key={keys}>
			{number}
		</li>
	)
});
```

```
let repos = this.state.data.items;
let repoList = [];
repos.forEach((p,keys) => {
	let item = <li key={keys}><a href={p.html_url}>{p.name}</a>({p.stargazers_count} stars)<br />{p.description}</li>;
	repoList.push(item);
})

let repos = this.state.data.items;
let repoList = repos.map(function(repo,index){
    return(
        <li key={index}>
            <a href={repo.html_url}>{repo.name}</a>({repo.stargazers_count} stars)<br />
            {repo.description}
        </li>   
    );
});
```
## Handling Events绑定事件


## Forms表单

### 基础语法
#### 实例：输入文字实时显示
```
class HelloMessage extends React.Component{
	constructor(){
		super();
		this.state = {
			value: 'Hello World!'
		};
	}
	handleChange(even){
		this.setState({
			value: even.target.value
		})
	}
	render(){
		let value = this.state.value;
		return(
			<div>
				<input type='text' value={value} onChange={this.handleChange.bind(this)} />
				<h4>{value}</h4>
			</div>
		);
	}
}
ReactDOM.render(<HelloMessage />,mountNode);
```


#### 实例：输入文字实时显示
你需要在父组件通过创建事件句柄 (handleChange) ，并作为 prop (updateStateProp) 传递到你的子组件上。
```
class Content extends React.Component{
	render(){
		return(
			<div>
				<input type='text' value={this.props.myDataProp} onChange={this.props.updataStateProp} />
				<h4>{this.props.myDataProp}</h4>
			</div>
		)
	}
}
class HelloMessage extends React.Component{
	constructor(){
		super();
		this.state = {
			value: 'Hello World!'
		};
	}
	handleChange(even){
		this.setState({
			value: even.target.value
		})
	}
	render(){
		let value = this.state.value;
		return(
			<div>
				<Content myDataProp={value} updataStateProp={this.handleChange.bind(this)} />
			</div>
		);
	}
}
ReactDOM.render(<HelloMessage />,mountNode);
```

#### 实例：点我
```
class HelloMessage extends React.Component{
	constructor(){
		super();
		this.state={
			value: 'Hello World!'
		}
	}
	handleChange(event){
		this.setState({
			value: 'luuman is good man!'
		})
	}
	render(){
		let value = this.state.value;
		return(
			<div>
				<button onClick={this.handleChange.bind(this)}>点我</button>
				<h4>{value}</h4>
			</div>
		)
	}
}
ReactDOM.render(<HelloMessage />,mountNode);
```

当你需要从子组件中更新父组件的 state 时，你需要在父组件通过创建事件句柄 (handleChange) ，并作为 prop (updateStateProp) 传递到你的子组件上。实例如下：

#### 实例：点我
```
class Content extends React.Component{
	render(){
		return(
			<div>
				<button onClick={this.props.updateStateProp}>点我</button>
				<h4>{this.props.myDataProp}</h4>
			</div>
		)
	}
}
class HelloMessage extends React.Component{
	constructor(){
		super();
		this.state = {
			value: 'Hello World!'
		}
	}
	handleChange(event){
		this.setState({
			value: 'luuman is good man!'
		})
	}
	render(){
		let value = this.state.value;
		return <div><Content myDataProp={value} updateStateProp={this.handleChange.bind(this)}></Content></div>
	}
}
ReactDOM.render(<HelloMessage />,mountNode);
```

## Refs and the DOM
### React Refs
React 支持一种非常特殊的属性 Ref ，你可以用来绑定到 render() 输出的任何组件上。
这个特殊的属性允许你引用 render() 返回的相应的支撑实例（ backing instance ）。这样就可以确保在任何时间总是拿到正确的实例。

>使用方法：
绑定一个 ref 属性到 render 的返回值上：

在其它代码中，通过 this.refs 获取支撑实例:
```
<input ref="myInput" />

var input = this.refs.myInput;
var inputValue = input.value;
var inputRect = input.getBoundingClientRect();
```

#### 实例：点我输入框获取焦点
```
class MyComponent extends React.Component{
	handleClick(){
		this.refs.myInput.focus();
	}
	render(){
		return(
			<div>
				<input type='text' ref='myInput' />
				<input type='button' value='点我输入框获取焦点' onClick={this.handleClick.bind(this)} />
			</div>
		);
	}
}
ReactDOM.render(<MyComponent />,mountNode);
```
当组件插入到 DOM 后，ref属性添加一个组件的引用于到this.refs.name获取。

实例中，我们获取了输入框的支撑实例的引用，子点击按钮后输入框获取焦点。
我们也可以使用 getDOMNode()方法获取DOM元素


## React AJAX
React 组件的数据可以通过 componentDidMount 方法中的 Ajax 来获取，当从服务端获取数据库可以将数据存储在 state 中，再用 this.setState 方法重新渲染 UI。
当使用异步加载数据时，在组件卸载前使用 componentWillUnmount 来取消未完成的请求。
### 实例
```
$.get(URL,function(data){})
```

#### 实例：获取 Github 用户最新 gist 共享描述:
```
class UserGist extends React.Component{
	constructor() {
		super();
		this.state = {
			username: '',
			lastGistUrl: ''
		}
	}
	componentDidMount(){
		this.serverRequest = $.get(this.props.source,function(result){
			let lastGist = result[0];
			this.setState({
				username: lastGist.owner.login,
				lastGistUrl: lastGist.html_url
			})
		}.bind(this))
	}
	componentWillUnmount(){
		this.serverRequest.abort();
	}
	render(){
		return(
			<div>
				{this.state.username}
				<a href={this.state.lastGistUrl}>{this.state.lastGistUrl}</a>
			</div>
		)
	}
}

ReactDOM.render(<UserGist source="https://api.github.com/users/octocat/gists" />,mountNode);
```

#### 实例：拉取数据
```
import $ from 'jquery';
import React from 'react';
import ReactDOM from 'react-dom';
const mountNode = document.getElementById('root');

class RipoList extends React.Component{
    constructor(){
        super();
        this.state = {
            loading: true,
            error: null,
            data: null
        };
    }
    componentDidMount(){
        this.props.promise.then(
            value => this.setState({
                loading: false,
                data: value
            }),
            error => this.setState({
                loading: false,
                error: error
            })
        );
    }
    render(){
        if(this.state.loading){
            return <span>Loading...</span>;
        }else if(this.state.error != null){
            return  <span>Error: {this.state.error.message}</span>;
        }else{
            let repos = this.state.data.items;
            let repoList = repos.map(function(repo,index){
                return(
                    <li key={index}>
                        <a href={repo.html_url}>{repo.name}</a>({repo.stargazers_count} stars)<br />
                        {repo.description}
                    </li>   
                );
            });
            return(
                <main>
                    <h1>Most Popular JavaScript Projects in Github</h1>
                    <ol>{repoList}</ol>
                </main>
            )
        }
    }
}
ReactDOM.render(<RipoList promise={$.getJSON('https://api.github.com/search/repositories?q=javascript&sort=stars')} />,mountNode);
```








## Add-Ons 添加插件

### jquery

```
import $ from 'jquery';
import React from 'react';
import ReactDOM from 'react-dom';

class HelloWorld extends React.Component{
	render(){
		return(
			<div>HelloWorld</div>
		);
	}
}

ReactDOM.render(<HelloWorld />,$('#example')[0]);
```

### recharts
- [React图表组件库](http://recharts.org/ "")

### bootstrap
- [React组件库](https://react-bootstrap.github.io/ "")

### MarkdownEditor
- [MarkDown](https://github.com/jonschlinkert/remarkable "Markdown解析器")


## ES6
[ECMAScript 6 入门](http://es6.ruanyifeng.com/ "阮一峰")

### let
用来声明变量。它的用法类似于var，但是所声明的变量，只在let命令所在的代码块内有效。

> for循环的计数器

```
for (let i = 0; i < 10; i++) {}

console.log(i);
//ReferenceError: i is not defined
```

> 下面的代码如果使用var，最后输出的是10

```
var a = [];
for (var i = 0; i < 10; i++) {
  a[i] = function () {
    console.log(i);
  };
}
a[6](); // 10
```
> 如果使用let，声明的变量仅在块级作用域内有效，最后输出的是6

```
var a = [];
for (let i = 0; i < 10; i++) {
  a[i] = function () {
    console.log(i);
  };
}
a[6](); // 6
```