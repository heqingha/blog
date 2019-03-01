---
title: css3
date: 2017-10-17 14:37:12
tags:
- css3
categories:
- web skill
---

## <span id='0'>[calc()使用](http://www.w3cplus.com/css3/how-to-use-css3-calc-function.html)</span>

> 能实现自适应的布局 box-sizing, 浏览器对calc()的兼容性还算不错，在IE9+、FF4.0+、Chrome19+、Safari6+都得到较好支持著作权归作者所有。不过calc()最大的好处就是用在流体布局上(百分比)，可以通过calc()计算得到元素的宽度高度

<!-- more -->

```css
    #div{
        float: left;
        width: 50%;
        width:calc(50% - 2px - 80px);
        height: 100px;
        background: red;
        border: thin solid;
        padding: 40px;
        //box-sizing: border-box;
    }
```

* [calc()使用](#0)
* [边框](#1)
* [背景](#2)
* [渐变](#3)
* [文本效果](#4)
* [2D转换](#5)
* [3D转换](#6)
* [过渡](#7)
* [动画](#8)
* [多列](#9)
* [用户界面](#10)
* [弹性盒](#11)

## <span id='1'>边框</span>

> 用 CSS3，你可以创建圆角边框，添加阴影框，并作为边界的形象而不使用设计程序

* border-radius (圆角)
```css
    <div id='div'></div>
    #div
    {
        width: 200px;
        height: 200px;
        border:2px solid;
        border-radius:25px;
    }
```
* box-shadow  (盒阴影)
```css
    #div
        {
            width: 200px;
            height: 200px;
            box-shadow: 20px 20px 20px 0 gold inset;
            box-shadow: 10px 10px 5px #888888;
            background: red;
            margin: 50px;
            // box-shadow: 10px(向右偏移,可取负数,必填) 
                           10px(向下偏移,可取负数,必填) 
                           20px(模糊距离,可选)
                           5px(阴影尺寸 可选)                                      
                           #888888(颜色,可选 默认黑色)
                           inset(内阴影,可选);
        }
```
* [border-image](http://www.runoob.com/cssref/css3-pr-border-image.html)  (边界图片) ...Internet Explorer 不支持

        border-image:   source(图片路径) 
                        slice(图像边界向内偏移) 
                        width(图像边界的宽度) 
                        outset(用于指定在边框外部绘制 border-image-area 的量) 
                        repeat|initial|inherit(用于设置图像边界是否应重复（repeat）、拉伸（stretch）或铺满（round）);
    默认border-image: none 100% 1 0 stretch

## <span id='2'>背景</span>

* background-size: 100% 100%(200px 200px)注:(写成px的时候不包含padding 加box-sizing: border-box 一样)   (规定背景的绘制区域)

```css
    #div {
        width: 200px;
        height: 200px;
        padding: 20px;
        background: #ccc url('http://pic10.nipic.com/20101014/5888301_112136782000_2.jpg') no-repeat;
        background-size: 100% 100%;
        box-sizing: border-box;
        /* background-size: 240px 240px; *
    }

```
* background-origin  (content-box(只填充内容区域)、padding-box(包含padding) 或 border-box(包含边框) ) =>  相对于背景图片(背景色完全没有作用  规定背景图片的定位区域)

```css
    #div {
        width: 200px;
        height: 200px;
        padding: 20px;
        border: 10px dotted red;
        background: #ccc url('http://pic10.nipic.com/20101014/5888301_112136782000_2.jpg') no-repeat;
        background-size: 100% 100%;
        background-origin: content-box;
        //background-origin: padding-box;
        //background-origin: border-box;
    }

```
* background-clip (content-box(只填充内容区域)、padding-box(包含padding) 或 border-box(包含边框) )=>相对于背景色(对图片的padding content 有作用  对border-box无用(默认就是))  (规定背景图片的尺寸)

```css
    #div {
        width: 200px;
        height: 200px;
        padding: 20px;
        background:  url('http://pic10.nipic.com/20101014/5888301_112136782000_2.jpg') no-repeat;
        background-size: 100% 100%;
        background-color: green;
        background-clip: border-box;
        /* background-origin: border-box; */
        border: 10px dashed red;
    }
```
* background-origin与background-clip区别

1. background-origin只作用于背景图片,对背景色无用, background-clip即可作用于背景图片,也可作用于背景色
2. 当两者都作用于背景图片时, 不同于border-box 属性,background-origin边框在图片上面, 并且填充了边框,  background-clip 没有填充边框

## <span id='3'>渐变 (线性渐变) (径向渐变)</span>

> 在两个或多个指定的颜色之间显示平稳的过渡,如果你想要在渐变的方向上做更多的控制，你可以定义一个角度(逆时针)，而不用预定义方向（to bottom、to top、to right、to left、to bottom right，等等）

* 线性渐变 background: linear-gradient(direction(90deg), color-stop1, color-stop2, ...);

```css
    #div 
        {
        width: 200px;
        height: 200px;
        background: -webkit-linear-gradient(top, red, blue, green);
        //background: -webkit-linear-gradient(left, red, blue, green);
        //background: -webkit-linear-gradient(top left, red, blue, green);
        //background: -webkit-linear-gradient(left bottom, red, blue, green);
        //background: -webkit-repeating-linear-gradient(top left, red, blue 10%,green 30%);//重复渐变
    }
```

* 径向渐变 background: radial-gradient(center, shape(ellipse/circle) size, start-color, ..., last-color);

```css
    #div
        {
            width: 200px;
            height: 200px;
            background: -webkit-radial-gradient(red, green, blue);
            //background: -webkit-radial-gradient(red 5%, green 15%, blue 60%);
            //background: -webkit-repeating-radial-gradient(red, green 10%, blue 15%);//重复渐变
        }
```
## <span id='4'>文本效果</span>

* text-shadow: 5px(水平阴影)   5px(垂直阴影)   10px(模糊距离)   green;
```css
    #div
        {
            width: 200px;
            height: 200px;
            text-shadow: 5px 5px 5px #FF0000;
            font-size: 40px;
        }
```
* word-wrap:break-word(自动换行)   文字超出会自动换行    字母 数字 标点符号不换  (ps不兼容 Opera)

```css
 #div
    {
        width: 200px;
        height: 200px;
        text-shadow: 5px 5px 5px #FF0000;
        font-size: 40px;
        border: 2px solid red;
        word-wrap:break-word;
    }
```
## <span id='5'>2D转换</span>

 > 可以移动，比例化，反过来，旋转，和拉伸元素。
 transform  (IE10 ff  Opera 支持. IE9(-ms-)和Chrome Safari(-webkit-) 要加前缀)

* transform: translate(50px, 50px) (translateX/Y)    (上下 作用偏移,以元素中心点为中心偏移,设置transform-origin属性无用)
* transform: rotate(90deg) rotate(X/Y)               (旋转,以中心为原点旋转,可改变中心点)
* transform: scale(2,2) (scaleX/Y)                   (元素尺寸增加或减少倍数,中心点旋转,可改变中心点)
* transform: skew(10deg,0deg) (skewX/Y)              (元素翻转 x y,中心点旋转,可改变中心点)
* transform:matrix(0.866,0.5,-0.5,0.866,0,0)         (matrix()方法,六个参数，包含旋转，缩放，移动（平移）和倾斜功能)
* transform-origin:0 0;                              (允许改变被转换元素的位置,默认元素中心点)

```css
    #div
    {
        width: 200px;
        height: 200px;
        background: red;
        //transform:translate(200px,200px);
        //transform: rotate(30deg);
        //transform-origin:0 0;
        //transform: skew(30deg,20deg);
        transform: scale(2,2)
        //transform:matrix(0.866,0.5,-0.5,0.866,0,0);
        margin: 100px auto;
        transform-origin:0 0;
        //animation:myfirst 5s linear 2s infinite alternate;
    }
    @keyframes myfirst
        {
        from {transform: scale(1,1)}
        to {transform: scale(2,2)}
        }
```
## <span id='6'>3D转换</span>

> 方法 同2D 只是多了Z轴  rotate可以规定X Y Z (transform: rotateX/Y/Z(30deg))

* transform: translate3d(x,y,z)
* transform: scale3d(x,y,z)
* transform: rotate3d(x,y,z,angle)
* perspective: 500 (设置从何处查看一个元素的角度,perspective 属性只影响 3D 转换元素)
* perspective-origin: 10% 10% (设置一个3D元素的基数位置)
* transform-style: preserve-3d (让转换的子元素保留3D转换)

## <span id='7'>过渡</span>

> 为了添加某种效果可以从一种样式转变到另一个的时候，无需使用Flash动画或JavaScript

* transition 属性是一个简写属性，用于设置四个过渡属性  (默认all 0 ease 0) (给运动元素加此属性,hover时加要变的属性)
* transition-property (哪个属性变)
* transition-duration (完成这个效果的时间  必须设置 否则默认为0)
* transition-timing-function ([规定速度效果的速度曲线](http://www.runoob.com/cssref/css3-pr-transition-timing-function.html)   linear|ease|ease-in|ease-out|ease-in-out)
* transition-delay (效果延时多少开始)

```css
    #div
        {
            width:100px;
            height:100px;
            background:red;
            transition:width 2s;
        }

    #div:hover
        {
            width:300px;
        }
```
## <span id='8'>动画</span>

* @keyframes 规则用于创建动画。在 @keyframes 中规定某项 CSS 样式，就能创建由当前样式逐渐改为新样式的动画效果
* 先定义动画,然后在动画元素属性里面调用,(animation: 动画名  运动的时间 [速度曲线](http://www.runoob.com/cssref/css3-pr-animation-timing-function.html) 延时多少开始 运动次数(infinite 无数次)  反向播放动画 )
* animation:myfirst 5s linear 2s infinite alternate;
* animation-play-state:running/paused (属性规定动画正在运行还是暂停 js中使用 可以控制动画暂停与播放)

```css
    #div 
        {
            width: 100px;
            height: 100px;
            position:relative;
            animation:myfirst 5s linear 2s infinite alternate;
            //animation-play-state:paused;
        }
    @keyframes myfirst
        {
            0%   {background:red; left:0px; top:0px;}
            25%  {background:yellow; left:200px; top:0px;}
            50%  {background:blue; left:200px; top:200px;}
            75%  {background:green; left:0px; top:200px;}
            100% {background:red; left:0px; top:0px;}
        } 
```

## <span id='9'>多列</span>

> 可以将文本内容设计成像报纸一样的多列布局

* column-count 属性规定元素应该被分隔的列数  (column-count:3)
* column-gap 属性规定列之间的间隔 (column-gap:30px)
* column-rule 属性设置列之间的宽度、样式和颜色规则  (column-rule:4px outset #ff0000)
* column-width 属性规定列的宽度  (column-width: auto(浏览器决定 没有效果) / 100px)
* column-span 属性规定元素应横跨多少列  (column-span: 1 / all)
* columns 属性是一个简写属性，用于设置列宽和列数   (columns:100px(4) 3(1);)

```css
    <div id="div">
        <h2>英国维斯米斯特教堂碑文</h2>
        当我年轻的时候，我梦想改变这个世界；当我成熟以后，我发现我不能够改变这个世界，我将目光缩短了些，决定只改变我的国家；当我进入暮年以后，我发现我不能够改变我们的国家，我的最后愿望仅仅是改变一下我的家庭，但是，这也不可能。当我现在躺在床上，行将就木时，我突然意识到：如果一开始我仅仅去改变我自己，然后，我可能改变我的家庭；在家人的帮助和鼓励下，我可能为国家做一些事情；然后，谁知道呢?我甚至可能改变这个世界。
    </div>
    #div 
        {
            column-count: 3;
            column-gap:90px;
            column-rule:4px outset #ff0000;
            //column-rule:4px dotted #ff0000;
            column-span: all;
            column-width:100px;
            //column-span: 1; // 默认
            //columns:100px 3;
        }
        h2
        {
            column-span:1;
            -webkit-column-span:1; /* Safari and Chrome */
        }
```

## <span id="10">用户界面</span> 

* 调整尺寸(Resizing)

```css
    <div id='div'>调整属性指定一个元素是否由用户可调整大小的</div>
    #div
        {
            border:2px solid;
            padding:10px 40px; 
            width:300px;
            resize:both;
            overflow:auto;
        }
```
* 外形修饰（outline-offset ）

```css
    <div id='div'>
        这个 div有一个轮廓边界15 px边境外的边缘。Internet Explorer 不兼容 outline-offset属性.
    </div>
    #div
        {
            margin:20px;
            width:150px; 
            padding:10px;
            height:70px;
            border:2px solid black;
            outline:2px solid red;
            outline-offset:15px;
        }
```

##  <span id="11">弹性盒</span> 

> 弹性盒子是 CSS3 的一种新的布局模式。CSS3 弹性盒（ Flexible Box 或 flexbox），是一种当页面需要适应不同的屏幕大小以及设备类型时确保元素拥有恰当的行为的布局方式。引入弹性盒布局模型的目的是提供一种更加有效的方式来对一个容器中的子元素进行排列、对齐和分配空白空间

* 弹性盒子 = 弹性容器(Flex container) + 弹性子元素(Flex item)
* 弹性容器设置 display 属性的值为 flex 或 inline-flex将其定义为弹性容器(弹性容器上)
* direction: rtl (right-to-left) 修改弹性子元素的排列方式也会改变，页面布局也跟着改变 (弹性容器上)
* flex-direction 顺序指定了弹性子元素在父容器中的位置 (flex-direction: row | row-reverse | column | column-reverse)(弹性容器上)
    * row：横向从左到右排列（左对齐），默认
    * row-reverse：反转横向排列（右对齐，从后往前排，最后一项排在最前面
    * column：纵向排列
    * column-reverse：反转纵向排列，从后往前排，最后一项排在最上面
* justify-content(内容对齐): flex-start | flex-end | center | space-between | space-around (弹性容器上)
* align-items(设置或检索弹性盒子元素在侧轴（纵轴）方向上的对齐方式): flex-start | flex-end | center | baseline | stretch(弹性容器上)
* flex-wrap(用于指定弹性盒子的子元素换行方式): nowrap|wrap|wrap-reverse|initial|inherit; (弹性容器上)
    * nowrap - 默认， 弹性容器为单行。该情况下弹性子项可能会溢出容器。
    * wrap - 弹性容器为多行。该情况下弹性子项溢出的部分会被放置到新行，子项内部会发生断行
    * wrap-reverse -反转 wrap 排列
* align-content(用于修改 flex-wrap 属性的行为): flex-start | flex-end | center | space-between | space-around | stretch, 类似于 align-items, 但它不是设置弹性子元素的对齐，而是设置各个行的对齐 (弹性容器上)
    * stretch - 默认。各行将会伸展以占用剩余的空间。
    * flex-start - 各行向弹性盒容器的起始位置堆叠。
    * flex-end - 各行向弹性盒容器的结束位置堆叠。
    * center -各行向弹性盒容器的中间位置堆叠。
    * space-between -各行在弹性盒容器中平均分布。
    * space-around - 各行在弹性盒容器中平均分布，两端保留子元素与子元素之间间距大小的一半。 

### 弹性子元素属性

* order 属性设置弹性容器内弹性子元素的属性
* align-self (在弹性子元素上使用。覆盖容器的 align-items 属性): auto | flex-start | flex-end | center | baseline | stretch
* flex (设置弹性盒子的子元素如何分配空间): auto | initial | none | inherit |  [ flex-grow ] || [ flex-shrink ] || [ flex-basis ]

```css
    <div class="flex-container">
        <div class="flex-item">flex item 1</div>
        <div class="flex-item">flex item 2</div>
        <div class="flex-item">flex item 3</div>
        <div class="flex-item">flex item 4</div>  
    </div>

    .flex-container {
        display: -webkit-flex;
        display: flex;
        width: 400px;
        height: 250px;
        background-color: lightgrey;
        //direction: rtl;
        //flex-direction: row;
        //flex-direction: row-reverse;
        //flex-direction: column;
        //flex-direction: column-reverse;
        //justify-content: flex-start;
        //justify-content: flex-end;
        //justify-content: center;
        //justify-content: space-between;
        //justify-content: space-around;
        //flex-wrap: nowrap;
        //flex-wrap: wrap;
        //flex-wrap: wrap-reverse;
    }

    .flex-item {
        background-color: cornflowerblue;
        width: 15%;
        height: 100px;
        margin: 10px;
    }
```
