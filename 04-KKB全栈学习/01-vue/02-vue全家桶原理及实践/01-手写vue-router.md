## Vue Router

Vue Router 是 [Vue.js](https://cn.vuejs.org/) 官⽅的路由管理器。它和 Vue.js 的核⼼深度集成，让构建单⻚⾯应⽤变得易如反掌。

Vue Router 离开 Vue 就无法使用

**安装vue-router**

```shell
$ vue add router
```



## vue-router 使用的核心步骤

+ 使用 vue-router 插件,  router.js

  ```js
  import Router form 'vue-router';
  vue.use(Router);
  ```

  ```
  Vue.use() 会自动执行插件的 install 方法
  ```

+ 创建 Router 实例，router.js

  ```js
  export default new Router({...})
  ```

+ 在根组件上添加该实例，main.js

  ```js
  import router from './router'
  new Vue({
   	router,
  }).$mount("#app");
  ```

  ```
  为什么需要在 new vue 中挂载 router?
  方便 this.$router.push(...) 等操作
  ```

+ 添加路由视图，App.vue

  ```js
  <router-view></router-view>
  ```

+ 导航

  为什么 `<router-view>`、`<router-link>` 可以直接使用

  ```js
  <router-link to="/">Home</router-link>
  <router-link to="/about">About</router-link>
  ```

  



## Vue-router 需求分析

+ 作为⼀个插件存在：实现 VueRouter 类和 install ⽅法，用来 `Vue.use()`

+ 实现两个全局组件：router-view 用于显示匹配组件内容，router-link 用于跳转

+ 监控url变化：监听 hashchange 或 popstate 事件

  实现 SPA 页面不刷新

  + hash - #/about

  + history api - /about

+ 响应最新url：

  创建⼀个响应式的属性 current 代表当前 url 地址，

  当它改变时获取对应组件并 render



## 功能实现

实现一个 VueRouter 类

+ 处理路由选项
+ 监控 url 变化， hashChange
+ 响应 url 变化

实现 install 方法

+ $router 注册

+ 两个全局组件实现

  `router-link`、`router-view`



## 1、创建 Vue-router 插件壳子

```
根目录下新建 src/krouter/kvue-router.js
```

```js
// 保存 Vue 构造函数
let Vue;

class KVueRouter {
    constructor(options) {
        // 路由配置选项
        this.$options = options;
    }
}

// vue.use 自动执行 install 方法
// install 方法参数为 Vue 构造函数
KVueRouter.install = function (_Vue) {
    Vue = _Vue;
}

export default KVueRouter;
```



## 2、实现 vue 类的静态方法 install 

### $router注册：挂载 router 实例到 Vue

**开发过程中我们这样使用**

```js
// 引入 、加载 router
import Router form 'vue-router';
vue.use(Router);

// router 配置
export default new Router({...});

// 挂载 router
import router from './router'
new Vue({
 	router,
}).$mount("#app");
```

```js
在使用时：先通过 Vue.use() 加载 router，然后将 router 挂载到 Vue 实例上

但是在 router 类中，通过 Vue.use() install 时，就需要将 router 实例挂载到 Vue 上。

如何解决这个问题？
通过 mixin 做一个全局混入
在 beforeCreated 钩子中将 Vue 实例混入 router 实例
```



**挂载 router 实例, 让我们在子组件中可以使用**

```
根目录下 src/krouter/kvue-router.js
```

```js
KVueRouter.install = function (_Vue) {
    Vue = _Vue;

    Vue.mixin({
        beforeCreate() {
            // 此时 this 已经指向 Vue 实例了
            // 因为全局混入在任何组件中都会执行
            // 如果 vue 根实例上存在 router
            // 证明 new Vue({ router }) 已经挂载在根实例了
            if (this.$options.router) {
                Vue.prototype.$router = this.$options.router;
            }
        }
    })
}
```



**检测**

```
根目录下新建 src/krouter/index.js
```

```js
import Vue from 'vue'
import KVueRouter from './kvue-router'
import Home from '../views/Home.vue'

Vue.use(KVueRouter)

const routes = [
  {
    path: '/',
    name: 'Home',
    component: Home
  },
  {
    path: '/about',
    name: 'About',
    component: () => import(/* webpackChunkName: "about" */ '../views/About.vue')
  }
]

const router = new KVueRouter({
  mode: 'history',
  base: process.env.BASE_URL,
  routes
})

export default router
```



```
根目录下 src/main.js
```

```js
import router from './krouter'
new Vue({
  router,
  render: h => h(App)
}).$mount('#app')
```



```
根目录下 src/App.vue
```

```vue
<template>
  <div id="app">
    <div id="nav">
      <router-link to="/">Home</router-link> |
      <router-link to="/about">About</router-link>
    </div>
    <router-view/>
  </div>
</template>
```



```
报错：没有注册 <router-link> <router-view>
```



### 实现两个全局组件

**router-link**

```
根目录下src/krouter/kvue-router.js 的 KVueRouter.install下
```

```js
// <router-link to="/about">xxx</router-link>
// <a href="#/about">xxx</a>
Vue.component('router-link', {
    props: {
        to: {
            type: String,
            required: true
        }
    },
    render(h) {
      	// h(tag, props, children)
      	// h(标签, 标签的属性, 子标签)
        return h('a', {
            attrs: {
                href: "#" + this.to
            }
        }, [this.$slots.default]);
      	// jsx 写法 return <a href={'#'+this.to}>{this.$slots.default}</a>;
    }
})
```

```
检测
 - 浏览器不报 没有注册 <router-link> 的错了
 - 出现了我们所期望的 <a>
```



**router-view**

```
根目录下src/krouter/kvue-router.js 的 KVueRouter.install下
```

```js
Vue.component('router-view', {
    render(h) {
        return h(null);	// 暂时不做处理
    }
})
```

```
检测
 - 浏览器不报 没有注册 <router-view> 的错了
```



##  VueRouter 类的实现

### defineReactive 给 router 添加一个响应式属性

```
vue.util.defineReactive()
给一个对象定义一个响应式的属性
```

```
根目录下src/krouter/kvue-router.js
router 类的 constructor 里
```

```js
// 创建响应式数据 current，保存 url 变化
// current 为路由实例响应属性
const initial = window.location.hash.slice(1) || '/';
Vue.util.defineReactive(this, 'current', initial);
```



### 监听 url 变化

```
根目录下src/krouter/kvue-router.js
router 类的 constructor 里
```

```js
// 监控 url 变化, 给响应式数据重新赋值
window.addEventListener('hashchange', this.onHashChange.bind(this));
```



```
根目录下src/krouter/kvue-router.js
router 类里
```

```js
onHashChange() {
    // 响应数据发生改变，就会触发 vue 的重新渲染
    this.current = window.location.hash.slice(1);
}
```



**检测**

```
根目录下src/krouter/kvue-router.js
KVueRouter.install 里
```

````js
Vue.component('router-view', {
    render(h) {
        console.log(this.$router.current);
        return h(null);
    }
})
````

```
- 浏览器改变 url
- 修改 router 类的响应属性
- Vue 重新渲染
```



### url 变化，组件渲染到 `<router-view>`

```
根目录下src/krouter/kvue-router.js
KVueRouter.install 里
```

```js
Vue.component('router-view', {
    render(h) {

        let component = null;

        // url 改变时，从路由配置的映射表里拿到对应的组件
        const route = this.$router.$options.routes.find(
            route => route.path === this.$router.current
        );

        if (route) {
            component = route.component;
        }
        return h(component);
    }
})
```

```
- 因为前面通过 mixin 已经将 router 实例的属性挂载在 vue 实例上了
- routs 为路由表的映射配置
- 所以通过 this.$router.$options.routes 可以获取到所有路由映射表
- 我们可以通过 this.$router.current 获取到当前路由
- 从路由配置表中获取到当前路由对应的组件，然后进行渲染
```



**检测**

```
浏览器切换路由 - 对应内容显示
```



## 优化

前面写的 router-view 组件中，url 每改变一次，就需要遍历一次路由的配置表，这样是不好的，我们接下来对此进行优化。

+ 做一个 url 和 route 对象的映射关系

  ```
  根目录下 src/krouter/kvue-router.js
  constructor 里
  ```

  ```js
  // 缓存 path 和 route 的映射关系
  this.routeMap = {};
  this.$options.routes.forEach(route => {
      this.routeMap[route.path] = route;
  })
  ```

+ url 发生改变，直接渲染对应的 component

  ```js
  Vue.component('router-view', {
      render(h) {
          const { routeMap, current } = this.$router;
          const component = routeMap[current] ? routeMap[current].component : null;
          return h(component);
      }
  })
  ```



## 嵌套路由的处理

