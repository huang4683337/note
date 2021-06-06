# Vue.extend

在实际开发过程中我们需要开发一些全局的组件，比如：弹框等。

这类组件具有以下的共同性

+ 在任何地方都可以调用
+ 可以传入自定义的一些参数
+ 具有共工的 css 和 方法



具体是实现方式有两种：`借鸡生蛋：render函数`、`组件实例创建：extend方法`



## extend

[vue.extend](https://cn.vuejs.org/v2/api/?#Vue-extend)

[propsData](https://cn.vuejs.org/v2/api/?#propsData)

### 官网给的案例如下

```html
<div id="mount-point"></div>
```

```js
// 创建构造器
var Profile = Vue.extend({
  template: '<p>{{firstName}} {{lastName}} aka {{alias}}</p>',
  data: function () {
    return {
      firstName: 'Walter',
      lastName: 'White',
      alias: 'Heisenberg'
    }
  },
  nethods:{
    ...
  },
  ...
})
// 创建 Profile 实例，并挂载到一个元素上。
new Profile().$mount('#mount-point')
```

以上实现有个小问题就是通过`$mount('#mount-point')`生成的真实`DOM`会替换掉宿主标签，也就是说生成的`DOM` 中 `<div id="mount-point"></div>`不存在了



**解决方案：**

```js
const comp = new Profile().$mount();
document.querySelector("#mount-point").appendChild(comp.$el);
/**
comp.$el: 通过 vue.extend 构造出的 DOM 或者组件实例
*/
```



### **具体实现如下**

`createBox.js`

```js
import Vue from "vue";

/**
 * 传递一个组件配置，返回一个组件实例，并且挂载它到body上
 * @param {*} Component 一个完整的组件组件
 * @param {*} props     组件中的 props 属性对应参数
 */
function create(Component, props) {
    // 构建一个子类
    const Comp = Vue.extend(Component)

    // 创建子类实例, 并且传入相关属性值
    const comp = new Comp({
        propsData: props
    });

    // 虚拟 DOM 转成真实 DOM，如果有参数则会替换参数对应 DOM
    comp.$mount()

    // 真实 DOM 挂载到 body 下
    document.body.appendChild(comp.$el)

    // comp 实例下添加移除 DOM，销毁组件的方法
    comp.remove = () => {
        document.body.removeChild(comp.$el)
        comp.$destroy()
    }
    debugger

    // 返回当前实例
    return comp;
}


export default create;
```

`组件Notice.vue`

```html
<template>
  <div class="box" v-if="isShow">
    <h3>{{title}}</h3>
    <p class="box-content">{{message}}</p>
  </div>
</template>

<script>
export default {
  props: {
    title: {
      type: String,
      default: ""
    },
    message: {
      type: String,
      default: ""
    },
    duration: {
      type: Number,
      default: 1000
    }
  },
  data() {
    return {
      isShow: false
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
    }
  }
};
</script>
```

`使用`

```js
import createBox from "@/utils/createBox";
import Notice from "@/components/Notice";

createBox(Notice, {
  title: "来搬砖",
  message: isValid ? "成功" : "失败",
  duration: 3000,
}).show();
```



