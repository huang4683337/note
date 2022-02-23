## Composition API

[Composition API](https://v3.vuejs.org/guide/composition-api-introduction.html)字⾯意思是组合API，它是为了实现基于函数的逻辑复⽤机制⽽产⽣的。



## 测试文件路径

```
源码中：根目录/packages/vue/examples/源码调试/00-测试.html
源码的打包结果：根目录/packages/vue/dist/vue.global.js
```





## setup

```html
<div id="app">
    <h1 @click="onclick">{{state.message}}</h1>
    <p>{{state.counter}}</p>
    <p>{{state.doubleCounter}}</p>
</div>

<script src="../../dist/vue.global.js"></script>


<script>
    // 用到什么引入什么
    // 没引入的打包时，会被摇树优化掉
    const { createApp, reactive，computed } = Vue;

    // 组合 API
    const app = createApp({
        // data、methods 等都可以写在里面
        // 消除 this、消除动态推断
        setup() {
            // 创建响应式数据
            const state = reactive({
                message: 'hello vue3',
                counter: 1,
                doubleCounter: computed(() => state.counter * 2)
            });

            function onclick() {
                console.log('click me');
                state.message = 'vue3 hello'
            }

            setInterval(() => {
                state.counter++;
            }, 1000)

            // 返回上下文
            return { state, onclick }
        }
    })

    // vue.component 在打包时
    // 没有引用的组件无法摇树优化
    // 声明一个组件
    app.component('comp', {
        template: '<div>comp</div>'
    })

    app.mount('#app');
</script>
```



## 生命周期

可以在代码的任意位置写, 可以任意写多个

```html
<div id="app">
    <h1 @click="onclick">{{state.message}}</h1>
</div>

<script src="../../dist/vue.global.js"></script>


<script>

    const { createApp, reactive, onMounted } = Vue;

    const app = createApp({

        setup() {
            // 创建响应式数据
            const state = reactive({
                message: 1,
            });

            function onclick() {
                state.message++;
            }

            onMounted(() => {
                state.message = 10;
            })

            // 返回上下文
            return { state, onclick }
        }
    })


    app.component('comp', {
        template: '<div>comp</div>'
    })

    app.mount('#app');
</script>
```





## watch

```html
<div id="app">
    <h1 @click="onclick">{{state.message}}</h1>
</div>

<script src="../../dist/vue.global.js"></script>


<script>

    const { createApp, reactive, onMounted, watch } = Vue;

    const app = createApp({

        setup() {
            // 创建响应式数据
            const state = reactive({
                message: 1,
            });

            function onclick() {
                state.message++;
            }

            onMounted(() => {
                state.message = 10;
            })

            watch(() => state.message, (val, oldV) => {
                console.log('message: ' + val);
            })

            // 返回上下文
            return { state, onclick }
        }
    })


    app.component('comp', {
        template: '<div>comp</div>'
    })

    app.mount('#app');
</script>
```



## computed

```html
<template>
    <div>
        {{ resCount}}
        <el-input
            v-model="count"
            placeholder="Please input"
        />
    </div>
</template>

<script>
import { defineComponent, reactive, toRefs, computed } from 'vue';

export default defineComponent({
    setup() {
        const state = reactive({
            count: 0
        });
        const resCount = computed(() => +state.count + 2);
        return {
            ...toRefs(state),
            resCount
        };
    }
});
</script>
```





## 单值响应式 ref

**使用方式**

```
1 - 通过 ref 包装一个单值响应式，const v = ref(值|空);
2 - 通过 xx.value 获取定义的值，v.value = '';
3 - 在 setup 中 return 出去
2 - html 中写时不用 state.xx, 直接 xxx
```



**具有功能**

```
1 - js 中定义一个单值相应式
2 - 通过 ref 获取 DOM
	  <a ref='aRef'>
	  const aRef = ref(null);
	  aref.xx(); // 调用 DOM 方法
	  return { aRef }
```



```html
<div id="app">
    <p @click="clickFn">{{counter}}</p>
</div>

<script src="../../dist/vue.global.js"></script>


<script>

    const { createApp, ref } = Vue;

    const app = createApp({

        setup() {

            // 单值响应式: 通过 ref 包装
            // 交互需要使用 ref.value
            const counter = ref("hell, vue3");
            function clickFn() {
                console.log('counter: ' + counter.value);
                counter.value += 1;
            }
            // 返回上下文
            return { counter, clickFn }
        }
    })


    app.component('comp', {
        template: '<div>comp</div>'
    })

    app.mount('#app');
</script>
```



## reactive 定义的值在DOM中不想要外部包装

```
- <p>{{state.counter}}</p>
- ...toRefs(state) 转化
- <p>{{counter}}</p>
```



```html
<div id="app">
    <p @click="clickFn">{{counter}}</p>
</div>

<script src="../../dist/vue.global.js"></script>


<script>

    const { createApp, reactive, toRefs } = Vue;

    const app = createApp({

        setup() {
            const state = reactive({
                counter: 1
            })

            function clickFn() {
                console.log('counter: ' + state.counter);
                state.counter += 1;
            }
            // 返回上下文
            return { ...toRefs(state), clickFn }
        }
    })


    app.component('comp', {
        template: '<div>comp</div>'
    })

    app.mount('#app');
</script>
```





## 组合API拆分使用

```html
<div id="app">
    <p @click="clickFn">{{counter}}</p>
</div>

<script src="../../dist/vue.global.js"></script>


<script>

    const { createApp, reactive, toRefs } = Vue;

  	// 功能拆分出来
    function useCounter() {
        // 数据响应
        const state = reactive({ counter: 1 })
        function clickFn() {
            console.log('counter: ' + state.counter);
            state.counter += 1;
        }
        // 返回上下文
        return { state, clickFn }
    }

    const app = createApp({

        setup() {
          	// 导出
            const { state, clickFn } = useCounter();
            return { ...toRefs(state), clickFn }
        }
    })


    app.component('comp', {
        template: '<div>comp</div>'
    })

    app.mount('#app');

</script>
```

