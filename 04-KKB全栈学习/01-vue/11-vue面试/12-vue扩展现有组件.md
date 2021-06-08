## 使用 Vue.mixin 全局混入

混入（mixin）是一种分发 Vue 组件中可复用功能的非常灵活的方式。混入对象可以包含任意组件选项。当组件使用混入时，所有混入对象的选项将被混入该组件本身选项。mixin 接收一个混入对象的数组。



**main.js 加入全局混入**

```js
Vue.mixin({
  updated: function () {
    console.log("我是全局的混入");
  }
})
```



**局部混入**

```vue
<template>
  <div>
    <p>num:{{ num }}</p>
    <p>
      <button @click="add">增加数量</button>
    </p>
  </div>
</template>

<script>
// 临时定义一个方法，用于局部混入
const addLog = {
  updated: function () {
    console.log(`数据发生变化，变化成${this.num}`);
  },
};

export default {
  name: "HelloWorld",
  mixins: [addLog],
  data() {
    return {
      num: 1,
    };
  },
  methods: {
    add() {
      this.num++;
    },
  },
  updated() {
    console.log("我是原生的update");
  },
};
</script>
```



**结论**

```
执行顺序：全局混入方法 > 局部混入方法 > 当前组件方法
```



## 加 slot 扩展

通过插槽添加内容