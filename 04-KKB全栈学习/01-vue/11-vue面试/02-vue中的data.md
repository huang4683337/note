## 为什么组件中 data必须是函数？

## 为什么实例中 data 可以不是函数？

```
源码地址 src\core\instance\state.js - initData()
```

```js
function initData (vm: Component) {
  let data = vm.$options.data
  // 判断选项类型
  data = vm._data = typeof data === 'function'
    ? getData(data, vm)
    : data || {}
  if (!isPlainObject(data)) {
    data = {}
    process.env.NODE_ENV !== 'production' && warn(
      'data functions should return an object:\n' +
      'https://vuejs.org/v2/guide/components.html#data-Must-Be-a-Function',
      vm
    )
  }
  ......
}
```





## 测试代码

```html
<!DOCTYPE html>
<html>

<head>
    <title>Vue事件处理</title>
</head>

<body>
    <div id="demo">
        <h1>vue组件data为什么必须是个函数? </h1>
        <comp></comp>
        <comp></comp>
      	<p>{{counter}}</p>
    </div>
    <script src="../../dist/vue.js"></script>
    <script>
        Vue.component('comp', {
            template: '<div @click="counter++">{{counter}}</div>',
            data(){
              return { counter: 0 }
            }
        })
        // 创建实例
        const app = new Vue({
            el: '#demo',
          	data: { counter: 0 }
        });
    </script>
</body>

</html>
```



## 总结

```
1、Vue.component() 方法声明组件时只会执行一次

2、如果一个组件多次调用并且 data 是一个对象
那么某次调用组件的状态变更就会影响当前组件的所有实例。

3、data 使用函数，会在 initData 时将 data 作为一个工厂函数返回全新的 data 对象，可以有效的规避实例之间的污染问题

4、因为根实例只有一个，所以不需要担心这个问题

5、源码中会有一个检测，如果是根实例，就不会检测。
如果时组件，就是一个函数，就无法绕过检测，data 必须是函数
```



```
vue2_org/vue-study-web22-source/src/core/util/options.js
126 行断点查看源码
```



