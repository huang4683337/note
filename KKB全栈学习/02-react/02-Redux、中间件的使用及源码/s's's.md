plan Object 是什么？



## combineReducers 原理

[combineReducers 原理](https://sevencai.github.io/2016/10/25/Redux%20%E4%B8%AD%20combineReducers%20%E5%92%8C%20createStore%E7%9A%84%E5%AE%9E%E7%8E%B0%E5%8E%9F%E7%90%86/)

[combineReducers 原理](https://segmentfault.com/a/1190000011555502)

```js
// 暗号：坦桑尼亚
export default function combineReducers(reducers) {
  return function combination(state = {}, action) {
    let nextState = {};
    let hasChanged = false;
    for (let key in reducers) {
      const reducer = reducers[key];
      nextState[key] = reducer(state[key], action);
      hasChanged = hasChanged || state[key] !== nextState[key];
    }

    hasChanged =
      hasChanged || Object.keys(nextState).length !== Object.keys(state).length;
    return hasChanged ? nextState : state;
  };
}
```

