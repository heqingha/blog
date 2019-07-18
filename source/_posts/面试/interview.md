---
title: Interview issue
categories:
  - Interview
tags:
  - Interview
date: 2018-08-23 15:52:40
---

> Interview issue

<!--- more -->

- [请写出一种实现深拷贝的方法？](#1)

- [JQ 链式调用的实现方法？](#2)

- [有 arr = [{a:1,{b:2},{c:3}}] obj={d:4} 请使用 es6 写出一种方法实现,得到对象{a:1,b:2,c:3,d:4}](#3)

- [Javascript 垃圾收集方法？](#4)

- [简单写出 vue 的 MVVM 实现原理](#5)

- [实现 es5reduce 的方法](#6)

- [使用 es5 实现 Promise](#7)

- [什么是单线程和异步有何关系](#8)

- [JavaScript 中的 Currying(柯里化) 和 Partial Application(偏函数应用)](#9)

### <span id='1'>1. 请写出一种实现深拷贝的方法？</span>

```js
var objDeepCopy = function(source) {
  var sourceCopy = source instanceof Array ? [] : {};
  for (var item in source) {
    sourceCopy[item] =
      typeof source[item] === "object"
        ? objDeepCopy(source[item])
        : source[item];
  }
  return sourceCopy;
};
```

### <span id='2'>2. JQ 链式调用的实现方法？</span>

```js
    aQuery.prototype = {
        init: function() {
            alert(1);
            return this;
        },
        name: function() {
            alert(2);
            return this
        }
    aQuery.init().name()
```

### <span id='3'>3. 有 arr = [{a:1,{b:2},{c:3}}] obj={d:4} 请使用 es6 写出一种方法实现,得到对象{a:1,b:2,c:3,d:4}</span>

```js
.........
```

### <span id='4'>4. Javascript 垃圾收集方法？</span>

```
回收机制方式
1、定义和用法：垃圾回收机制(GC:Garbage Collection),执行环境负责管理代码执行过程中使用的内存。

2、原理：垃圾收集器会定期（周期性）找出那些不在继续使用的变量，然后释放其内存。但是这个过程不是实时的，因为其开销比较大，所以垃圾回收器会按照固定的时间间隔周期性的执行。

3. 垃圾回收策略：标记清除(较为常用)和引用计数。

标记清除：

　　定义和用法：当变量进入环境时，将变量标记"进入环境"，当变量离开环境时，标记为："离开环境"。某一个时刻，垃圾回收器会过滤掉环境中的变量，以及被环境变量引用的变量，剩下的就是被视为准备回收的变量。

　　到目前为止，IE、Firefox、Opera、Chrome、Safari的js实现使用的都是标记清除的垃圾回收策略或类似的策略，只不过垃圾收集的时间间隔互不相同。

引用计数：

　　定义和用法：引用计数是跟踪记录每个值被引用的次数。

　　基本原理：就是变量的引用次数，被引用一次则加1，当这个引用计数为0时，被视为准备回收的对象。
```

### <span id='5'>5. 简单写出 vue 的 MVVM 实现原理</span>

[vue 双向绑定原理](https://juejin.im/entry/5923973da22b9d005893805a)

### <span id='6'>6. 实现 es5reduce 的方法</span>

```js
// reduce() 从数组第一项开始，逐个遍历到最后
// reduceRight() 从数组的最后一项开始，向前遍历到第一项
// 都支持4个参数。(prev【前一个值】,cur【当前值】,index【项的索引】,array【数组对象】)
// reduce()和reduceRight()的差别在于从哪头开始遍历数组。除此之外都一样。
// 【兼容性】IE9+,Chrome,Firefox 3+,Safari 4+,Opera 10.5支持
// 这个函数返回的任何值都会作为第一个参数自动传给下一项。第一次迭代从数组的第二项开始。
//利用reduce求数组中所有值的和
var values = [1, 2, 3, 4, 5];
var sum = values.reduce(function(prev, cur, index, array) {
  return prev + cur;
});
//sum的值为15;
```

### <span id='7'>7. 使用 es5 实现 Promise</span>

[实现一个自己的 promise](https://blog.csdn.net/yibingxiong1/article/details/68075416)

### <span id='8'>8. 什么是单线程,和异步有何关系</span>

[异步和单线程](https://blog.csdn.net/ll_0801xyz/article/details/78232621)
[JavaScript 运行机制详解：再谈 Event Loop](http://www.ruanyifeng.com/blog/2014/10/event-loop.html)

### <span id='9'>9. JavaScript 中的 Currying(柯里化) 和 Partial Application(偏函数应用)</span>

[css88](http://www.css88.com/archives/7781)


