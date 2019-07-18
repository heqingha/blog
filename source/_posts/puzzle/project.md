---
title: puzzle in the project
categories:
  - puzzle
tags:
  - puzzle
date: 2019-03-06 15:54:12
---

> clear up puzzle that met in the project and find a solution about them.

<!-- more -->

### step  

> **describe（react）**：页面新增、修改、复制新增是同一个路由，同一个组件，仅仅根据query来区分，导致的问题，页面之间切换的时候，componentDidMount 只触发一次，在之中根据不同的query做的一系列判断加载fetch，不能实现
> **solution**: 每个页面加不同的路由，组件共用，根据 hook componentWillReceiveProps，当props 变化是，判断不同的pathname，处理原本要在componentDidMount执行的

> **describe（react）**：页面二次进入，redux设置的state 改变之后不会在回到初始状态，导致上次操作的数据或者按钮状态会有问题
> **solution**: 在componentdidmount里面做初始化状态操作，如果是掉了接口的操作可以不用初始化