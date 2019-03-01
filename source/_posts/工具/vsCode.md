---
title: vscode plug-in
categories:
  - web skill
tags:
  - plug-in
date: 2017-12-03 13:38:50
---

> Microsoft在2015年4月30日Build 开发者大会上正式宣布了 Visual Studio Code 项目：一个运行于 Mac OS X、Windows和 Linux 之上的，针对于编写现代 Web 和云应用的跨平台源代码编辑器,一款轻量，采取了和VS相同的UI界面，搭配合适的插件可以优化前端开发的体验

<!-- more -->

### 安装

[vscode官网](https://code.visualstudio.com/Download)

VScode 中文设置

1. 快捷键F1
2. 输入Configure Language 回车
3. “locale”:  "zh-CN"，locale设置为zh-CNK
4. 保存, 重启vscode

### 布局

* 左侧是用于展示所要编辑的所有文件和文件夹的文件管理器，依次是`资源管理器`，`搜索`，`GIT`，`调试`，`插件`，

* 右侧是打开文件的编辑区域，最多可同时打开三个编辑区域到侧边。

* 底栏：依次是`Git Branch`，`error&warning`，`编码格式`等。

### 插件

* vscode-icon

F1->icon-->icon Theme-->选择(安装自己喜欢的主题)

> 这款必须要推荐，明显提升效率的小插件，在项目文件多类型多的情况下，找到制定文件会大大缩短时间；

* fileheader

修改作者 文件--->首选项--->设置--->fileheader--->修改

> 顶部注释模板，可定义作者、时间等信息，并会自动更新最后修改时间, 快捷键 Ctrl+Alt+i

* HTML Snippets 

Ctrl+Shift+P-->输入snippets-->选择语言-->打开.json文件-->配置-->使用

> 超级实用且初级的 H5代码片段以及提示

* JavaScript Snippet Pack

> 针对js的插件，包含了js的常用语法关键字，很实用, 代码片段(Tab或者Enter补全)

* JavaScript Snippets

> 此扩展包含Visual Studio代码编辑器（支持JavaScript和TypeScript）的ES6语法中的JavaScript代码片段。

* HTML CSS Support

> 在编写样式表的时候，自动补全功能大大缩减了编写时间，推荐！让 html 标签上写class 智能提示当前项目所支持的样式新版已经支持scss文件检索   (提示已有的class名)

* Auto Close Tag

> 编写html代码的时候，写完开始标签，这款插件会自动补全结束标签，其实上面所说的html自动补全插件一个Tab就搞定了，不过有时也需要这款插件；

* Auto Rename Tag

> 非常实用！要修改标签名称的时候自动修改结束标签，节省一半时间，提升效率，非常棒！

* Document this

> js 的注释模板 （注意：新版的vscode已经原生支持,在function上输入/** tab） Ctrl+Alt+D 快捷键, 光标放在关键字上

* Change Case

下划线命名  <-->   驼峰命名    大小写转换   

> 虽然 VSCode 内置了开箱即用的文本转换选项，但其只能进行文本大小写的转换。而此插件则添加了用于修改文本的更多命名格式，包括驼峰命名、下划线分隔命名，snake_case 命名以及 CONST_CAS 命名等

* jQuery Code Snippets

> jquery提示插件

* Path Intellisense

> 自动路劲补全，默认不带这个功能的，赶紧装

* Emoji  

F1-->emo-->选择插入

> 很好玩的一款插件，可以在代码中插入emoji了，也许是程序猿的娱乐方式吧；

* Open-In-Browser

> 由于 VSCode 没有提供直接在浏览器中打开文件的内置界面，所以此插件在快捷菜单中添加了在默认浏览器查看文件选项，以及在客户端（Firefox，Chrome，IE）中打开命令面板选项

* Code Runner

> 非常强大的一款插件，能够运行多种语言的代码片段或代码文件：C，C ++，Java，JavaScript，PHP，Python，Perl，Ruby，Go等等，安装完成后，右上角出现一个三角形，点击这个按钮就可以运行你的文件了（必备）。

* Dash

> 查文档必备，搭配 dash（不过似乎只有 mac 版）,快捷键 ctrl + h 它根据你当前选中的语言查找 dash 里面的文档

* Debugger for Chrome

> 让 vscode 映射 chrome 的 debug功能，使静态页面都可以用 vscode 来打断点调试

* CSS Peek(窥视)

> 使用此插件，你可以追踪至样式表中 CSS 类和 ids 定义的地方。当你在 HTML 文件中右键单击选择器时，选择“ Go to Definition 和 Peek definition ”选项，它便会给你发送样式设置的 CSS 代码。

* HTML Boilerplate(样板)

> 通过使用 HTML 模版插件，你就摆脱了为 HTML 新文件重新编写头部和正文标签的苦恼。你只需在空文件中输入 html，并按 Tab 键，即可生成干净的文档结构。

* Prettier(格式化)

快捷键 Shift+Alt+F /////  Ctrl+Shift+P(F1)-->键入format-->格式化代码

> Prettier 是目前 Web 开发中最受欢迎的代码格式化程序。安装了这个插件，它就能够自动应用 Prettier，并将整个 JS 和 CSS 文档快速格式化为统一的代码样式。如果你还想使用 ESLint，那么还有个 Prettier – Eslint 插件，你可不要错过咯！

* Color Info

> 这个便捷的插件，将为你提供你在 CSS 中使用颜色的相关信息。你只需在颜色上悬停光标，就可以预览色块中色彩模型的（HEX、 RGB、HSL 和 CMYK）相关信息了

* SVG Viewer

> 此插件在 Visual Studio 代码中添加了许多实用的 SVG 程序，你无需离开编辑器，便可以打开 SVG 文件并查看它们。同时，它还包含了用于转换为 PNG 格式和生成数据 URI 模式的选项。

* Minify

F1-->minify

> 这是一款用于压缩合并 JavaScript 和 CSS 文件的应用程序。它提供了大量自定义的设置，以及自动压缩保存并导出为.min文件的选项。它能够分别通过 uglify-js、clean-css 和 html-minifier，与 JavaScript、CSS 和 HTML 协同工作。

* Quokka(调试工具插件)

> Quokka 是一个调试工具插件，能够根据你正在编写的代码提供实时反馈。它易于配置，并能够预览变量的函数和计算值结果。另外，在使用 JSX 或 TypeScript 项目中，它能够开箱即用

* ESLint

> EsLint可以帮助我们检查Javascript编程时的语法错误。比如：在Javascript应用中，你很难找到你漏泄的变量或者方法。EsLint能够帮助我们分析JS代码，找到bug并确保一定程度的JS语法书写的正确性。

* Font-awesome

> 用于 html 的Font-awesome代码片段

* filesize

> 在底部状态栏显示当前文件大小，点击后还可以看到详细创建、修改时间

* Git History

> 使用 command+shift+p（Ctrl+shift+p） 输入git log就可以看到了

* htmltagwrap

> 可以在选中HTML标签中外面套一层标签

使用：选择一大段代码，然后按“Alt + W”

* Image Preview

> 鼠标移到路径里显示图像预览

* Live Sass Compiler

> 实时编译 sass ,不过需要配置，附上我的配置

* markdownlint

> markdown 语法检查

* npm Intellisense

> 在导入语句中自动填充npm模块,跟Node.js Modules Intellisense差不多

* Project Manager

> 工程项目过多时，shift+cmd+p(shift+ctrl+p) 然后输入project，第一次选择edit Project编辑自己的工程项目，之后就可以直接选择open打开你的项目

* vscode-faker

> 生成假数据，地址，电话，图片等等

* Regex Previewer

> 测试正则的插件v

* React-Native/React/Redux snippets for es6/es7

> react代码片段，下载人数超多

* react-beautify

> 格式化 javascript, JSX, typescript, TSX 文件