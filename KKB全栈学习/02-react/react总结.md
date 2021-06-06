## react 更新数组后，组件不会重新渲染？

```js
// react 更新数组后，组件不会重新渲染？
// 因为数组使用的是引用，虽然更新了数组，但是引用没有发生改变
// 所以需要 [...arr] 深拷贝一下数组，然后再通过 set 重新赋值，触发更更新
```

```js
const [result, setResult] = React.useState([]);

// 组件不会重新渲染
result.push(index)
setResult(result)


// 深拷贝后组件重新渲染
result.push(index)
setResult([...result])
```

