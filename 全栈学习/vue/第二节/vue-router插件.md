vue-router 和 vue.js 是高度耦合的

为什么 <router-view>、 <router-link> 这两个组件可以不用我们手动注册可以直接使用





 路由： 通过数据响应监听地址的变化，根据hash地址去 new router的路由表中去匹配组件，将对应的组件在 <router-view>中使用render函数直接渲染



vue.component() 创造组件方法

render函数相关



Vue.util.defineReactive 可以给以一个obj定义一个响应式的属性





## 思考

多级路由的如何拆分



## 需求分析

```js
import VueRouter from 'vue-router'
Vue.use(VueRouter)
const routes = [
  {
    path: '/',
    name: 'Home',
    component: Home
  }
]

const router = new VueRouter({
  routes
})

export default router
```

+ 首先需要实现一个 `vue-router` 类
+ 实现 `vue.use()`
+ 建立路由和组件的映射关系，根据路由显示相应的组件
+ 实现 `<router-view>`、`<router-link>` 两个内置组件

