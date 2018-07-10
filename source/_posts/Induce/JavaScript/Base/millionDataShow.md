title: 上万的数据如何在前端页面快速展示？
date: 2018-03-15 18:29:00
description: 
categories:
- JavaScript
tags:
- JavaScript
toc: true
author:
comments:
original:
permalink: 
---
前端要呈现百万数据，这个需求是很少见的，但是展示千条稍微复杂点的数据，这种需求还是比较常见，只要内存够，javascript 肯定是吃得消的，计算几千上万条数据，js 效率根本不在话下，但是 DOM 的渲染浏览器扛不住，CPU 稍微搓点的电脑必然会卡爆。
首先，这种情况很少见。如果要在前端呈现大量的数据，一般的策略就是分页。虽然这么说，但是如果展示上百条数据，图片资源消耗，也会让页面发生卡顿。
<!-- more -->

# JavaScript效率

# DOM渲染卡顿
DOM渲染一直困扰前端的难题，由于设备参差不齐，CPU性能严重影响了页面的渲染能力。

## 方案

``` javascript
                          /==============>  box
        |     ....     | /
        +--------------+/
+=======|=====List=====|========+
|       +--------------+        |
|       |     List     |        |
|       +--------------+        |\
|       |     List     |        | \
|       +--------------+        |  \======> Container
+=======|=====List=====|========+    
        +--------------+
        |     ....     |        Created By Barret Lee
```

- [Demo](https://luuman.github.io/Works/Seat/SeatAll.html "")

<!-- - [virtual-list](https://github.com/sergi/virtual-list "") -->

``` javascript
function VirtualList(config) {
  var vWidth = config.width ? (config && config.width + 'px') : '100%'
  var vHeight = config.height ? (config && config.height + 'px') : '100%'
  var itemHeight = this.itemHeight = config.itemH

  // this.items = config.items
  this.generatorFn = config.generatorFn
  this.totalRows = config.totalRows || (config.items && config.items.length)
  //List Num
  var screenItemsLen = Math.ceil(config.height / itemHeight)
  //cache List Num
  this.cachedItemsLen = screenItemsLen * 3
  console.log(config.height)

  //Set Scroller init
  var scroller = VirtualList.createScroller(itemHeight * this.totalRows)
  this.container = VirtualList.createContainer(vWidth, vHeight)
  this.container.appendChild(scroller)
  this.render(this.container, 0)

  var self = this
  var lastRepaintY
  var maxBuffer = screenItemsLen * itemHeight
  var lastScrolled = 0

  this.rmNodeInerval = setInterval(function() {
    if (Date.now() - lastScrolled > 100) {
      var badNodes = document.querySelectorAll('[data-virtual="1"]')
      for (var i = 0, l = badNodes.length; i < l; i++) {
        self.container.removeChild(badNodes[i])
      }
    }
  }, 300)
  function onScroll(e) {
    var scrollTop = e.target.scrollTop
    if (!lastRepaintY || Math.abs(scrollTop - lastRepaintY) > maxBuffer) {
      var first = parseInt(scrollTop / itemHeight) - screenItemsLen
      self.render(self.container, first < 0 ? 0 : first)
      lastRepaintY = scrollTop
    }
    lastScrolled = Date.now()
    e.preventDefault && e.preventDefault()
  }
  this.container.addEventListener('scroll', onScroll)
}
VirtualList.prototype.render = function(node, num) {
  var allItem = num + this.cachedItemsLen
  if (allItem > this.totalRows) {
    allItem = this.totalRows
  }
  var fragment = document.createDocumentFragment()
  // for add Item
  console.log(allItem)
  for (var i = num; i < allItem; i++) {
    fragment.appendChild(this.createRow(i))
  }
  console.log(fragment)

  for (var j = 1, l = node.childNodes.length; j < l; j++) {
    node.childNodes[j].style.display = 'none'
    node.childNodes[j].setAttribute('data-virtual', '1')
  }
  node.appendChild(fragment)
}
VirtualList.prototype.createRow = function(i) {
  var item;
  if (this.generatorFn) {
    item = this.generatorFn(i)
  } else if (this.items) {
  }
  item.classList.add('item')
  item.style.position = 'absolute'
  item.style.top = (i * this.itemHeight + 'px')
  console.log(item)
  return item
}
VirtualList.createContainer = function(width, height) {
  var box = document.createElement('div')
  box.style.width = width
  box.style.height = height
  box.style.overflow = 'auto'
  box.style.position = 'relative'
  box.style.padding = 0
  box.style.border = '1px solid black'
  return box
}
VirtualList.createScroller = function(boxH) {
  var scroller = document.createElement('div')
  scroller.style.opacity = 0
  scroller.style.position = 'absolute'
  scroller.style.top = 0
  scroller.style.left = 0
  scroller.style.width = '1px'
  scroller.style.height = boxH + 'px'
  return scroller
}
function dataInit (rows, cloumns) {
  var data = []
  for (var i = 0; i < cloumns; i++) {
    var list = []
    for (var j = 0; j < rows; j++) {
      list.push({
        content: 'item-' + (rows * i + j + 1),
        id: rows * i + j + 1,
        cloumns: i + 1,
        rows: j + 1,
        isNo: [true, false][Math.floor(Math.random()*[true, false].length)]
      })
    }
    data.push({
      id: i + 1,
      list: list
    })
  }
  return data
}
```

### 初始化

``` javascript
var List = new VirtualList({
  width: 600, // box width
  height: 300, // box height
  itemH: 30, // item height
  totalRows: data.length, // data
  generatorFn: function(key) { // 生成器
    var el = document.createElement('div')
    return el
  }
})
```

### 计算

> 容器何以显示的List个数

``` javascript
var screenItemsLen = Math.ceil(config.height / itemHeight)
```
> 缓存的List Div的数码

``` javascript
this.cachedItemsLen = screenItemsLen * 3
```
> 容器的整体高度

``` javascript
itemHeight * this.totalRows
```

render() 计算需要展示的内容，即将替换的内容
createRow() 创建List
createContainer() 创建容器
createScroller() 创建将容器撑起div



