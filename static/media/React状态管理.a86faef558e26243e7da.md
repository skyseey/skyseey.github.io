# React状态管理

`redux三步走⭐⭐⭐`

## 1.Redux

> 版本：4.x-5.x

- 第一步 新建 ./store/index.js

  ```javascript
  import { createStore, combineReducers } from "redux";
  import { counterReducer, titaleReducer } from "./reducers"; // 假设你有一个名为reducers.js的文件
  
  // combineReducers收集组合reducer处理器
  const rootReducer = combineReducers({
    counter: counterReducer,
    // 如果有多个reducer，可以继续添加
  });
  
  const store = createStore(rootReducer);
  
  export default store;第一步 新建 ./store/index.js
  ```

- 第二部 新建 reducer  ./store/reducer.js

  ```javascript
  export const counterReducer = (
    prevState = {
      counter: 0, // 初始化state
    },
    actions
  ) => {
    let newState = { ...prevState }; // 浅拷贝一份 且必须返回一个新的object
    switch (actions.type) {
      case "+":
        newState.counter++;
        return newState;
      case "-":
        newState.counter--;
        return newState;
      default:
        return newState;
    }
  };
  ```

- 第三步 导入组件开始使用 

  ```javascript
  import store from "./store";
  import "./index.css";
  import { useState } from "react";
  
  const A = (props) => {
    const [couter, setCouter] = useState(store.getState().counter.counter);
  
    const handle = (type) => {
      store.dispatch({
        type,
      });
    };
  
    // 监听
    store.subscribe(() => {
      setCouter(store.getState().counter.counter);
    });
  
    return (
      <div className="dis">
        <button
          onClick={() => {
            handle("+");
          }}
        >
          +
        </button>
        <span>{couter}</span>
        <button
          onClick={() => {
            handle("-");
          }}
        >
          -
        </button>
      </div>
    );
  };
  
  export default A;
  
  ```

  

## 2.React-Redux

> 版本：4.x-6.x

- 第一步 和 第二部与redux一样 直接复用即可

- 第三步 使用 根组件注册（也是在最外层套壳 原理也就是向下传递具体参考其它文档）

  ```react
  import { Provider } from "react-redux";
  import store from "./store/index.js";
  import A from "./A";
  import B from "./B";
  const ReactRedux = () => {
    return (
      <Provider store={store}>
        <h1>ReactRedux</h1>
          <A />
          <B />
      </Provider>
    );
  };
  
  export default ReactRedux;
  
  ```

  

- 第四步 使用

  ```react
  import "./index.css";
  import { connect } from "react-redux";
  
  const A = (props) => {
    return (
      <div className="dis">
        <span>开头：{props.title}</span>
        <button
          onClick={() => {
            props.change("+");
          }}
        >
          +
        </button>
        <span>{props.counter.counter}</span>
        <button
          onClick={() => {
            props.change("-");
          }}
        >
          -
        </button>
      </div>
    );
  };
  
  // connect通过链式调用返回高阶组件HOC
  export default connect(
    (state) => {
      return {
        counter: state.counter,
      };
    },
    {
      change: (type) => {
        return {
          type,
        };
      },
    }
  )(A);
  
  ```

  

## 3.redux-persist

>  版本：4.x-6.x

- 第一步 持久化

  ```javascript
  import { createStore, combineReducers } from "redux";
  import { counterReducer, titaleReducer } from "./reducers"; // 假设你有一个名为reducers.js的文件
  
  // 导入持久化组件
  import { persistStore, persistReducer } from "redux-persist";
  import storage from "redux-persist/lib/storage";
  
  const persistConfig = {
    key: "storge",
    storage,
    // whitelist: ["counter"], //持久白名单 存在及缓存
  };
  
  const rootReducer = combineReducers({
    counter: counterReducer,
    title: titaleReducer,
    // 如果有多个reducer，可以继续添加
  });
  
  const persistReducers = persistReducer(persistConfig, rootReducer);
  
  const store = createStore(persistReducers);
  
  const persistStores = persistStore(store);
  
  export { store, persistStores };
  
  ```

  

- 第二步 根组件改造

  ```react
  import { Provider } from "react-redux";
  import { store, persistStores } from "./store/index.js";
  import { PersistGate } from "redux-persist/integration/react";
  import A from "./A";
  import B from "./B";
  const ReactRedux = () => {
    return (
      <Provider store={store}>
        <PersistGate loading={null} persistor={persistStores}>
          <h1>ReactRedux</h1>
          <A />
          <B />
        </PersistGate>
      </Provider>
    );
  };
  
  export default ReactRedux;
  
  ```

  

## 总结

