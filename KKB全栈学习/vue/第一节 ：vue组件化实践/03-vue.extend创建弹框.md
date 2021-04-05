```js
import Vue from "vue";

/**
 * 传递一个组件配置，返回一个组件实例，并且挂载它到body上
 * @param {*} Component 一个完整的组件组件
 * @param {*} props 组件中的 props 属性对应参数
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

    // 返回当前实例
    return comp;
}


export default create;
```



## 全局挂载方法

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

