

```js
const obj = {
  a:{
   	b:1 
  }
}

进行一个代理 object.definprototy 

obj.a = {ccc:22}
obj.a.ccc 是不会触发 代理的 set ，因为就没有对 ccc 进行代理
所以在每次对属性赋值对象时 都要在set 中重新进行一次代理

```

## object.definprototy  是不会对数组进行拦截的，所以数组需要额外处理