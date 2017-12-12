---
title: Markdown
date: 2017-10-17 13:49:58
categories:
- 语法
tags: 语法
---

> Markdown是一种可以使用普通文本编辑器编写的[标记语言](https://baike.baidu.com/item/%E6%A0%87%E8%AE%B0%E8%AF%AD%E8%A8%80/5964436?fr=aladdin, '标记语言')，通过简单的标记语法，它可以使普通文本内容具有一定的格式

<!-- more -->

## 用途
> Markdown的语法简洁明了、学习容易，而且功能比纯文本更强，因此有很多人用它写博客 当然还有其他的用途

## 目标
> 实现「易读易写」成为一种适用于网络的书写语言，Markdown 的语法全由一些符号所组成，这些符号经过精挑细选，其作用一目了然

## Markdown 语法的简要规则

## 1.标题

> 标题是每篇文章都需要也是最常用的格式，Markdown支持两种标题语法，Settext和atx形式。

> Settext形式：利用底线形式，用 = （最高级标题） 和 - （第二级标题）作为底线形式标题的标记。

> atx形式：在行首插入 1 到 6 个 #，对应标题 1 到 6 级（从大到小顺序）。

标题Settext形式
=====================
标题Settext形式
---------------------

# 一级标题
## 二级标题
### 三级标题
#### 四级标题
##### 五级标题
###### 六级标题

## 2.引用

> 文章中经常引用别人的话，为区别开来可以用引用标记特别注明。Markdown中使用email形式的 ‘>‘（右尖括号号）。可以在引用段落前加一个，也可以在段落每行前面加一个。如果区块引用由几段组成，引用段落之间需要加一个 >区分引用不同段落（这是引用标记和段落标记的组合）

> This is a blockquote.
>
> This is the second paragraph in the blockquote.
>
> ## This is an H2 in a blockquote

## 3.段落

> 一篇文章由N个段落组成。一个段落由一个或多个连续的行构成，Markdown通过一个以上的空行区别不同的段落。只要看起来像空行，不管是由空格符还是tab组成的，一律视为空行。

> 多个空格会被看成一个空格。一般段落首行没有缩进，如果需要首行缩进的话可用&emsp; (全方大空白)或 &ensp; （半方大空格）

&emsp;&emsp;这是段落1

&ensp;&ensp;这是段落2

## 4.强调

> Markdown使用星号 * 和底线 _ 标记强调区段。在强调部分前加上标记，在强调部分结束后加上标记限制强调范围。注意星号强调标志与强调内容之间不能有空格存在，不然变成无序列表了。

*强调部分变为斜体*   

_强调部分变为斜体_

**加重强调部分变为粗体**  

__加重强调变为粗体__

***特别强调部分变为粗斜体*** 

___特别强调部分变为粗斜体___

## 5.列表

> 列表分为无序列表和有序列表两种。 无序列表使用星号、加号、减号三种中任意一种作为无序列表的项目标记。三种符号可混用，但不建议这样做。注意列表标记和列表项目之间必须至少一个空格。

*  无序列表中一项
* 子项，以一个tab或者4个空格缩进
* 无序列表中另一项
+ 无序列表另一项
- 无序列表另一项

> 有序列表使用数字接一个英文句点作为项目标记。注意数字和句点间不能有空格，句点和项目内容之间至少有一个空格

1. 有序列表一项
2. 有序列表另一项
1. 有序列表最后一项

## 6.链接

> Markdown支持两种形式链接语法

> 1.行内插入链接：要建立一个行内式的链接，只要在方块括号后面紧接着圆括号并插入网址链接即可，如果你还想要加上链接的 title 文字，只要在网址后面，用双引号把 title 文字包起来即可

[链接文字链接文字](链接地址)

示例: [github](http://www.github.com)

This is [an example](http://example.com/ "Title") inline link.

[This link](http://example.net/) has no title attribute.

> 2.参考式的链接是在链接文字的括号后面再接上另一个方括号，而在第二个方括号里面要填入用以辨识链接的标记

[链接文字][链接引用标签]

[链接引用标签]: 链接地址 "http://www.baidu.com"

锚点  跳到页面的最后

This is [an example](#jump) reference-style link.

## 7.图片

> 图片引用类似于链接引用。图片可以是网络图片，也可以是本地图片，本地图片建议放在source/images目录中，按年月建立两层目录放置便于归档。本地图片的url类似为/images/year/month/img.png。

```
![alt MyEmail](/images/myemail.png "MyEmail")

```

例子：![alt MyEmail](/images/myemail.png "MyEmail")

## 8.代码

> 第一种：简单文字出现一个代码框。使用`<blockquote>`。（`不是单引号而是左上角的ESC下面~中的`）

> 第二种：大片文字需要实现代码框。使用Tab和四个空格。

`let a = 10;`

        let b = 12;
        let c = 22;

## 9.水平分割线

> 要生成水平分割线，可以在单独一行里输入3个或以上的短横线（减号）、星号或者下划线实现。短横线和星号之间可以输入任意空格。以下每一行都产生一条水平分割线

-------------------
**     *
__  _
__    _

## 10.表格

## 常用语法

| 输出后的效果 | Markdown | 快捷键 |
| ------------- |:-------------:| -----:|
| Bold | **text** | Ctrl/⌘ + B |
| Emphasize | *text* | Ctrl/⌘ + I |
| Strike-through | ~~text~~ | Ctrl + Alt + U |
| Link | [title](http://www.baidu.com)| Ctrl/⌘ + K |
| Inline Code | `code` | Ctrl/⌘ + Shift + K |
| Image | ![alt](http://) | Ctrl/⌘ + Shift + I |
| List | * item |Ctrl + L |
| Blockquote | > quote | Ctrl + Q |
| H1 | # H1 | Ctrl/⌘ + I |
| H2 | ## H2 | Ctrl/⌘ + H |
| H3 | ### H3 | Ctrl/⌘ + H (x2) |

## 11.片段

这是一个普通段落：

	这是一个代码区块。

<p>这是一个普通段落：</p>
<pre>
<code>这是一个代码区块。</code>
</pre>

```javascript
  var ihubo = {
    nickName  : "草依山",
    site : "http://jser.me"
  }
```
<span id = "jump">跳转到这里：</span>

## 感谢链接：

* [Markdown语法](http://www.appinn.com/markdown/)
* [Markdown-语法手册](http://blog.leanote.com/post/freewalk/Markdown-%E8%AF%AD%E6%B3%95%E6%89%8B%E5%86%8C)


