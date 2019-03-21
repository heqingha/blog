---
title: module
categories:
  - web skill
tags:
  - js
date: 2019-03-21 11:06:29
---

> AMD,CMD 和 CommonJs

<!--more-->

### commonJS（JS 在后端的表现制定的）

CommonJS 定义的模块分为:{模块引用(require)} {模块定义(exports)} {模块标识(module)}

CommonJS 中有一个全局性方法 require()，用于加载模块

require()用来引入外部模块；exports 对象用于导出当前模块的方法或变量，唯一的导出口；module 对象就代表模块本身。

NodeJS 是 CommonJS 规范的实现，运行在服务端，不支持浏览器环境，因此大家希望有个可以运行在两者上面的, 但由于 require 是同步的，如果加载的时间过长，会阻塞，在服务器端不是什么问题，因为所有的模块都存放在本地硬盘，可以同步加载完成，等待时间就是硬盘的读取时间，但是在浏览器算是个大问题，因为模块都放在服务器端，等待时间取决于网速的快慢，可能要等很长时间，浏览器处于"假死"状态。因此 AMD(异步模块定义)出现了，它就主要为前端 JS 的表现制定规范。

```js
//
```

### AMD

采用异步方式加载模块,模块的加载不影响它后面语句的运行。所有依赖这个模块的语句，都定义在一个回调函数中，等到加载完成之后，这个回调函数才会运行。

AMD 也采用 require()语句加载模块

```js
//AMD比较适合浏览器环境
require(["math"], function(math) {
  math.add(2, 3);
});
// require.js和curl.js。 实现了AMD规范

<script src="js/require.js" defer async="true" ></script>
<script src="js/require.js" data-main="js/main"></script>

// data-main属性的作用是，指定网页程序的主模块。在上例中，就是js目录下面的main.js，这个文件会第一个被require.js加载。由于require.js默认的文件后缀名是js，所以可以把main.js简写成main。

```

优势

（1）实现 js 文件的异步加载，避免网页失去响应；

（2）管理模块之间的依赖性，便于代码的编写和维护。

```js
// main.js
// 当主模块依赖于其他模块时
require(["moduleA", "moduleB", "moduleC"], function(moduleA, moduleB, moduleC) {
  // some code here
});
// 我们可以对模块的加载行为进行自定义
require.config({
　　　　baseUrl: "js/lib",
　　　　paths: {
　　　　　　"jquery": "jquery.min",
　　　　　　"underscore": "underscore.min",
　　　　　　"backbone": "backbone.min",
          "jquery": "https://ajax.googleapis.com/ajax/libs/jquery/1.7.2/jquery.min"
　　　　}
　　});
```
require.js加载的模块，采用AMD规范，模块必须采用特定的define()函数来定义

```js
// 无依赖定义模块
define(function (){
　　　　var add = function (x,y){
　　　　　　return x+y;
　　　　};
　　　　return {
　　　　　　add: add
　　　　};
　　});
// 有依赖定义模块
define(['myLib'], function(myLib){
　　　　function foo(){
　　　　　　myLib.doSomething();
　　　　}
　　　　return {
　　　　　　foo : foo
　　　　};
　　});
// 加载

require(['math'], function (math){
　　　　alert(math.add(1,1));
　　});
```
### CMD(sea.js)

与AMD蛮相近的(copy from https://github.com/seajs/seajs/issues/277)

```js
define(function(require,exports,module){...});
```

相同

 RequireJS 和 Sea.js 都是模块加载器，倡导模块化开发理念，核心价值是让 JavaScript 的模块化开发变得简单自然。

不同

1. 定位有差异。RequireJS 想成为浏览器端的模块加载器，同时也想成为 Rhino / Node 等环境的模块加载器。Sea.js 则专注于 Web 浏览器端，同时通过 Node 扩展的方式可以很方便跑在 Node 环境中。

2. 遵循的规范不同。RequireJS 遵循 AMD（异步模块定义）规范，Sea.js 遵循 CMD （通用模块定义）规范。规范的不同，导致了两者 API 不同。Sea.js 更贴近 CommonJS Modules/1.1 和 Node Modules 规范。

3. 推广理念有差异。RequireJS 在尝试让第三方类库修改自身来支持 RequireJS，目前只有少数社区采纳。Sea.js 不强推，采用自主封装的方式来“海纳百川”，目前已有较成熟的封装策略。

4. 对开发调试的支持有差异。Sea.js 非常关注代码的开发调试，有 nocache、debug 等用于调试的插件。RequireJS 无这方面的明显支持。

5. 插件机制不同。RequireJS 采取的是在源码中预留接口的形式，插件类型比较单一。Sea.js 采取的是通用事件机制，插件类型更丰富。

from： https://www.cnblogs.com/chenguangliang/p/5856701.html