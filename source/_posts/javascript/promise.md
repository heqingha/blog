---
title: promise
categories:
  - web skill
tags:
  - js
date: 2019-03-08 14:14:10
---

> from MDN : The Promise object is used for asynchronous computations. A Promise represents a single asynchronous operation that hasn't completed yet, but is expected in the future.

<!--more-->

我们就可以利用 then 进行「链式回调」，将异步操作以同步操作的流程表示出来,避免厄运回调金字塔，Promise 有三种状态（pending，fulfilled，rejected）有两种状态的改变&API

[Promise](https://blog.csdn.net/qq_34645412/article/details/81170576)

1. pending----->fulfilled 成功
2. pending----->rejected 失败
3. 一旦状态改变，promise.then 绑定的函数就会被调用，然后一直保持这个状态
4. Promise 新建后就会立即执行。而 then 方法中指定的回调函数，将在当前脚本所有同步任务执行完才会执行（单线程）


    ```js
        // explain of the item 5 
      var promise = new Promise(function(resolve, reject) {
        throw new Error("test");
      });
      // ==========  //
      var promise = new Promise(function(resolve, reject) {
        reject(new Error("test"));
      });
      promise.catch(function(error) {
        console.log(error);
      });
      ```
5. Promise 一旦 fulfilled 了，再抛错，也不会变为 rejected，就不会被 catch 了。
```js
   // Promise 一旦 fulfilled 了，再抛错，也不会变为 rejected，就不会被 catch 了。
   var promise = new Promise(function(resolve, reject) {
     resolve();
     throw "error";
   });
   promise.catch(function(e) {
    console.log(e); //This is never called
   });
   ```
6. Promise.prototype.then(onFulfilled, onRejected)
7. Promise.prototype.catch(onRejected),该方法是.then(undefined, onRejected)的别名
8. Promise.all(iterable),该方法用于将多个Promise实例，包装成一个新的Promise实例
   ```js
   // 接受一个数组（或具有Iterator接口）作参数,当p1, p2, p3状态都变为fulfilled，p的状态才会变为fulfilled，并将三个promise返回的结果，按参数的顺序（而不是 resolved的顺序）存入数组，传给p的回调函数
    var p = Promise.all([p1, p2, p3]);
   ```
9. Promise.race(iterable)，该方法同样是将多个Promise实例，包装成一个新的Promise实例。

  ```js
  // 当p1, p2, p3其中之一状态变为rejected，p的状态也会变为rejected，并把第一个被reject的promise的返回值，传给p的回调函数
  ```
10. .resolve()
  ```js
  Promise.resolve('Success');
  // ======== //
  new Promise(function (resolve) {
      resolve('Success');
  });
  Promise.resolve('success').then(function (value) {
    console.log(value);
  });
  // Promise.resolve()的另一个作用就是将thenable对象（即带有then方法的对象）转换为promise对象。
  var p1 = { 
    then: function(resolve) {
      throw new Error("error");
      resolve("Resolved");
    }
  };
  var p2 = Promise.resolve(p1);
  ```
11. .reject()

  ```js
  // 这个方法和上述的Promise.resolve()类似，它也是new Promise()的快捷方式。
  ```
```js
function request(url, param, successFun, errorFun) {
$.ajax({
  type: "POST",
  url: url,
  param: param,
  async: true, //默认为true,即异步请求；false为同步请求
  success: successFun,
  error: errorFun
});
}
// 对比 传统
request(
"test.html",
{},
function(data1) {
  console.log("第一次请求成功, 这是返回的数据:", data1);
  request(
    "test2.html",
    data1,
    function(data2) {
      console.log("第二次请求成功, 这是返回的数据:", data2);
    },
    function(error2) {
      console.log("第二次请求失败, 这是失败信息:", error2);
    }
  );
},
function(error1) {
  console.log("第一次请求失败, 这是失败信息:", error1);
}
);

function sendRequest(url, param) {
return new Promise(function(resolve, reject) {
  request(url, param, resolve, reject);
});
}

// 对比 promise
sendRequest("test.html", "")
.then(function(data1) {
  console.log("第一次请求成功, 这是返回的数据:", data1);
})
.then(function(data2) {
  console.log("第二次请求成功, 这是返回的数据:", data2);
})
.catch(function(error) {
  //用catch捕捉前面的错误
  console.log("sorry, 请求失败了, 这是失败信息:", error);
});
````




