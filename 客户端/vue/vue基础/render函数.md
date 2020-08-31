## render函数

### 官网案例如下

[render函数相关](https://cn.vuejs.org/v2/guide/render-function.html#JSX)

```js
/**
 * 使用 render 函数创建一个div
 * 字体颜色是红色
 * 内容是 31231
 */
const vm = new Vue({
    render: (h) => h('div', { style: 'color: red;' }, '31231')
})

const copm = vm.$mount();

document.body.appendChild(copm.$el)
```

> 将 `h` 作为 `createElement` 的别名是 Vue 生态系统中的一个通用惯例，实际上也是 JSX 所要求的。从 Vue 的 Babel 插件的 [3.4.0 版本](https://github.com/vuejs/babel-plugin-transform-vue-jsx#h-auto-injection)开始，我们会在以 ES2015 语法声明的含有 JSX 的任何方法和 getter 中 (不是函数或箭头函数中) 自动注入 `const h = this.$createElement`，这样你就可以去掉 `(h)` 参数了。对于更早版本的插件，如果 `h` 在当前作用域中不可用，应用会抛错。



### 参数详解

> `createElement（TagName，Option，Content）接受三个参数`
>
> ```js
> createElement(" 定义的元素 "，{ 元素的性质 }，" 元素的内容"/[元素的内容])
> ```
>
> 注意： `TagName`是一个组件时才可以使用 props、methods 等组件所拥有的属性



`Option`

```js
{
  // 与 `v-bind:class` 的 API 相同，
  // 接受一个字符串、对象或字符串和对象组成的数组
  'class': {
    foo: true,
    bar: false
  },
    
    
  // 与 `v-bind:style` 的 API 相同，
  // 接受一个字符串、对象，或对象组成的数组
  style: {
    color: 'red',
    fontSize: '14px'
  },
    
    
  // 普通的 HTML 特性
  attrs: {
    id: 'foo'
  },
    
    
  // 组件 prop
  props: {
    myProp: 'bar'
  },
    
    
  // DOM 属性
  domProps: {
    innerHTML: 'baz'
  },
    
    
  // 事件监听器在 `on` 属性内，
  // 但不再支持如 `v-on:keyup.enter` 这样的修饰器。
  // 需要在处理函数中手动检查 keyCode。
  on: {
    click: this.clickHandler
  },
    
    
  // 仅用于组件，用于监听原生事件，而不是组件内部使用
  // `vm.$emit` 触发的事件。
  nativeOn: {
    click: this.nativeClickHandler
  },
    
    
  // 自定义指令。注意，你无法对 `binding` 中的 `oldValue`
  // 赋值，因为 Vue 已经自动为你进行了同步。
  directives: [
    {
      name: 'my-custom-directive',
      value: '2',
      expression: '1 + 1',
      arg: 'foo',
      modifiers: {
        bar: true
      }
    }
  ],
    
    
  // 作用域插槽的格式为
  // { name: props => VNode | Array<VNode> }
  scopedSlots: {
    default: props => createElement('span', props.text)
  },
    
    
  // 如果组件是其它组件的子组件，需为插槽指定名称
  slot: 'name-of-slot',
    
    
  // 其它特殊顶层属性
  key: 'myKey',
  ref: 'myRef',
    
    
  // 如果你在渲染函数中给多个元素都应用了相同的 ref 名，
  // 那么 `$refs.myRef` 会变成一个数组。
  refInFor: true
}
```

### 写个Demo

```js

```





如果想要使用`JSX`语法可以配合[Babel插件](https://github.com/vuejs/jsx)这么做

```js
import AnchoredHeading from './AnchoredHeading.vue'

new Vue({
  el: '#demo',
  render: function (h) {
    return (
      <AnchoredHeading level={1}>
        <span>Hello</span> world!
      </AnchoredHeading>
    )
  }
}
```













```js
import Vue from "vue";

// 传递一个组件配置，返回一个组件实例，并且挂载它到body上
function create(Component, props) {
  const vm = new Vue({
    render: (h) => h(Component, { props }),
  }).$mount(); // 挂载将虚拟dom转换为dom

  // dom追加
  document.body.appendChild(vm.$el)

  // 获取组件实例
  const comp = vm.$children[0]

  comp.remove = () => {
    document.body.removeChild(vm.$el)
    vm.$destroy()
  }
  
  return comp
}

export default create;
```

