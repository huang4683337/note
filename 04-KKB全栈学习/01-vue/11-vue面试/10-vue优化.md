## 1、路由懒加载

```js
const router = new VueRouter({
    routes: [
        { path: '/foo', component: () => import('./Foo.vue') }
    ]
})
```

一开始进行打包时，打包体积会大幅度的减少。

使用的时候按需加载：访问的时候才加载



## 2、keep-alive 缓存

```vue
<template>
    <div id="app">
        <keep-alive>
            <router-view />
        </keep-alive>
    </div>
</template>
```



## 3、v-show 复用 DOM

```vue
<template>
    <div class="cell">
        <!--这种情况用v-show复用DOM，比v-if效果好-->
        <div v-show="value" class="on">
            <Heavy :n="10000" />
        </div>
        <section v-show="!value" class="off">
            <Heavy :n="10000" />
        </section>
    </div>
</template>
```

```
Heavy 存在大量数据的组件
考虑让他占用一定量的内存，避免了 v-if 不断的重建、删除DOM
```



## 4、v-for 遍历避免同时使用 v-if

```vue
<template>
    <ul>
        <li v-for="user in activeUsers" :key="user.id">
            {{ user.name }}
        </li>
    </ul>
</template>

<script>
    export default {
        computed: {
            activeUsers: function () {
                return this.users.filter(function (user) {
                    return user.isActive
                })
            }
        }
    }
</script>
```

```
v-for 里面需要根据某些属性判断是否需要显示。
提前通过 compute 计算出可以显示的内容，然后再通过 v-for 渲染
```



## 5、长列表性能优化（数据不变、数据变化）

**如果列表是纯粹的数据展示，不会有任何改变，就不需要做响应化**

```js
export default {
    data: () => ({
        users: []
    }),
    async created() {
        const users = await axios.get("/api/users");
        this.users = Object.freeze(users);
    }
};
```

```
对于一些不会变的数据，让 vue 不会对他进行响应式操作
赋值时通过 Object.freeze 对数据进行冻结，就不会触发响应式了。
```



**如果是大数据长列表，可采用虚拟滚动，只渲染少部分区域的内容**

```vue
<recycle-scroller class="items" :items="items" :item-size="24">
    <template v-slot="{ item }">
        <FetchItemView :item="item" @vote="voteItem(item)" />
    </template>
</recycle-scroller>
```

> 参考 [vue-virtual-scroller](https://github.com/Akryum/vue-virtual-scroller)、 [vue-virtual-scroll-list](https://github.com/tangbc/vue-virtual-scroll-list)



## 6、事件的销毁

Vue 组件销毁时，会自动解绑它的全部指令及事件监听器，但是仅限于组件本身的事件。

如果引入组件外部的事件（比如定时器），就需要手动销毁。

```js
created() {
    this.timer = setInterval(this.refresh, 2000)
},
beforeDestroy() {
    clearInterval(this.timer)
}
```



## 7、图片懒加载

对于图片过多的页面，为了加速页面加载速度，所以很多时候我们需要将页面内未出现在可视区域内的图片先不做加载， 等到滚动到可视区域后再去加载。

```html
<img v-lazy="/static/img/1.png">
```

> 参考项目: [vue-lazyload](https://github.com/hilongjw/vue-lazyload)



## 8、第三方插件按需引入

像element-ui这样的第三方组件库可以按需引入避免体积太大。

```js
import Vue from 'vue';
import { Button, Select } from 'element-ui';

Vue.use(Button)
Vue.use(Select)
```



## 9、无状态的组件标记为函数式组件

```vue
<template functional>
    <div class="cell">
        <div v-if="props.value" class="on"></div>
        <section v-else class="off"></section>
    </div>
</template>
<script>
    export default {
        props: ['value']
    }
</script>
```

```
无状态组件：组件的数据不会发生改变，标记为函数式组件
使用：<template functional>
```



## 10、子组件分割

```vue
<template>
    <div>
        <ChildComp />
    </div>
</template>
<script>
    export default {
        components: {
            ChildComp: {
                methods: {
                    heavy() { /* 耗时任务 */ }
                },
                render(h) {
                    return h('div', this.heavy())
                }
            }
        }
    }
</script>
```

```
组件中有比较耗时、频繁变动的功能，把这些功能单独拆分成一个组件
让他单独创建一个 Watcher
```



## 11、变量本地化

```vue
<template>
    <div :style="{ opacity: start / 300 }">
        {{ result }}
    </div>
</template>

<script>
    import { heavy } from '@/utils'
    export default {
        props: ['start'],
        computed: {
            base() { return 42 },
            result() {
                const base = this.base // 不要频繁引用this.base
                let result = this.start
                for (let i = 0; i < 1000; i++) {
                    result += heavy(base)
                }
                return result
            }
        }
    }
</script>
```

```
对于不需要频繁计算的数据、或者数据量大且不会频繁变动的数据
可以通过 compute 将数据缓存
```



## 12、SSR

```
服务端渲染（SSR）
 - 提升首屏加载速度
 - 有利于 SEO
```

