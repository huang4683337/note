## setup： 组合API入口函数

+ 只在初始化时执行一次
+ `return一个对象`，对象中的属性可以在 template 中使用



### setup 执行时机

+ beforeCreate 之前执行一次，此时组件对象还没有创建
+ this 是 unfined，无法通过 this 去访问 data、computed、methods、props
+ 所有 composition API 相关回调函数中都不可以使用 this 访问



### setup 返回值

+ 返回一个对象，供模板使用
  + 返回对象中的属性会和 data 中返回的属性合并成为组件属性
  + 返回对象中的方法，回和 methods 中的方法合并成为组件对象的方法
  + 如果出现重名，setup 优先
  + vue3 中 data、methods 的属性、方法尽量不要和 setup 中重名
+ 因为 setup 只会在 beforeCreate 之前执行一次，这时候还没创建组件等，所以 setup 无法访问 data、methods 中的属性、方法
+ setup 不能是一个 async 函数，因为 async 返回一个 promise 对象，但是模板中不能直接访问到 promise 对象中的属性



### setup 参数

`setup(props, context)`  /  `setup( props, (attrs, slots, emit) )`

+ **props：**父组件传入 && 在子组件中通过 props 接收的属性，组成的对象

  ```vue
  <!-- 父组件 -->
  <child :xxx="xxx"/>
  
  <!-- 子组件 -->
  props: ['xxx'],
  setup(props, context) {
      console.log('xxxx', props.xxx);
      return {};
  }
  ```

  

+ **attrs：**父组件传入 && 没有在子组件中通过 props 接收的属性，组成的对象，相当于 this.$attrs

  ```vue
  setup(props, context) {
      console.log(context.attrs);
  }
  ```

+ **slots：**所有传入插槽的内容组成的对象，相当于 this.$slots

  ```vue
  setup(props, context) {
      console.log(context.slots);
  }
  ```

+ **emit**：用来分发自定义事件的函数，相当于 this.$emit

  ```vue
  <!-- 父组件 -->
  <child @xxx="xxx"/>
  
  <!-- 子组件 -->
  <div @click="chilcCLick">子组建</div>
  
  setup(props, context) {
      const chilcCLick = () => {
          context.emit('xxx', 'vvvvv');
      };
      return { chilcCLick };
  }
  ```

  



## 数据响应

### ref：定义单值响应式

+ 定义单值响应式（一般用于定义基本数据类型的数据）
+ ref 内部是通过对 value 属性添加 getter/setter 来实现对数据的劫持
+ 通过 xx.value 获取、修改单值响应式

```js
// 定义单值响应式
let count = ref(0);

// 通过 xx.value 获取、修改单值响应式
console.log( count.value )
count.value++;
```



#### ref 定义多值响应式

如果使用 ref 定义对象/数组，vue 内部会自动将其转化为 reactive 的代理对象

```vue
<div>{{msg.name}}</div>
<button @click="handleClick">点我啊</button>

setup() {
    let msg = ref({
        name: 'Tome'
    });

    const handleClick = () => {
        msg.value.name += '=';
    };

    return {
        msg,
        handleClick
    };
}
```



### reactive：定义多值响应式

+ 定义复杂类型的数据响应式
+ reactive 接收一个普通对象，返回该对象的 响应式 代理器对象
  + 响应式转换是深层的，会影响对象内部嵌套的所有属性
  + 内部基于 Proxy 实现，通过代理对象操作源对象的数据，都是响应式的

```vue
<template>
    <div>{{user.age}}</div>
    <button @click="handleClick">点我啊</button>
</template>

<script lang="ts">
import { defineComponent, ref, reactive } from 'vue';

export default defineComponent({
    name: 'App',
    setup() {
        const user = reactive({
            name: 'Tom',
            age: 18
        });
        const handleClick = () => {
            user.age++;
        };
        return {
            user,
            handleClick
        };
    }
});
```

