---
title: web framework
categories:
  - web framework
tags:
  - web framework
date: 2019-03-01 16:42:20
---

> redux dva vuex

<!-- more -->

# 1. use react with redux and react-router

### Redux → Store &nbsp; Action &nbsp; Reducer

### Store（一个应用只有一个 store，将 action 和 reducer 联系在一起）

```js
// store.js
//  创建store,接受两个参数
import { applyMiddleware, compose, createStore, combineReducers } from "redux";
// 其他的依赖 在实际项目中根据需要自行配置
const store = createStore(createRootReducer(), initialState);
// 创建store,接受两个参数,reducer 是所有reducer结合为一个总reducer
// 创建完成之后 store提供四个方法
// getState： 获取应用当前State。
// subscribe：添加一个变化监听器。
// dispatch：分发 action。修改State。
// replaceReducer：替换 store 当前用来处理 state 的 reducer。

function createRootReducer() {
  return combineReducers({
    ...reducers // 各个模块 页面的 小reducer
  });
}
export const history = syncHistoryWithStore(store);
```

### Action(改变 state 的唯一途径), 通过 store.dispatch(action), 此时 action = {type: 'mark'，payload: {newstate}}

```js
// action.js
const action = data => dispatch => {
  return serverAjax(data).then(res => {
    if (res.result === "success") {
      dispatch({ type: "mark", payload: res });
    } else {
      dispatch({ type: "mark", payload: {} });
    }
  });
};
export const ACTION_HANDLERS = {
  ["mark"]: (state, { payload: res }) => {
    if (res) {
      return {
        ...state,
        data: res.data
      };
    }
  }
};
```

### Reducer(state, action)，接受两个参数，返回新的 state，

```js
// reducer.js
// 这边考虑每个页面 模块都有个初始的initstate，封装一个函数用来创建 reducer，这样就可以分开定义每个页面initstate，写起来清晰
export default function createReducer(initState, handlers) {
  return function reducer(state = initState, action) {
    const handler = handlers[action.type]
    return handler ? handler(state, action) : state
  }
}
import {ACTION_HANDLERS} from 'action.js'
const initialState = {
  data1: [],
  data2: {},
  loading: false
  // ...一个页面的initstate
};

export default createReducer(initialState, ACTION_HANDLERS); // 这边暴露出去 会在react 组件中用到
```

### React-Redux ,redux 独立，需使用 react-redux（提供两个接口，Provider, connect）库来将 react redux 连接起来

1. Provider 是一个 React 组件，它的作用是保存 store 给子组件中的 connect 使用。

```js
import { Provider } from "react-redux";
import { Router } from "react-router";
import store from "store.js";
const MOUNT_NODE = document.getElementById("app");

// 需要定义路由文件

ReactDOM.render(
  <Provider store={store}>
    <Router children={routes} />
  </Provider>,
  MOUNT_NODE
);
```

2. connect(将 state 和 改变 state 的方法（action）转化成 props 传递给组件)

```js
// component.js
import * as actionCreators from 'reducer.js'
import { bindActionCreators } from 'redux'
// bindActionCreators 的作用就是将 Actions 和 dispatch 组合起来生成 mapDispatchToProps 需要生成的内容。
function mapStateToProps(state) {
  return { one: state.one }
}

function mapDispatchToProps(dispatch) {
  return { actions: bindActionCreators(actionCreators, dispatch) }
}

@connect(
    ({ stateone }) => ({ stateone }),
    // 这边的state，会去总的reducer 里面找到对应的 state 返回
    mapDispatchToProps
)
// 两种connect的方法 both ok
export default connect(mapStateToProps, mapDispatchToProps)(Component)
```

### router

```js
export default {
  path: "/",
  tableName: "home",
  component: require("COMPONENT/App").default,
  childRoutes: [
    {
      path: "home",
      getComponent(nextState, callback) {
        require.ensure(
          [],
          require => {
            callback(null, require("CONTAINER/home").default);
          },
          "home"
        );
      }
    },

    // 强制“刷新”页面的 hack
    { path: "redirect", component: require("COMPONENT/Redirect").default },
    // 无路由匹配的情况一定要放到最后，否则会拦截所有路由
    {
      path: "*",
      component: require("CONTAINER/NotFound/index.jsx").default
    }
  ]
};
```

last attach a pic of react-redux

![react-redux](/img/webframe/react-redux.png "react-redux")

来源： https://www.w3ctech.com/topic/1561

2. dva (一个简化 erdux 的库)

> dva 是基于现有应用架构 (redux + react-router + redux-saga 等)的一层轻量封装，没有引入任何新概念，是框架，不是 library，类似 emberjs，会很明确地告诉你每个部件应该怎么写，这对于团队而言，会更可控

dva 中的 model 实际上类似于封装了 redux 里面的 action 和 reducer，并给每个 model 提供一个 namespace 交于 strore 管理。这样，在外部引用的时候，可以直接获取到 model 对应的 namespace，然后根据 namespace 获取数据。

dva-cli 可快速初始化一个项目，基于 Roadhog(一个 cli 工具,提供 server、 build 和 test 命令)，可在.roadhogrc.js 自行配置转发，插件等需要的

```js
// npm i dva-cli -g
// dva new project
// npm i
// npm start

import dva from "dva";
import createLoading from "dva-loading";
import { browserHistory } from "dva/router";

// 1. Initialize
const app = dva({
  onError(e) {
    // message.error(e.message, 3);
  }
  //history: browserHistory,
});

// 2. Plugins
app.use(createLoading({ effects: true }));

// 3. Model
app.model(require("./models/app"));

// 4. Router
app.router(require("./router"));

// 5. Start
app.start("#root");
```
