mutations中的 state是从哪里来的？

action中的 commit、 dispatch 是从哪里来的？

> vue源码中默认 `$$xx`  是不会被定义到 VM 实例上的
>
> data 中 $$xx 是不会被代理到 vm实例上的

```js
data(){
	return{
    $$store: xxxx // $$store不会被代理到 vm实例上的
  }
}
```

octotree = 》 可以在github上看到目录树



## 注意：在 vue 中，访问某个属性时（obj.xx）, 想要触发某些函数之类的，首先就要想到通过 Object.defineProperty 给当 obj 添加 get 描述

 

index.js

```js
import Vue from 'vue'
import Vuex from './kvuex'

Vue.use(Vuex)

export default new Vuex.Store({
  state: {
    counter: 0
  },
  mutations: {
    // state哪来的？
    add(state) {
      state.counter++
    }
  },
  actions: {
    add({commit}) {
      setTimeout(() => {
        commit('add')
      }, 1000);
    }
  },
  getters: {
    doubleCounter(state) {
      return state.counter * 2
    }
  }
})
```

kvuex.js

```js
// 1.实现一个Vuex插件
// 2.实现一个Store类
// 2.1.响应式的数据state
// 2.2.commit()实现
// 2.3.dispatch()实现
// 3.挂载到Vue.prototype
let Vue
class Store {
  constructor(options) {
    // 保存mutations
    this._mutations = options.mutations

    // 保存 actions
    this._actions = options.actions

    // 保存 getters
    this._warpedGetters = options.getters

    // 定义 computed 选项, 用于计算 getter
    const computed = {}

    // 定义 getters, 用于 this.$store.getter.xx
    this.getters = {}

    // 保存 this, 在 forEach 遍历时使用
    const store = this
    Object.keys(this._warpedGetters).forEach(key => {

      // getter 中定义的方法 => doubleCounter(state) { return state.counter * 2 }
      const fn = store._warpedGetters[key]

      // 转换为 computed 可以使用的无参数形式
      /**
       * computed:{
       *   doubleCounter:function(){ return this.xxx }
       * }
       */
      computed[key] = function () {
        return fn(store.state)
      }

      // 为 getter 添加只读属性
      Object.defineProperty(store.getters, key,{
        get: ()=> store._vm[key]
      })

    })


    // 响应式的数据state
    this._vm = new Vue({
      data: {
        $$state: options.state
      }
    })

    this.commit = this.commit.bind(this)
    this.dispatch = this.dispatch.bind(this)

    // getters
    // this.getters = {}
    // 遍历getters配置，动态设置计算属性，它们的值是getters的函数计算结果
    // 响应式的属性怎么设置？ 

    // 可以利用Vue computed特性
  }

  get state() {
    return this._vm._data.$$state
  }
  set state(v) {
    console.error('please use replaceState to reset state');
  }

  // 实现commit
  // store.commit(type, payload)
  commit(type, payload) {
    const entry = this._mutations[type]

    if (!entry) {
      console.error('unknown mutation type');
      return
    }
    // 指定一下上下文
    entry(this.state, payload)
  }

  dispatch(type, payload) {
    const entry = this._actions[type]

    if (!entry) {
      console.error('unknown action type');
      return
    }
    // 指定一下上下文即Store实例
    return entry(this, payload)
  }
}

function install(_Vue) {
  Vue = _Vue

  Vue.mixin({
    beforeCreate() {
      if (this.$options.store) {
        Vue.prototype.$store = this.$options.store
      }
    }
  })
}

// 默认导出就是Vuex
export default { Store, install }
```

main.js

```js
import store from './kstore'
new Vue({
  router, // 此处挂上VueRouter实例，this.$router.push(...)
  store,
  render: h => h(App)
}).$mount('#app')
```

