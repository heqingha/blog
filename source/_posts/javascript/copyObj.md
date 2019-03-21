---
title: shallow copy and deep copy of the objects
categories:
  - web skill
tags:
  - web skill
date: 2019-03-20 17:44:40
---

> Summarizes deep and shallow copy of objects， and give some code examples
<!--more-->
### 带着问题：首先为什么会使用到深拷贝，浅拷贝，什么情况下使用深，什么情况下使用浅？？？

1. 当我们需要 copy 一个基本类型的变量时，可以直接 a = b, 但是当我们需要拷贝对象时，直接复制一个对象时会有问题，由于引用类型存储的是一个指针名称，实际的值是这个指针所指向的地址，当我们 copy 时，会指向同一个地址，这时一个两者改变其中一个都会影响另一个，因此引入浅拷贝的概念

   浅拷贝有多种方式,这边介绍二种,es6 语法

   1）  通过 Object.assign
   2）  通过展开运算符（…）来解决

```js
let a = {
  age: 1
};
let b = a;
a.age = 2;
console.log(b.age); // 2

let a = {
  age: 1
};
let b = Object.assign({}, a);
a.age = 2;
console.log(b.age); // 1

let a = {
  age: 1
};
let b = { ...a };
a.age = 2;
console.log(b.age); // 1
```

2. 那什么时候需要深拷贝呢？？？ 对象里面又是对象的（通俗讲），解决办法多种

    1. 通过JSON.parse(JSON.stringify(object))，但是该方法也是有局限性的：

        会忽略 undefined
        会忽略 symbol
        不能序列化函数
        不能解决循环引用的对象
        代码如下
    2. 一个函数解决

```js
//解决方法一
let a = {
    age: 1,
    jobs: {
        first: 'FE'
    }
}
let b = JSON.parse(JSON.stringify(a))
a.jobs.first = 'native'
console.log(b.jobs.first) // FE
// 方法一出现的问题
let obj = {
  a: 1,
  b: {
    c: 2,
    d: 3,
  },
}
obj.c = obj.b
obj.e = obj.a
obj.b.c = obj.c
obj.b.d = obj.b
obj.b.e = obj.b.c
let newObj = JSON.parse(JSON.stringify(obj))
console.log(newObj)

let a = {
    age: undefined,
    sex: Symbol('male'),
    jobs: function() {},
    name: 'yck'
}
let b = JSON.parse(JSON.stringify(a))
console.log(b) // {name: "yck"}

//解决方法二，弥补方法一的不足,ps直接复制的 ，自己也看不到
function structuralClone(obj) {
  return new Promise(resolve => {
    const {port1, port2} = new MessageChannel();
    port2.onmessage = ev => resolve(ev.data);
    port1.postMessage(obj);
  });
}

var obj = {a: 1, b: {
    c: b
}}
// 注意该方法是异步的
// 可以处理 undefined 和循环引用对象
(async () => {
  const clone = await structuralClone(obj)
})()

// 这边附上自己的深拷贝函数

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

文章来源 ： https://yuchengkai.cn/docs/frontend/#%E6%B7%B1%E6%8B%B7%E8%B4%9D