vue使用ts

+ 通过 exted 实现

  ```js
  export default Vue.extends({
    
  })
  ```

+ 通过 `vue-property-decorator` 实现





interface 只定义结构不定义实现



联合类型

```js
let a: string | number;
a 既可以是数字 也可以是 字符串
```



交叉类型

```js

```



泛型

```js
在定义时不指定具体类型，使用时传入具体类型
```



声明文件

+ 对自己写的文件进行声明

  ```js
  // index.js
  
  import VueRouter from './vue-router'
  // 实现一个VueRouter类
  const router = new VueRouter({
    routes
  })
  
  export default router
  ```

  ```js
  // index.d.ts
  import VueRouter from 'vue-router'
  
  declare const router: VueRouter
  
  export default router
  ```

+ 在某些模块上进行扩充

  vue 项⽬中还可以在 `shims-vue.d.ts` 中对已存在模块进⾏补充

  ```shell
  $ npm i @types/xxx
  ```

  ```js
  // main.ts
  // axios 挂载到 vue 原型上
  import axios from 'axios'
  Vue.prototype.$axios = axios;
  ```

  ```js
  //  shims-vue.d.ts
  import Vue from "vue";
  import { AxiosInstance } from "axios";
  
  declare module "vue/types/vue" {	// 在 node_modules 下找到 vue/types/vue
   interface Vue {	// 在 vue.d.ts 中扩展 axiox
     $axios: AxiosInstance;
   }
  }
  ```



装饰器

装饰器是⼯⼚函数，它能访问和修改装饰⽬标。

@xx 和 @xx( ) 区别

```js
// @xx
function log(target: Function) {
  target.prototype.log = function () {
    console.log('11111')
  }
}

@log
class Foo {
  bar = 'bar'
}

const foo = Foo();

foo.log();
```



```js
// @xx()
function log(fn: any) {
  return function (target: Function) {
    target.prototype.log = function () {
      fn('111')
    }
  }
}

@log(window.alert)
class Foo {
  bar = 'bar'
}

const foo = Foo();

foo.log();
```

+ 类装饰器
+ 方法装饰器
+ 属性装饰器

