## 事件总线

任意两个组件之间传值常⽤事件总线 或 vuex的⽅式。


```js
// Bus：事件派发、监听和回调管理
class Bus {
 constructor(){
   this.callbacks = {}
 }
 $on(name, fn){
   this.callbacks[name] = this.callbacks[name] || []
   this.callbacks[name].push(fn)
 }
 $emit(name, args){
   if(this.callbacks[name]){
     this.callbacks[name].forEach(cb => cb(args))
   }
 }
}
// main.js
Vue.prototype.$bus = new Bus()
```

```js
// child1
this.$bus.$on('foo', handle)
// child2
this.$bus.$emit('foo')
```




> 实践中通常⽤`Vue`代替`Bus`，因为`Vue`已经实现了`$on`和`$emit`

