title: React Demo
date: 2017-02-15 18:29:00
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
fjdfj
<!-- more -->

#### TimeList
```
//app.js
import React from 'react';
import ReactDOM from 'react-dom';
const mountNode = document.getElementById('example');

class Timer extends React.Component {
    constructor(props) {
        super(props);
        this.state = {secondsElapsed: 0};
    }

    tick() {
        this.setState((prevState) => ({
            secondsElapsed: prevState.secondsElapsed + 1
        }));
    }

    componentDidMount() {
        this.interval = setInterval(() => this.tick(), 1000);
    }

    componentWillUnmount() {
        clearInterval(this.interval);
    }

    render() {
        return (
            <div>Seconds Elapsed: {this.state.secondsElapsed}</div>
        );
    }
}

ReactDOM.render(<Timer />, mountNode);
```

#### TODO
```
//app.js
import React from 'react';
import ReactDOM from 'react-dom';

class TodoApp extends React.Component {
    constructor(props) {
        super(props);
        this.handleChange = this.handleChange.bind(this);
        this.handleSubmit = this.handleSubmit.bind(this);
        this.state = {items: [], text: ''};
    }

    render() {
        return (
            <div>
                <h3>TODO</h3>
                <TodoList items={this.state.items} />
                <form onSubmit={this.handleSubmit}>
                    <input onChange={this.handleChange} value={this.state.text} />
                    <button>{'Add #' + (this.state.items.length + 1)}</button>
                </form>
            </div>
        );
    }

    handleChange(e) {
        this.setState({text: e.target.value});
    }

    handleSubmit(e) {
        e.preventDefault();
        var newItem = {
            text: this.state.text,
            id: Date.now()
        };
        this.setState((prevState) => ({
            items: prevState.items.concat(newItem),
            text: ''
        }));
    }
}

class TodoList extends React.Component {
    render() {
        return (
            <ul>
                {this.props.items.map(item => (
                    <li key={item.id}>{item.text}</li>
                ))}
            </ul>
        );
    }
}

ReactDOM.render(<TodoApp />, document.getElementById('example'));
```

