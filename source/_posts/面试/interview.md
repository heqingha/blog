---
title: 前端面试
categories:
  - 前端技术
tags:
  - 前端面试
date: 2018-08-23 15:52:40
---

> 2108 前端面试题整理

<!--- more -->

- [请写出一种实现深拷贝的方法？](#1)

- [JQ 链式调用的实现方法？](#2)

- [有 arr = [{a:1,{b:2},{c:3}}] obj={d:4} 请使用 es6 写出一种方法实现,得到对象{a:1,b:2,c:3,d:4}](#3)

- [Javascript 垃圾收集方法？](#4)

- [简单写出 vue 的 MVVM 实现原理](#5)

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

```js
```

[2018 最新 Web 前端经典面试试题及答案](https://blog.csdn.net/wdlhao/article/details/79079660)
