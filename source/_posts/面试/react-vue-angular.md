---
title: 前端框架
categories:
  - 前端技术
tags:
  - 前端框架
date: 2019-02-24 18:51:20
---

> 前端三大框架概念区别简总（angular,react,vue）

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
    el: "#app",
    template: "<h1>{{message +'这是在template中的'}}</h1>",
    //有template 作为模板编译成render函数，优先级高于外部HTML，页面显示
    render: function(createElement) {
      return createElement("h1", "this is createElement");
    },
    // 它是以createElement作为参数，然后做渲染操作，而且我们可以直接嵌入JSX
    // render函数选项 > template选项 > outer HTML.
    data: {
      message: "Vue的生命周期"
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
      vm.message = "触发组件更新";
      // 挂载结束状态
      // this.$el
      // [object HTMLDivElement]  <div id='app'><h1>Vue的生命周期</h1></div>
      // this.$data   [obiect object]
      // this.message Vue的生命周期
    },
    beforeUpdate: function() {
      // 更新前状态 当vue发现data中的数据发生了改变，会触发对应组件的重新渲染
      // this.$el     [object HTMLDivElement]
      // this.$data   [obiect object]
      // this.message Vue的生命周期(此时view层还未改变，但课监听到data的变化)
    },
    updated: function() {
      // 更新完成状态
      // this.$el     undefined
      // this.$data   [obiect object] 已进行数据绑定
      // this.message 触发组件更新
    },
    beforeDestroy: function() {
      // 销毁前状态
      // 实例销毁之前调用。在这一步，实例仍然完全可用
    },
    destroyed: function() {
      // 销毁完成状态
      // Vue 实例指示的所有东西都会解绑定，所有的事件监听器会被移除，所有的子实例也会被销毁。
    }
  });
  ```

  总结：

  1. **首先创建一个实例，new vue 过程中执行 init（vue 组件默认执行），init 过程中调用 beforeCreate**
  2. **后在 injections（注射）和 reactivity（反应性）的时候，调用 created，created 完成之后，此时已经完成了双向数据绑定**
  3. **beforeMount 中，去判断实例里面是否含有“el”选项，若无，调用 vm.$mount(el)方法，若有，去判断是否有“template”，若有，它会把template解析成一个render function，(其实这个hook function中就是相关的render函数被调用，此时$el 还只是我们在 HTML 里面写的节点)，**
     `render函数选项 > template选项 > outer HTML.（render函数优先级）`
  4. **之后 mounted，就把渲染出来的内容挂载到了 DOM 节点上**
  5. **当数据变化的时候，调用 beforeUpdate，经过 Virtual DOM，之后调用 updated 更新 dom，**
  6. **beforeDestroy，实例销毁之前，这个 hook 实例仍可用**
  7. **destroyed 实例销毁后调用。实例指示的所有东西都会解绑定，所有的事件监听器会被移除，所有的子实例也会被销毁。**

- react(UI 库): 构建用户界面的 Js 库，JSX 语法，无指令，组件化（state,props），某个组件的状态发生变化时，以该组件为根，重新渲染整个组件子树

```js
import React, { Component } from "react";
class Test extends Component {
  // 类的构造方法( constructor() )，继承了react Component这个基类，也就继承这个react的基类，才能有render(),生命周期等方法可以使用，这也说明为什么函数组件不能使用这些方法的原因。
  constructor(props) {
    //   super(props)用来调用基类的构造方法( constructor() ), 也将父组件的props注入给子组件
    super(props);
  }
  componentWillMount() {
    // 在组件挂载到DOM前调用，且只会被调用一次，在这边调用this.setState不会引起组件重新渲染，也可以把写在这边的内容提前到constructor()中，所以项目中很少用。
  }

  componentDidMount() {
    //   组件挂载到DOM后调用，且只会被调用一次
  }
  //   父组件重新render引起子组件重新render的情况有两种，
  //   1.  每当父组件重新render导致的重传props，子组件将直接跟着重新渲染，无论props是否有变化。可通过shouldComponentUpdate方法优化。
  // 2. 在componentWillReceiveProps方法中，将props转换成自己的state
  componentWillReceiveProps(nextProps) {
    //   此方法只调用于props引起的组件更新过程中，参数nextProps是父组件传给当前组件的新props。但父组件render方法的调用不能保证重传给当前组件的props是有变化的，所以在此方法中根据nextProps和this.props来查明重传的props是否改变，以及如果改变了要执行啥，比如根据新的props调用this.setState出发当前组件的重新render
  }

  shouldComponentUpdate(nextProps, nextState) {
    // 应该使用这个方法，否则无论props是否有变化都将会导致组件跟着重新渲染
    // 此方法通过比较nextProps，nextState及当前组件的this.props，this.state，返回true时当前组件将继续执行更新过程，返回false则当前组件更新停止，以此可用来减少组件的不必要渲染，优化组件性能。
    if (nextProps.someThings === this.props.someThings) {
      return false;
    }
  }
  componentWillUpdate(nextProps, nextState) {
    //   此方法在调用render方法前执行，在这边可执行一些组件更新发生前的工作，一般较少用。
  }
  componentDidUpdate(prevProps, prevState) {
    //   此方法在组件更新后被调用，可以操作组件更新的DOM，prevProps和prevState这两个参数指的是组件更新前的props和state
  }
  render() {
    //   根据组件的props和state，return 一个React元素，不负责组件实际渲染工作，之后由React自身根据此元素去渲染出页面DOM。render是纯函数，不能在里面执行this.setState，会有改变组件状态的副作用。
    // 反复调用
  }
  componentWillUnmount() {
    //   此方法在组件被卸载前调用，可以在这里执行一些清理工作，比如清楚组件中使用的定时器，清楚componentDidMount中手动创建的DOM元素等，以避免引起内存泄漏。
  }
}
```

只执行一次： constructor、componentWillMount、componentDidMount

执行多次：render 、子组件的 componentWillReceiveProps、componentWillUpdate、componentDidUpdate

有条件的执行：componentWillUnmount（页面离开，组件销毁时）

不执行的：根组件（ReactDOM.render 在 DOM 上的组件）的 componentWillReceiveProps（因为压根没有父组件给传递 props）

- react & vue: 组件化，钩子函数（生命周期），使用 Virtual DOM,不内置 ajax，route 核心包，以插件形式加载

- vue & angular(MVVM): 双向数据绑定,支持指令（内置和自定义指令），内置过滤器和自定义过滤器,不支持低端浏览器

![三大框架](/img/interview/react-ag-vue.png "三大框架")