#### MarkdownEditor
[remarkable](https://github.com/jonschlinkert/remarkable "Markdown解析器")
```
//app.js
import React from 'react';
import ReactDOM from 'react-dom';
const Remarkable = require('remarkable');
const mountNode = document.getElementById('example');

class MarkdownEditor extends React.Component {
    constructor(props) {
        super(props);
        this.handleChange = this.handleChange.bind(this);
        this.state = {value: 'Type some *markdown* here!'};
    }
    handleChange() {
        this.setState({value: this.refs.textarea.value});
    }
    getRawMarkup() {
        var md = new Remarkable();
        return { __html: md.render(this.state.value) };
    }
    render() {
        return (
            <div className="MarkdownEditor">
                <h3>Input</h3>
                <textarea
                    onChange={this.handleChange}
                    ref="textarea"
                    defaultValue={this.state.value} />
                <h3>Output</h3>
                <div
                    className="content"
                    dangerouslySetInnerHTML={this.getRawMarkup()}
                />
            </div>
        );
    }
}

ReactDOM.render(<MarkdownEditor />, mountNode);
```

#### ReCharts
[Recharts](http://recharts.org/#/en-US)
```
//app.js
import React from 'react';
import ReactDOM from 'react-dom';
import {LineChart, Line, XAxis, YAxis, CartesianGrid} from 'recharts';
const mountNode = document.getElementById('example');
const data = [
    {name: 'Page A', uv: 4000, pv: 2400, amt: 2400},
    {name: 'Page B', uv: 3000, pv: 1398, amt: 2210},
    {name: 'Page C', uv: 2000, pv: 9800, amt: 2290},
    {name: 'Page D', uv: 2780, pv: 3908, amt: 2000},
    {name: 'Page E', uv: 1890, pv: 4800, amt: 2181},
    {name: 'Page F', uv: 2390, pv: 3800, amt: 2500},
    {name: 'Page G', uv: 3490, pv: 4300, amt: 2100},
];

const TinyLineChart = React.createClass({
    render () {
        return (
    <LineChart width={1000} height={400} data={data}>
        <XAxis dataKey="name"/>
        <YAxis/>
        <CartesianGrid stroke="#eee" strokeDasharray="5 5"/>
        <Line type="monotone" dataKey="uv" stroke="#8884d8" />
        <Line type="monotone" dataKey="pv" stroke="#82ca9d" />
    </LineChart>
        );
    }
})

ReactDOM.render(<TinyLineChart />,mountNode);
```







#### 实例：数组列表
```
const names = ['Alice', 'Emily', 'Kate'];

ReactDOM.render(
    <div>{names.map(
        function (name) {
            console.log(name);
            return <div>Hello, {name}!</div>
        }
    )}</div>,
    mountNode
);
```

#### 实例：文字列表
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



#### 实例：鼠标划过"My Website"，下拉即会展现
```
.navigation__dropdown{
    display: none;
}
.navigation__dropdown--open{
    display: block;
}

const navigationConfig = [
    {
        href: 'http://ryanclark.me',
        text: 'My Website',
        children: [
            {
                href: 'http://ryanclark.me/how-angularjs-implements-dirty-checking/',
                text: 'Angular Dirty Checking'
            },
            {
                href: 'http://ryanclark.me/getting-started-with-react/',
                text: 'React'
            }
        ]
    }
];
class Navigation extends React.Component{
    constructor(){
        super();
        this.state = {
            openDropdown: -1
        }
        console.log('constructor');
    }
    openDropdown(id){
        this.setState({
            openDropdown: id
        });
        console.log('openDropdown');
        console.log(id);
    }
    closeDropdown(){
        this.setState({
            openDropdown: -1
        })
        console.log('closeDropdown');
    }
    render(){
        console.log(this.state);
        let config = this.props.config;
        let items = config.map((item,keys) => {
            let children, dropdown;
            if(item.children){
                children = item.children.map((child,keyss) => {
                    return(
                        <li key={keyss} className='navigation_dropdown_item'>
                            <a className='navigation_dropdown_line' href={child.href} >{child.text}</a>
                        </li>
                    )
                });
                let dropdownClass = 'navigation_dropdown';
                if(this.state.openDropdown === keys){
                    dropdownClass += ' navigation_dropdown--open';
                }
                console.log(this.state.openDropdown,keys);
                dropdown = <ul className={dropdownClass}>{children}</ul>
            }
            return(
                <li key={keys} className='navigation_item' onMouseOut={this.closeDropdown.bind(this)} onMouseOver={this.openDropdown.bind(this,keys)}>
                    <a className='navigation_link' href={item.href}>
                        {item.text}
                    </a>
                    {dropdown}
                </li>
            )
        },this);
        return <div className='navigation'>{items}</div>
    }
}
Navigation.DefaultProps = {
    config: []
}
Navigation.propTypes = {
    config: React.PropTypes.array
}

ReactDOM.render(<Navigation config={navigationConfig} />,mountNode);
```


#### 实例：点击显示内容
```
class Bend extends React.Component{
    constructor(){
        super();
        this.state = {
            isShowContent: false
        }
    }
    handleClick(event){
        this.setState({
            isShowContent: !this.state.isShowContent
        })
    }
    dateToString(date){
        date = date || (new Date());
        return 'Date: ' + [date.getFullYear(),date.getMonth()+1,date.getDate()].join('-')
    }
    render(){
        if(this.state.isShowContent){
            let options = <div onClick={this.handleClick.bind(this)}>state modified</div>
            return options
        }
        return(
            <div onClick={this.handleClick.bind(this)}>
                <h1 className='title'>React,hello.</h1>
                <div>{this.props.children}</div>
                <p>{this.dateToString(new Date())}</p>
            </div>
        )
    }
}

ReactDOM.render(React.createElement(Bend,null,'This is a people!'),mountNode);
```




#### 实例：
```
```