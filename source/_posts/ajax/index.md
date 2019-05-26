---
title: ajax & xhr
categories:
  - web skill
tags:
  - js
date: 2019-05-26 11:03:20
---

> 记忆 ajax （Asynchronous JavaScript and XML） 概念、原理 封装实用 ajax Fn

<!--more-->

1. 什么是 ？

> AJAX 不是新的编程语言，而是一种使用现有标准的新方法, 创建交互网页应用的一门技术

2. 应用场景 ?

> 实时更新，表单验证等等

3. 优缺点 ?

> 实现局部更新（无刷新状态下），减轻了服务器端的压力

> 破坏了浏览器前进和后退机制, 一个 Ajax 请求多了，也会出现页面加载慢的情况, 搜索引擎的支持程度比较低, ajax 的安全性问题不太好（可以用数据加密解决）

4. 如何实现 ？

> AJAX 的基础 XMLHttpRequest。所有现代浏览器均支持 XMLHttpRequest 对象（IE5 和 IE6 使用 ActiveXObject）,XMLHttpRequest 用于在后台与服务器交换数据

5. 如何使用（四步） ？

   1.创建 ajax、 2.连接服务器、 3.发送请求、 4.接受返回值

---创建 ajax

```js
var xhr = null;
if (window.XMLHttpRequest) {
  xhr = new XMLHttpRequest();
} else {
  xhr = new ActiveXObject("Mricosoft.XMLHTTP");
}
```

---连接服务器、

xhr.open（"请求方式（GET/POST）"，url 路径，“异步/同步”）
xhr.open（"请求方式（GET/POST）"，url 路径 + “？”请求数据 + ，“异步/同步”）

---发送请求、

```js
// 当请求方式为GET的时候，发送请求为xhr.send(null)
//当请求方式为POST的时候，发送请求为xhr.send(请求数据)
//此外使用POST的时候必须在xhr.send（请求数据）上面添加
xhr.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");

// 例子

xmlhttp.open("POST", "/try/ajax/demo_post2.php", true);
xmlhttp.setRequestHeader("Content-type", "application/x-www-form-urlencoded");
xmlhttp.send("fname=Henry&lname=Ford");
```

---接受返回值

如需获得来自服务器的响应，请使用 XMLHttpRequest 对象的 responseText 或 responseXML 属性
当请求被发送到服务器时，我们需要执行一些基于响应的任务
每当 readyState(XMLHttpRequest 的状态) 改变时，就会触发 onreadystatechange 事件

![readyState](/img/ajax/xhr.png)

```js
xhr.onreadystatechange = function() {
  if (xmlhttp.readyState == 4 && xmlhttp.status == 200) {
    document.getElementById("myDiv").innerHTML = xmlhttp.responseText;
  }
};
```

6. 封装使用

```js
function ajax(options) {
  var xhr = null;
  var params = formsParams(options.data);
  function formsParams(data) {
    var arr = [];
    for (var prop in data) {
      arr.push(prop + "=" + data[prop]);
    }
    return arr.join("&");
  }
  //创建对象
  if (window.XMLHttpRequest) {
    xhr = new XMLHttpRequest();
  } else {
    xhr = new ActiveXObject("Microsoft.XMLHTTP");
  }
  // 连接
  if (options.type == "GET") {
    xhr.open(options.type, options.url + "?" + params, options.async);
    xhr.send(null);
  } else if (options.type == "POST") {
    xhr.open(options.type, options.url, options.async);
    xhr.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");
    xhr.send(params);
  }
  xhr.onreadystatechange = function() {
    if (xhr.readyState == 4 && xhr.status == 200) {
      options.success(xhr.responseText);
    }
  };
  
}

ajax({
  url: "a.php", // url---->地址
  type: "POST", // type ---> 请求方式
  async: true, // async----> 同步：false，异步：true
  data: {
    //传入信息
    name: "张三",
    age: 18
  },
  success: function(data) {
    //返回接受信息
    console.log(data);
  }
});
```
