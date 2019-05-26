---
title: Interview issue
categories:
  - Interview
tags:
  - Interview
date: 2018-04-12 10:03:20
---

> there are some Interview issue in article

<!--- more -->

* [=＝和===的区别？＝＝怎么进行类型转换的，说说有哪几种情况？](#0)

* [布局：一个 div(200px*200px)在左侧，另一个 div 自适应在右侧](#1)

* [给 Object 扩展一个方法 clone，实现深度克隆对象](#2)

* [px、em、rem](#3)

* [用 html,css 实现一个 div 居中在窗口](#4)

* [用css实现，两行文本，间距10px,字体是14px,距顶端和底端15px,拒左边10px](#5)

* [扩展Date的format方法](#6)

* [写出下面结果](#7)

* [alert(1&&2),alert(1||0)](#8)

* [mouseenter和mouseover的区别](#9)

* [js字符串两边截取空白的trim的原型方法的实现](#10)

* [三道判断输出的题都是经典的题](#11)

* [position不同值和区别](#12)

* [讲述你对reflow和repaint的理解](#13)

* [讲述你对reflow和repaint的理解](#14)



### <span id='0'>1. =＝和===的区别？＝＝怎么进行类型转换的，说说有哪几种情况？</span>

    ==匹配两个变量的的值，如果类型不匹配，会强制类型转换，
    ===不但匹配两个变量的值，还会匹配两个变量的数据类型是否相同，如果其中有一项不相同，匹配失败。
    ===不会类型转换，执行效率高。

### <span id='1'>2. 布局：一个 div(200px*200px)在左侧，另一个 div 自适应在右侧</span>

1.  如果不考虑浏览器的兼容问题的话，可以使用 css3 的新增属性 calc();calc 是 calculate 的简写，汉语为计算的意思。
```css
  .left {
    width: 200px;
    float: left;
    height: 200px;
    background: pink;
  }
  .right {
    float: left;
    width: calc(100% - 200px);
    height: 200px;
    background: yellow;
  }
```
2.  可以使用百分比进行布局，该方法不需考虑浏览器的兼容性.但与第一种进行比较， 右侧 div 的宽度百分比需要自己去进行计算。
```css
  // 要依赖父元素
  .left {
    width: 200px;
    float: left;
    height: 200px;
    background: pink;
  }
  .right {
    float: left;
    width: 85%;
    height: 200px;
    background: yellow;
  }
```

3.  使用绝对元素进行定位


```css
.left {
  width: 200px;
  height: 200px;
  position: absolute;
  background: pink;
}
.right {
  left: 200px;
  right: 0;
  height: 200px;
  position: absolute;
  background: yellow;
}
```

4.  div 的宽度在不进行设置的情况下会自动填满父标签的宽度。所以我们可以并不对右侧 div 的宽度进行设置


```css
.left {
  width: 200px;
  height: 300px;
  float: left;
  background: pink;
}
.right {
  height: 300px;
  margin-left: 200px;
  background: yellow;
}
```

5.  flex 布局


```css
// 要依赖父元素
.container {
  width: 1000px;
  height: 400px;
  border: 1px solid red;
  display: flex; /*flex布局*/
}
.left {
  width: 200px;
  height: 100%;
  background: gray;
  flex: none;
}
.right {
  height: 100%;
  background: green;
  flex: 1; /*flex布局*/
}
```

6.  table 布局
    // 要依赖父元素


```css
.container {
  width: 1000px;
  height: 400px;
  border: 1px solid red;
  display: table; /*table布局*/
}
.left {
  width: 200px;
  height: 100%;
  background: gray;
  display: table-cell;
}
.right {
  height: 100%;
  background: green;
  display: table-cell;
}
```
7.  BFC(块级格式化上下文)
```css
.container {
  width: 1000px;
  height: 400px;
  border: 1px solid red;
}
.left {
  width: 200px;
  height: 100%;
  background: gray;
  float: left;
}
.rigth {
  overflow: hidden; /* 触发bfc */
  background: green;
}
```

### <span id='2'>3. 给 Object 扩展一个方法 clone，实现深度克隆对象</span>

[javaScript 中浅拷贝和深拷贝的实现](javaScript中浅拷贝和深拷贝的实现)

### <span id='3'>4. px、em、rem</span>

> 有何区别

* px 在缩放页面时无法调整那些使用它作为单位的字体、按钮等的大小；
* em 的值并不是固定的，会继承父级元素的字体大小，代表倍数；
* rem 的值并不是固定的，始终是基于根元素 html 的，也代表倍数。

> em

em 的使用是相对于其父级的字体大小的，即倍数。浏览器的默认字体高都是 16px，未经调整的浏览器显示 1em = 16px。但是有一个问题，如果设置 1.2em 则变成 19.2px，问题是 px 表示大小时数值会忽略掉小数位的（你想像不出来半个像素吧）。而且 1em = 16px 的关系不好转换，因此，常常人为地使 1em = 10px。这里要借助字体的 % 来作为桥梁。

因为默认时字体 16px = 100%，则有 10px = 62.5%。所以首先在 body 中全局声明 font-size=62.5%=10px，也就是定义了网页 body 默认字体大小为 10px，由于 em 有继承父级元素字体大小的特性，如果某元素的父级没有设定字体大小，那么它就继续了 body 默认字体大小 1em = 10px。

但是由于 em 是相对于其父级字体的倍数的，当出现有多重嵌套内容时，使用 em 分别给它们设置字体的大小往往要重新计算。比如说你在父级中声明了字体大小为 1.2em，那么在声明子元素的字体大小时设置 1em 才能和父级元素内容字体大小一致，而不是 1.2em（避免 1.2\*1.2=1.44em）, 因为此 em 非彼 em。再举个例子：

`<span>Outer <span>inner</span> outer</span>`

```css
body {
  font-size: 62.5%;
}
span {
  font-size: 1.6em;
}
```

> rem

rem 的出现再也不用担心还要根据父级元素的 font-size 计算 em 值了，因为它始终是基于根元素（<html>）的。比如默认的 html font-size=16px，那么想设置 12px 的文字就是：12÷16=0.75(rem)
仍然是上面的例子，CSS 改为：

```css
html {
  font-size: 62.5%;
}
span {
  font-size: 16px;
  font-size: 1.6rem;
}
```

结果：内外 <span> 的内容均为 16px。

> 需要注意的是，为了兼容不支持 rem 的浏览器，我们需要在各个使用了 rem 地方前面写上对应的 px 值，这样不支持的浏览器可以优雅降级。

### <span id='4'>5. 用 html,css 实现一个 div 居中在窗口</span>

  1. Flex 布局, 不考虑兼容老式浏览器的话，用Flex布局简单直观一劳永逸

  ```css
      .out{
          width: 200px;
          height: 200px;
          background: pink;
          display: flex;
          display: -webkit-flex; /* Safari */
          /*align-items:center;    /* 处理垂直居中 */
      }
      .in {
          width: 100px;
          height: 100px;
          background: blue;
          margin: auto;           /* 水平垂直居中 */
          word-wrap: break-word;  /*如果是字母数字， 处理换行 */
      }
  ```

  2. 不知道自己高度的情况下, 利用绝对定位，结合transform: translate（）属性。

    ```css
        .out{
        width: 200px; 
        height: 200px;
        background: pink;
        position: relative;
        }
        .in {
        background: blue;
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
        }
    ```
  3. 已知容器高度的情况下，利用定位

    ```css
        .out {
            width: 200px; 
            height: 200px;
            background: pink;
            position: relative;
        }
        .in {
            width: 100px;
            height: 100px;
            background: blue;
            position: absolute;
            /*第一种*/
            top: 50%;
            left: 50%;
            margin-top:-50px;
            margin-left:-50px;
            /*第二种*/
            top:0;
            right:0;
            bottom:0;
            left:0;
            margin:auto;
        }
    ```

  4. 已知容器高度， 利用display: table-cell;

    ```css
        .out{
            width: 200px;
            height: 200px;
            background: pink;
            display:table-cell;
            vertical-align:middle;
        }

        .in {
            width: 100px;
            height: 100px;
            background: blue;
            margin: 0 auto;
        }
    ```

### <span id='5'>6. 用css实现，两行文本，间距10px,字体是14px,距顶端和底端15px,拒左边10px</span>

> word-spacing 对中文没有作用， 对字母有作用，从空格隔那开始起作用，而letter-spacing对两者都有作用，是字间的距离，包括空格哪里的也会隔开

    ```css
    .out{
        font-size: 14px;
        letter-spacing: 10px;
        padding: 15px 0 15px 10px;
    }
    ```

### <span id='6'>7. 扩展Date的format方法</span>

```js
Date.prototype.format = function (format) {
    var o = {
        "M+": this.getMonth() + 1,
        "d+": this.getDate(),
        "h+": this.getHours(),
        "m+": this.getMinutes(),
        "s+": this.getSeconds(),
        "q+": Math.floor((this.getMonth() + 3) / 3),
        "S": this.getMilliseconds()
    };
    if (/(y+)/.test(format)) {
        format = format.replace(RegExp.$1, (this.getFullYear() + "").substr(4 - RegExp.$1.length));
    }
    for (var k in o) {
        if (new RegExp("(" + k + ")").test(format)) {
            format = format.replace(RegExp.$1, RegExp.$1.length == 1 ? o[k] : ("00" + o[k]).substr(("" + o[k]).length));
        }
    }
    return format;
};
```
### <span id='7'>8. 写出下面结果</spanid>

```js
　　function b(c){
　　　　console.log(c);
　　　　function c(){
　　　　　　　console.log("d");
　　　　}
　　}
　　b(10);

// 结果 function c(){ console.log("d"); }
```
### <span id='8'>9. alert(1&&2),alert(1||0) </span>

    alert(1&&2)的结果是2
    只要“&&”前面是false，无论“&&”后面是true还是false，结果都将返“&&”前面的值;
    只要“&&”前面是true，无论“&&”后面是true还是false，结果都将返“&&”后面的值;
    alert(0||1)的结果是1
    只要“||”前面为false,不管“||”后面是true还是false，都返回“||”后面的值。
    只要“||”前面为true,不管“||”后面是true还是false，都返回“||”前面的值。

### <span id='9'>10. mouseenter和mouseover的区别</span>

    mouseover事件：不论鼠标指针穿过被选元素或其子元素，都会触发 mouseover 事件。
    mouseenter事件：只有在鼠标指针穿过被选元素时，才会触发 mouseenter 事件。
    mouseout事件：不论鼠标指针离开被选元素还是任何子元素，都会触发 mouseout 事件。
    mouseleave事件：只有在鼠标指针离开被选元素时，才会触发 mouseleave 事件。

### <span id='10'> 11. js字符串两边截取空白的trim的原型方法的实现</span>
```js
//我的笨方法，当时还想错了一些，回来后实现了一下，思路是这样
String.prototype.trim = function () {
    var arr=this.split("");
    while(1) {
       if(arr[0]==" ") {
           arr.shift();
           continue;
        }
        break;
    }
    while(1){
        if(arr[arr.length-1]==" ") {
            arr.pop();
            continue;
        }
        break;
    }
    return arr.join("");
}
//后来面试官跟我说一句话就解决了，然而我正则都忘了，平时没怎么用
String.prototype.trim = function () {
    return this.replace(/(^\s*)|(\s*$)/g,'');
};
```

### <span id='11'> 12. 三道判断输出的题都是经典的题</span>
```js
    //5.1
    var a=4;
    function b() {
      a=3;
      console.log(a);
      function a(){};
    }
    b();
    //明显输出是3，因为里面修改了a这个全局变量，那个function a(){}是用来干扰的，虽然函数声明会提升，就被a给覆盖掉了，这是我的理解
    //5.2
    //不记得具体的就类似如下
    var baz=3;
    var bazz={
      baz: 2,
      getbaz: function() {
            return this.baz
      }
    }
    console.log(bazz.getbaz())
    var g=bazz.getbaz;
    console.log(g());
    //第一个输出是2，第二个输出是3，这题考察的就是this的指向，函数作为对象本身属性调用的时候this指向对象，作为普通函数调用的时候就指向全局了
    //5.3
    var arr=[1,2,3,4,5];
    for(var i=0;i<arr.length;i++)
    { 
      arr[i]=function(){alert(i)}
    }
    arr[3]();
    //典型的闭包啊，看都不用看，肯定弹出5啊
```
### <span id='12'>13. position不同值和区别</span>

    1.absolute: 生成绝对定位的元素，相对于 static 定位以外的第一个父元素进行定位。
    元素的位置通过 “left”, “top”, “right” 以及 “bottom” 属性进行规定。(不占位)
    2.relative: 生成相对定位的元素，相对于其正常位置进行定位。因此，”left:20” 会向元素的 LEFT 位置添加 20 像素。（占位）
    3.static：默认值。没有定位，元素出现在正常的流中（忽略 top, bottom, left, right 或者 z-index 声明）
    4.inherit：规定应该从父元素继承 position 属性的值。
    5.fixed：生成绝对定位的元素，相对于浏览器窗口进行定位。元素的位置通过 “left”, “top”, “right” 以及 “bottom” 属性进行规定。

### <span id='13'>14. 讲述你对reflow和repaint的理解</span>

> repaint就是重绘，reflow就是回流。repaint主要是针对某一个DOM元素进行的重绘，reflow则是回流，针对整个页面的重排

### 严重性：

在性能优先的前提下，性能消耗 reflow大于repaint。

### 体现：

repaint是某个DOM元素进行重绘；reflow是整个页面进行重排，也就是页面所有DOM元素渲染。

#### 如何触发：

style变动造成repaint和reflow。
不涉及任何DOM元素的排版问题的变动为repaint，例如元素的color/text-align/text-decoration等等属性的变动。
除上面所提到的DOM元素style的修改基本为reflow。例如元素的任何涉及长、宽、行高、边框、display等style的修改。

### 常见触发场景：

#### 触发repaint：

    color的修改，如color=#ddd；
    text-align的修改，如text-align=center；
    a:hover也会造成重绘。
    :hover引起的颜色等不导致页面回流的style变动。

#### 触发reflow：

    width/height/border/margin/padding的修改，如width=778px；
    动画，:hover等伪类引起的元素表现改动，display=none等造成页面回流；
    appendChild等DOM元素操作；
    font类style的修改；
    background的修改，注意着字面上可能以为是重绘，但是浏览器确实回流了，经过浏览器厂家的优化，部分background的修改只触发repaint，当然IE不用考虑；
    scroll页面，这个不可避免；
    resize页面，桌面版本的进行浏览器大小的缩放，移动端的话，还没玩过能拖动程序，resize程序窗口大小的多窗口操作系统。
    读取元素的属性（这个无法理解，但是技术达人是这么说的，那就把它当做定理吧）：读取元素的某些属性（offsetLeft、offsetTop、offsetHeight、offsetWidth、scrollTop/Left/Width/Height、clientTop/Left/Width/Height、getComputedStyle()、currentStyle(in IE))；

#### 如何避免：

尽可能在DOM末梢通过改变class来修改元素的style属性：尽可能的减少受影响的DOM元素。
避免设置多项内联样式：使用常用的class的方式进行设置样式，以避免设置样式时访问DOM的低效率。
设置动画元素position属性为fixed或者absolute：由于当前元素从DOM流中独立出来，因此受影响的只有当前元素，元素repaint。
牺牲平滑度满足性能：动画精度太强，会造成更多次的repaint/reflow，牺牲精度，能满足性能的损耗，获取性能和平滑度的平衡。
避免使用table进行布局：table的每个元素的大小以及内容的改动，都会导致整个table进行重新计算，造成大幅度的repaint或者reflow。改用div则可以进行针对性的repaint和避免不必要的reflow。
避免在CSS中使用运算式：学习CSS的时候就知道，这个应该避免，不应该加深到这一层再去了解，因为这个的后果确实非常严重，一旦存在动画性的repaint/reflow，那么每一帧动画都会进行计算，性能消耗不容小觑。

[interview](https://segmentfault.com/a/1190000002629708)
[webpack](https://segmentfault.com/a/1190000014148611)
[http](https://www.cnblogs.com/daisygogogo/p/10741597.html)
[http](https://www.cnblogs.com/ranyonsue/p/5984001.html)
[tcp/ip](https://blog.csdn.net/qq_41517936/article/details/80886618)
[es6解析](https://juejin.im/post/5c6234f16fb9a049a81fcca5)
[SSR](https://www.jianshu.com/p/10b6074d772c)