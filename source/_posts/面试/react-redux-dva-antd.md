---
title: 前端框架
categories:
  - 前端技术
tags:
  - 前端框架
date: 2019-02-24 18:51:20
---

> 前端三大框架概念区别简总结（angular,react,vue）

<!--- more -->

**MVX 框架模式：MVC+MVP+MVVM**

1. MVC：Model+View+controller（M 和 V 的协调者），主要是基于分层的目的，让彼此的职责分开。

   用户 User 通过控制器 Controller 来操作模板 Model 从而达到视图 View 的变化。数据直接从数据库读取

2. MVP：是从 MVC 模式演变而来的，都是通过 Controller/Presenter 负责逻辑的处理+Model 提供数据+View 负责显示。

   在 MVP 中，Presenter 完全把 View 和 Model 进行了分离，主要的程序逻辑在 Presenter 里实现。
   并且，Presenter 和 View 是没有直接关联的，是通过定义好的接口进行交互，从而使得在变更 View 的时候可以保持 Presenter 不变。

3. MVVM：MVVM 是把 MVC 里的 Controller 和 MVP 里的 Presenter 改成了 ViewModel。Model+View+ViewModel。

   MVVM 的界面与 viewmodel 是松耦合，界面数据从 viewmodel 中获取
   View 的变化会自动更新到 ViewModel,ViewModel 的变化也会自动同步到 View 上显示。
   这种自动同步是因为 ViewModel 中的属性实现了 Observer，当属性变更时都能触发对应的操作。

### 三大框架概述

- vue: 构建用户界面的渐进式、轻量级框架,插件化, 倾向于（html+css+js）,易上手，组件的依赖是在渲染过程中自动追踪的，所以系统能精确知晓哪个组件确实需要被重渲染，开发者不需要考虑组件是否需要重新渲染之类的优化。

  ```js
    //<div id="app">
    //    <!--html中修改的-->
    //  <h1>{{message + '这是在outer HTML中的'}}</h1>
    //</div>
  var vm = new Vue({
  el: '#app',
  template: "<h1>{{message +'这是在template中的'}}</h1>",
  //有template 作为模板编译成render函数，优先级高于外部HTML，页面显示
  render: function(createElement) {
        return createElement('h1', 'this is createElement')
  },
  // 它是以createElement作为参数，然后做渲染操作，而且我们可以直接嵌入JSX
  // render函数选项 > template选项 > outer HTML.
  data: {
    message: 'Vue的生命周期'
  },
  //  this.$el   this.$data(数据双向绑定的表示)   this.message
  beforeCreate: function() {
      // vm实例 创建前状态
      // 初始化事件，进行数据的观测
      // this.$el     undefined
      // this.$data   undefined
      // this.message undefined
  },
  created: function() {
      //创建完毕状态(已进行数据绑定)
      // this.$el     undefined
      // this.$data   [obiect object] 已进行数据绑定
      // this.message Vue的生命周期
  },
  beforeMount: function() {
      // 挂载前状态
      // 首先判断对象是否有el选项。如果有的话就继续向下编译，如果没有el选项，则停止编译，也就意味着停止了生命周期，直到在该vue实例上调用vm.$mount(el)。
      //可以看到此时是给vue实例对象添加$el成员，并且替换掉挂在的DOM元素.此时是虚拟dom的形式
      // this.$el     
      // [object HTMLDivElement]  <div id='app'><h1>{{message}}用此占位</h1></div>
      // this.$data   [obiect object]
      // this.message Vue的生命周期
  },
  mounted: function() {
      vm.message = '触发组件更新'
      // 挂载结束状态
      // this.$el     
      // [object HTMLDivElement]  <div id='app'><h1>Vue的生命周期</h1></div>
      // this.$data   [obiect object]
      // this.message Vue的生命周期
  },
  beforeUpdate: function () {
      // 更新前状态 当vue发现data中的数据发生了改变，会触发对应组件的重新渲染
      // this.$el     [object HTMLDivElement]
      // this.$data   [obiect object]
      // this.message Vue的生命周期(此时view层还未改变，但课监听到data的变化)
  },
  updated: function () {
      // 更新完成状态
      // this.$el     undefined
      // this.$data   [obiect object] 已进行数据绑定
      // this.message 触发组件更新
  },
  beforeDestroy: function () {
      // 销毁前状态
      // 实例销毁之前调用。在这一步，实例仍然完全可用
  },
  destroyed: function () {
      // 销毁完成状态
      // Vue 实例指示的所有东西都会解绑定，所有的事件监听器会被移除，所有的子实例也会被销毁。
  }
  })
  
  ```
  总结： 
  1. **首先创建一个实例，new vue过程中执行init（vue组件默认执行），init过程中调beforeCreate**
  2. **然后在injections（注射）和reactivity（反应性）的时候，调用created，created完成之后，去判断实例里面是否含有“el”选项，若无，调用vm.$mount(el)方法，执行下一步；若有，直接执行下一步。紧接着会判断是否含有“template”这个选项，如果有的话，它会把template解析成一个render function**
  3. **然后在beforeMount中，相关的render函数被调用，$el还只是我们在HTML里面写的节点，**
  4. **之后mounted，就把渲染出来的内容挂载到了DOM节点上**
  5. **当数据变化的时候，调用beforeUpdate，经过Virtual DOM，之后调用updated更新dom，**
  6. **beforeDestroy，实例销毁之前，这个hook实例仍可用**
  7. **destroyed 实例销毁后调用。实例指示的所有东西都会解绑定，所有的事件监听器会被移除，所有的子实例也会被销毁。**

- react(UI 库): 构建用户界面的 Js 库，JSX 语法，无指令，组件化（state,props），某个组件的状态发生变化时，以该组件为根，重新渲染整个组件子树

- react & vue: 组件化，钩子函数（生命周期），使用 Virtual DOM,不内置 ajax，route 核心包，以插件形式加载

- vue & angular(MVVM): 双向数据绑定,支持指令（内置和自定义指令），内置过滤器和自定义过滤器,不支持低端浏览器

![三大框架](/img/interview/react-ag-vue.png "三大框架")
