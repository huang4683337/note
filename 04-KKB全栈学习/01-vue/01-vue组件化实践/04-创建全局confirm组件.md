## 组件返回一个 promise 实例

```
修改 src/utils/create.js
```

```js
const showFonfirm = options => {
    return new Promise((resolve, reject) => {
        // 创建、挂载组件，得到一个组件实例
        const instance = create(Notice, options);

        // 组件实例下挂载一个 callback 方法
        instance.callback = action => {
            console.log(action);
            if (action == 'click') {
                resolve(action)
            } else {
                reject();
            }
        }

        // 调用组件内部显示的方法
        instance.show()
    })
}
```

```js
// 导出插件
export default {
    install(Vue) {
        Vue.prototype.$notice = function (options) {
            return showFonfirm(options);
        }
    }
}
```



## 修改 Notice 组件

```
src/utils/Notice.vue
```

```vue
<template>
  <div class="box" v-if="isShow">
    <h3>{{ title }}</h3>
    <p class="box-content">{{ message }}</p>
    <div @click="handleClick"></div>
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
    handleClick() {
      this.callback("click");
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

```
handleClick 调用 promise 中给组件实例添加的 callback 方法
```



## 使用

```js
this.$notice({
  title: "title",
  message: "成功",
  duration: 50000,
}).then((res) => {
  console.log(res);
});
```

