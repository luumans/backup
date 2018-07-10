模块化编程

# IIFE(立即执行的函数表达式)
Imdiately Invoked Function Expression 立即执行的函数表达式

## 普通函数
``` javascript
function(window, undefined){
  // 代码...
}
```

## 转换为表达式
``` javascript
(function(window, undefined){
  // 立即执的行代码...
})(window);
```

``` javascript
~function(){
  console.log(1);
}();
+function(){
  console.log(2);
}();
(function(){
  console.log(3);
}())
```

## 写法解析
### 普通写法
``` javascript
var wall = {}; // 声明定义一个命名空间wall

// 定义方法
(function(window, WALL, undefined){
  // 给wall命名空间绑定方法sub
  WALL.sub = function(){
    console.log('hello sub');
  };
})(window, wall);

(function(window, WALL, undefined){
  // 给wall命名空间绑定方法 suan
  WALL.suan = function(){
    console.log('wall suan');
  };
})(window, wall);

// 调用
wall.sub();
wall.suan();
```
定义命名空间 =》 命名空间加东西
问题：不足的地方就是必须先声明一个命名空间，然后才能执行相关的绑定代码。存在顺序加载的问题。

### 放大模式
``` javascript
var wall = (function(window, WALL, undefined){
  if(typeof WALL == 'undefined'){
    WALL = {};
  }

  // 给wall命名空间绑定方法sub
  WALL.sub = function(){
    console.log('hello sub');
  }

  return WALL; // 返回引用
})(window, wall);

var wall = (function(window, WALL, undefined){
  if(typeof WALL == 'undefined'){
    WALL = {};
  }

  // 给wall命名空间绑定方法 suan
  WALL.suan = function(){
    console.log('wall suan');
  }

  return WALL; // 返回引用
})(window, wall);

// 调用
wall.sub();
wall.suan();
```

### 宽放大模式
``` javascript
(function(window, WALL, undefined){
  // 给wall命名空间绑定方法sub
  WALL.sub = function(){
    console.log('hello sub');
  }
})(window, window.wall || (window.wall = {}));

(function(window, WALL, undefined){
  // 给wall命名空间绑定方法 suan
  WALL.suan = function(){
    console.log('wall suan');
  }
})(window, window.wall || (window.wall = {}));

// 调用
wall.sub();
wall.suan();
```

### 分文件加载IIFE要注意的点
``` javascript
;(function(window, WALL, undefined){
  // 给wall命名空间绑定方法sub
  WALL.sub = function(){
    console.log('hello sub');
  }
})(window, window.wall || (window.wall = {}));
```

合并文件：
``` javascript
// modelA.js 文件
wall.log()

// modelB.js 文件
(function(window, WALL, undefined){
  // 给wall命名空间绑定方法sub
  WALL.sub = function(){
    console.log('hello sub');
  }
})(window, window.wall || (window.wall = {}));
```

最后导致执行为wall.log()(.....) Uncaught TypeError: (intermediate value)(...) is not a function



### 宽放大模式
``` javascript
```
