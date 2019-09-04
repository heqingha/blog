---
title: js数组
categories:
  - web skill
tags:
  - js
date: 2018-05-07 11:03:28
---

> javascript数组的方法，包含es6，Array 对象用于在单个的变量中存储多个值。

<!-- more -->

创建 Array 对象的语法：

```js
new Array();
new Array(size);
new Array(element0, element1, ..., elementn);
```
![for example](/img/js/array1.png);

### 1. concat() 方法用于连接两个或多个数组

该方法不会改变现有的数组，而仅仅会返回被连接数组的一个副本

```js
var a = [1,2,3]
var b = [4,5,6]
a.concat(b)

result: 返回新数组 [1,2,3,4,5,6]  a = [1,2,3]  b = [3,4,5]
```

### 2. join() 方法用于把数组中的所有元素放入一个字符串。

元素是通过指定的分隔符进行分隔的。

```js
var a = [1,2,3]
a.join('-')

result: 返回新string '1-2-3'   a = [1,2,3]
```

### 3. pop() 方法用于删除并返回数组的最后一个元素。

```js
var a = [1,2,3]
a.pop()

result: [1,2]    a = [1,2] (被改变) 返回被删除的值
```
### 4. push() 方法用于添加元素，返回push的最后一个值。

```js
var a = [1,2,3]
a.push(4,5,6)

result: [1,2,3,4,5,6]    a = [1,2,3,4,5,6] (被改变) 返回push的最后一个值
```

### 5. reverse() 方法用于颠倒数组中元素的顺序。

```js
var a = [1,2,3]
a.reverse()

result: [3,2,1]    a = [3,2,1] (被改变) 返回颠倒数组
```
### 6. shift() 方法用于把数组的第一个元素从其中删除，并返回第一个元素的值。

```js
var a = [1,2,3]
a.shift()

result: [2,3]    a = [2,3] (被改变) 返回第一个元素的值 1
```
### 7. unshift() 方法可向数组的开头添加一个或更多元素，并返回新的长度。

```js
var a = [1,2,3]
a.unshift(0,9)

result [0,9,1,2,3]  a = [0,9,1,2,3] (被改变) 返回数组的长度 6
```

### 8. slice() 方法可从已有的数组中返回选定的元素。

```js
var a = [1,2,3]
a.slice(0,2)

result [1,2]  a = [1,2,3] 返回截取的数组 [1,2], 和splice()不同，splice会改变原数组
```

### 9. sort() 方法用于对数组的元素进行排序。

```js
var a = [6,9,1]
var b = [1, 5, 10, 25, 40, 1000]
a.sort()
b.sort()  // 1, 10, 1000, 25, 40, 5(说明)
function sortNumber(a,b) {
    return a - b
    }
b.sort(sortNumber) //right

result [1,6,9]  a = [1,6,9] (被改变) 返回排序后的数组

```

ps : 说明

如果调用该方法时没有使用参数，将按 **字母** 顺序对数组中的元素进行排序，说得更精确点，是按照字符编码的顺序进行排序。要实现这一点，首先应把数组的元素都转换成字符串（如有必要），以便进行比较。

如果想按照其他标准进行排序，就需要提供比较函数，该函数要比较两个值，然后返回一个用于说明这两个值的相对顺序的数字。比较函数应该具有两个参数 a 和 b，其返回值如下：

若 a 小于 b，在排序后的数组中 a 应该出现在 b 之前，则返回一个小于 0 的值。
若 a 等于 b，则返回 0。
若 a 大于 b，则返回一个大于 0 的值。

### 10. splice() 方法向/从数组中添加/删除项目，然后返回被删除的项目。该方法会改变原始数组

```js
var a = [1,2,3]
a.splice(0,0,4)  
//1.规定添加/删除项目的位置，使用负数可从数组结尾处规定位置
//2.要删除的项目数量。如果设置为 0，则不会删除项目
//3.可选。向数组添加的新项目。
result [4,1,2,3]  a = [4,1,2,3] (被改变) 返回选取的数组
```

### 11. toSource()方法表示对象的源代码

```js
function employee(name,job,born)
    {
    this.name=name;
    this.job=job;
    this.born=born;
}

var bill=new employee("Bill Gates","Engineer",1985);
document.write(bill.toSource());
//只有 Gecko 核心的浏览器（比如 Firefox）支持该方法，也就是说 IE、Safari、Chrome、Opera 等浏览器均不支持该方法。
```

### 12. toString() 方法可把数组转换为字符串，并返回结果。

```js
var a = [1,2,3]
a.toString()  // 1,2,3 不接受参数，带, 不同于join,'1-2-3'

result: '1,2,3' 不改变原数组， 返回字符串
```

### 13. valueOf() 方法返回 Array 对象的原始值。

```js
var a = ['1', '2', '3']
a.valueOf() 

result: ["1", "2", "3"] 
```
### 14. toLocaleString() 把数组转换为本地字符串.

```js
var a = ['1', '2', '3', 'a']
a.toLocaleString() 

result: "1,2,3,a"  不改变原数组
```

### 15. indexOf()从数组开头开始查找所在项的位置,lastIndexOf() 从数组末尾开始向前查找

> 比较参数和数组中每一项时，使用全等操作符。=== 没有找到时返回-1

### 参考链接

[MDN Array](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array)

[SegmentFault](https://segmentfault.com/a/1190000005029014)

[阮一峰 ECMAScript 6 入门 Array](http://es6.ruanyifeng.com/#docs/array)

