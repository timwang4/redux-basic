#store 的实现

Store 提供了三个方法:

· store.getState()
· store.dispatch()
· store.subscribe()

``` javascript
import { createStore } from 'redux';
let { subscribe, dispatch, getState } = createStore(reducer);
```

createStore方法还可以接受第二个参数，表示 State 的最初状态。这通常是服务器给出的。

``` javascript
let store = createStore(todoApp, window.STATE_FROM_SERVER)
```

上面代码中，window.STATE_FROM_SERVER就是整个应用的状态初始值。注意，如果提供了这个参数，它会覆盖 Reducer 函数的默认初始值。

下面是createStore方法的一个简单实现，可以了解一下 Store 是怎么生成的。

``` javascript
const createStore = (reducer) => {
  let state;
  let listeners = [];

  const getState = () => state;

  const dispatch = (action) => {
    state = reducer(state, action);
    listeners.forEach(listener => listener());
  };

  const subscribe = (listener) => {
    listeners.push(listener);
    return () => {
      listeners = listeners.filter(l => l !== listener);
    }
  };

  dispatch({});

  return { getState, dispatch, subscribe };
};
```