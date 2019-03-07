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

### Redux → Store &nbsp; Action &nbsp; Reducer (下面分开介绍，并附上主代码)

> 文章开头先概括 redux, 创建 store(连接 reducer&action)数据会存放在 store，会分发成各个小 state, reducer 也会可分发，页面开始渲染初始 state，当用户操作需要改变数据的时候，只能通过 dispatch 发送 action 途径改变，之后触发 reducer 纯函数，接受 state，action，返回新的 state 数据，render 页面，完成页面 update

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
  tableName: "app",
  component: require("COMPONENT/App").default,
  childRoutes: [
    {
      path: "app",
      getComponent(nextState, callback) {
        require.ensure(
          [],
          require => {
            callback(null, require("CONTAINER/app").default);
          },
          "app"
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

...react-redux is done

来源： https://www.w3ctech.com/topic/1561

# 2. dva (一个简化 redux 的库)

> dva 是基于现有应用架构 (redux + react-router + redux-saga 等)的一层轻量封装，没有引入任何新概念，是框架，不是 library，类似 emberjs，会很明确地告诉你每个部件应该怎么写，这对于团队而言，会更可控

dva 中的 model 实际上类似于封装了 redux 里面的 action 和 reducer，并给每个 model 提供一个 namespace 交于 strore 管理。这样，在外部引用的时候，可以直接获取到 model 对应的 namespace，然后根据 namespace 获取数据。

dva-cli 可快速初始化一个项目，基于 Roadhog(一个 cli 工具,提供 server、 build 和 test 命令)，可在.roadhogrc.js 自行配置转发，插件等需要的。

create 一个 app 的步骤

      npm i dva-cli -g
      dva new projectname
      npm i
      npm start

### 这里分享我的配置文件

```js
// .roadhog.js
// 代理目标地址
const _target = "https://a.b.com";
const path = require("path");
export default {
  entry: "src/index.js",
  disableCSSModules: false,
  autoprefixer: null,
  theme: {
    "@primary-color": "#...",
    "@link-color": "#...",
    "@border-radius-base": "2px",
    "@font-size-base": "13px",
    "@animation-duration-slow": ".2s",
    "@animation-duration-base": ".2s"
  },
  proxy: {
    "/api": {
      target: _target,
      changeOrigin: true
    }
  },
  extraBabelPlugins: [
    "transform-runtime",
    ["import", { libraryName: "antd", style: true }],
    [
      "module-resolver",
      {
        root: ["./src"],
        alias: {
          components: path.resolve(__dirname, "src/components/"),
          config: path.resolve(__dirname, "src/config/"),
          models: path.resolve(__dirname, "src/models/"),
          routes: path.resolve(__dirname, "src/routes/"),
          services: path.resolve(__dirname, "src/services"),
          constants: path.resolve(__dirname, "src/constants"),
          assets: path.resolve(__dirname, "src/assets"),
          utils: path.resolve(__dirname, "src/utils"),
          nodeModule: path.resolve(__dirname, "node_modules")
        }
      }
    ]
  ],
  env: {
    development: {
      extraBabelPlugins: ["dva-hmr"]
    }
  },
  publicPath: "/public×××/"
};
```

### dva 初始化部分

```js
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
// loading使用 loading.effects['user/query']
// 3. Model
app.model(require("./models/app"));
// 4. Router
app.router(require("./router"));
// 5. Start
app.start("#root");
```

### 这里最主要是 model，将 reducer action 合并，写起来更加方便，model 包括以下

namespace 命名空间，每个模块的唯一标志

state 各个模块的数据存放地

subscriptions 监听数据 在 app.start() 是执行 一般页面里面初始数据 也可写在 componentDidMount hook

effects 异步操作 接收数据 改变数据的唯一途径

reducers 处理数据，唯一可以修改 state 的地方，由 action 触发

```js
// 一个model 例子
import { hashHistory, routerRedux } from "dva/router";
export default {
  namespace: "app",
  state: {
    data: [],
    obj: {}
  },
  subscriptions: {
    setup({ dispatch, history }) {
      history.listen(({ pathname, query }) => {
        dispatch({ type: "query", payload: {} });
      });
    }
  },
  effects: {
    *query({ payload }, { select, put, call }) {
      // 异步请求
      const res = yield call("接口", payload);
      // put  用来发起一条action
      // call 以异步的方式调用函数
      // select 从state中获取相关的数据
      // take 获取发送的数据
      // 可实现跳转路由
      yield put(routerRedux.push("/other"));
      yield put({
        type: "querySuccess",
        payload: res
      });
    }
  },
  reducers: {
    querySuccess(state, { payload }) {
      return {
        ...state,
        data: res.data
      };
    }
  }
};
```

### router 文件 和 component 文件

```js
import React from "react";
import { connect } from "dva";
function App({ location, dispatch, app, params }) {
  // app  model里面 ，params 当路由带参数的时候，是参数的obj
  const config = {
    ...app,
    location,
    // es6写法， 对象的方法
    query(params) {
      dispatch({
        type: "app/query",
        payload: params
      });
    }
  };
  return (
    <div>
      <Components {...config} />
      {/*来自类组件  有状态和生命周期*/}
    </div>
  );
}

function mapStateToProps({ app }) {
  return { app };
}
export default connect(mapStateToProps)(App);
```

...dva is done

# 3. vuex (专为 Vue 应用程序开发的状态管理模式)

> 我一般理解的时候和 redux 进行对比，找到彼此的相似点，方便理解与记忆

首先 vuex 的核心是 store 了，包含着应用的 state，并且状态存储是响应式的，和 redux 一样，不能直接改变 state，vuex 中是通过显式提交（commit）mutation改变state，可理解 redux 通过 dispath 发送 action，触发 reducer 来改变 state，个人感觉 vuex 其实和 dva 更相似，封装在一个 model 里面，书写起来更方便

下来介绍 vuex 的使用过程，并附上住代码，可参考 github 一个简易的 vuex 项目 https://github.com/heqingha/vue-cli

### 1） 初始化一个 vue 实例，注册 store，store 实例会注入到根组件下的所有子组件中，且子组件能通过 this.$store 访问到

```js
// main.js
import Vue from "vue";
import App from "./App";
import router from "./router";
import axios from "axios";
import VueAxios from "vue-axios";
// 使用ui组件ElementUI
import ElementUI from "element-ui";
import "element-ui/lib/theme-chalk/index.css";
import store from "store";
Vue.use(VueAxios, axios);
new Vue({
  el: "#app",
  router,
  store,
  components: { App },
  template: "<App/>"
  // 这边证明render 函数的优先级高于 template
  // render: function(createElement) {
  //   return createElement('h1', '66666')
  // }
});
```

### 2）创建 store，Vuex 允许我们将 store 分割成模块（module），每个模块拥有自己的 state、mutation、action、getter、甚至是嵌套子模块，访问方式 store.state.a,store.state.b

```js
// store.js
// 首先创建一个store
import vue from "vue";
import vuex from "vuex";
import createLogger from "vuex/dist/logger"; // 修改日志
const debug = "productions"; // 开发环境中为true，否则为false
export default new vuex.Store({
  state, //跟级别的 state
  getters, //跟级别的 getter
  mutations, //根级别的mutations名称（官方推荐mutions方法名使用大写）
  actions, //根级别的 action
  // 从其他地方import进来，将每个页面的store分开成不同的module，便于状态管理
  modules: {
    a,
    b
  },
  plugins: debug ? [createLogger()] : [] // 开发环境下显示vuex的状态修改
});
```

### 3) 创建每个模块的 module

```javascript
// a.js
export default {
  state: {
    a: {}
  },
  actions: {
    a(context, { payload }) {
      // context.state 是模块的局部状态对象
      //   context.commit("loading", true);
      // 这边对ajax 做了个封装，详情可看 github具体代码
      a.a(payload).then(res => {
        if (res.result === "success") {
          context.commit("a", res.data);
        } else {
          context.commit("a", null);
        }
      });
    }
  },
  // store 的计算属性，就像计算属性一样，getter 的返回值会根据它的依赖被缓存起来，且只有当它的依赖值发生了改变才会被重新计算。
  // 访问getter this.$store.getters.gettersss,
  // 也可以接受其他 getter 作为第二个参数
  // 也可以通过让 getter 返回一个函数,实现给 getter 传参
  // getter 在通过方法访问时，每次都会去进行调用，而不会缓存结果
  getters: {
    // 对于模块内部的 getter，根节点状态会作为第三个参数暴露出来：
    gettera (state, getters, rootState) {
      return state.a + rootState.a
    },
    getterb: (state, getters) => {
    return getters.doneTodos.length
    },
    getterc: (state) => (id) => {
    return state.todos.find(todo => todo.id === id)
  }
  }
  mutations: {
    loading(state, payload) {
      //   state.loading = true;
    },
    a(state, payload) {
      state.a = payload;
      // 这边和react写法有点不同，react return {...}
    }
  }
};
```

### 4) .vue 组件

```js
import { mapGetters, mapMutations, mapActions, mapState } from "vuex";
export default {
  data() {
    return {};
  },
  methods: {
    ...mapGetters({
      // 把 `this.doneCount` 映射为 `this.$store.getters.doneTodosCount`
      doneCount: "doneTodosCount"
    }),
    ...mapMutations([
        // 将 `this.a()` 映射为 `this.$store.commit('a')`,适用于同步操作，不涉及接口
        "a"
      ]),
      ...mapActions([
        'b', // 将 `this.b()` 映射为 `this.$store.dispatch('b')`
      ]),
  },
};
```

总结总是记不住的主要代码

```js
this.$store.dispatch{type: '', payload}      触发action 异步
context.commit("a", payload)                 提交 mutation 触发
this.a({type: '', payload})                  映射之后的调用方式
this.$store.commit(type: '', payload)        若是同步操作，直接调用commit
```
vuex is done