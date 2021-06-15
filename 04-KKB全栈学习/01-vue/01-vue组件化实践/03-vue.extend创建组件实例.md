## 定义壳子

```
src.uril/create.js
```

```js
/**
 * 传递一个组件配置，返回一个组件实例，并且挂载它到body上
 * @param {*} Component 一个完整的组件组件
 * @param {*} props 组件中的 props 属性对应参数
 */
function create(Component, props) {
  
}


export default create;
```



## 创建组件实例

```
src.uril/create.js
```

### extend

```
- extend 函数
	- const Cotr = Vue.extend(Component)
	- new Cort();
	- propsData 来接收组件参数
```

```js
function create(Component, props) {
    const Cotr = Vue.extend(Component);
    const vm = new Cotr({
        propsData: props
    })
}

export default create;
```



### new Vue

```
- new Vue() 借鸡生蛋

const vm = new Vue({
	render: h => h(Component,props)
})
```

```js
function create(Component, props) {
    const vm = new Vue({
        render: h => h(Component, props)
    })
}

export default create;
```





## 组件挂载

```
- $mount: 虚拟 DOM 转成真实 DOM，如果有参数则会替换参数对应 DOM
- vm.$el 获取当前组件真实 DOM
- 插入DOM: document.body.appendChild(comp.$el)
- 删除 DOM
- 销毁实例
```



### extend

```js
// 传递一个组件实例，返回一个组件实例，并且挂载到 body 上
import Vue from "vue";

// 创建函数接收要创建组件定义
function create(Component, props) {
    // Vue.extend 创建组件实例
    const Cotr = Vue.extend(Component);
    const comp = new Cotr({ propsData: props });

    // 虚拟 DOM 转 真实 DOM
    comp.$mount();

    // 获取当前组件真实 DOM
    // comp.$el;

    // 真实 DOM 放入 body
    document.body.appendChild(comp.$el);

    // 当前组件实例销毁方法
    comp.remove = () => {
        document.body.removeChild(comp.$el);
        comp.$destroy();
    }

    return comp;
}

export default create;
```



### new Vue

```js
// 传递一个组件实例，返回一个组件实例，并且挂载到 body 上
import Vue from "vue";

// 创建函数接收要创建组件定义
function create(Component, props) {
    // 通过 Vue 创建组件实例
    // 组件实例挂载在 Vue 下
    const vm = new Vue({
        render: h => h(Component, { props })
    });

    // 虚拟 DOM 转 真实 DOM
    vm.$mount();

    // 获取当前组件真实 DOM
    // vm.$el;

    // 真实 DOM 放入 body
    document.body.appendChild(vm.$el);

    // 获得当前组件实例
    const comp = vm.$children[0];

    // 当前组件实例销毁方法
    comp.remove = () => {
        document.body.removeChild(vm.$el);
        vm.$destroy();
    }

    return comp;
}

export default create;
```



## Notice 组件

```
src/utils/Notice.vue
```

```vue
<template>
  <div class="box" v-if="isShow">
    <h3>{{ title }}</h3>
    <p class="box-content">{{ message }}</p>
  </div>
</template>

<script>
export default {
  name: "Notice",
  props: {
    title: {
      type: String,
      default: "",
    },
    message: {
      type: String,
      default: "",
    },
    duration: {
      type: Number,
      default: 1000,
    },
  },
  data() {
    return {
      isShow: false,
    };
  },
  methods: {
    show() {
      this.isShow = true;
      setTimeout(this.hide, this.duration);
    },
    hide() {
      this.isShow = false;
      this.remove();
    },
  },
};
</script>

<style scoped>
* {
  margin: 0;
  padding: 0;
}
.box {
  position: absolute;
  width: 100%;
  top: 160px;
  left: 0;
  text-align: center;
  /* 关闭点击事件 */
  /* pointer-events: none; */
  background-color: #fff;
  border: grey 3px solid;
  box-sizing: border-box;
}
.box-content {
  width: 200px;
  margin: 10px auto;
  font-size: 14px;
  padding: 8px 16px;
  background: #fff;
}
</style>
```



## 全局挂载方法

### 定义组件

```
src/utils/create.js
```

```js
import Notice from '@/components/create.vue'
//... 
export default {
  install(Vue) {
    Vue.prototype.$notice = function (options) {
      return create(Notice, options)
    }
  }
}
```



### 引入组件

```
根目录/main.js
```

```js
import Notice from './utils/create';
Vue.use(Notice)
```



### 使用

```js
this.$notice({
  title: "title",
  message: "成功",
  duration: 500,
});
```

