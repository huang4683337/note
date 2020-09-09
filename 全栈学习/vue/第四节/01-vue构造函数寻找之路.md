

## 目的： 在vue包中找到vue的构造函数

+ `src\platforms\web\entry-runtime-with-compiler.js`
  + 对 `vue` 的 `mount` 做了扩展
    + 将模板编译为渲染函数  `render`
+ `src\platforms\web\runtime\index.js`
  + 安装web平台补丁函数  `patch`  用于初始化和更新
  + 实现了  `$mount`
+ `src\core\index.js`
  + 初始化全局API

+ `src\core\instance\index.js`
  + vue构造函数
  + 定义 `vue`实例属性和方法





### 下载源码

[报错解决](https://blog.csdn.net/LiyangBai/article/details/105253976)

```js
cd  node_modules\rollup-plugin-alias
npm install
npm run build
```



```js
package.json 添加--sourcemap


"dev": "rollup -w -c scripts/config.js --sourcemap --environment TARGET:web-full-dev",
```





## `package.json` 找 `dev` 选项对应的 

```js
 "dev": "rollup -w -c scripts/config.js --sourcemap --environment TARGET:web-full-dev",
```

> 总结：通过 `package.json` 找到了配置文件 `scripts/config.js`，以及配置文件中的目标 `web-full-dev`




### scripts/config.js 

  ```js
  作用：配置文件
  路径：scripts\config.js
  ```

  

### TARGET:web-full-dev

  ```js
  作用：配置文件对应的目标, 找到入口文件
  路径：src\platforms\web\entry-runtime-with-compiler.js
  ```

  ```JS
  'web-full-dev': {
      entry: resolve('web/entry-runtime-with-compiler.js'),
      dest: resolve('dist/vue.js'),
      format: 'umd',
      env: 'development',
      alias: { he: './entity-decoder' },
      banner
    },
      
  // entry中 web 对应的时配置的别名， resolve是对node的方法进行一次封装
  ```

> 总结：通过配置文件中的目标，找到了入口文件 `src\platforms\web\entry-runtime-with-compiler.js`



## 入口文件 entry-runtime-with-compiler.js

+ platforms 目录：决定在哪个平台

+ 路径：`src\platforms\web\entry-runtime-with-compiler.js`

+ 作用：对 `vue` 的 `mount` 做了扩展
  + 将模板编译为渲染函数  `render`

  ```JS
  // 对vue的mount做了扩展
  
   mount = Vue.prototype.$mount	// 先保存原来的
  
  // 再用一个新的方法覆盖
  Vue.prototype.$mount = function (){
    
    // 最后调用原来的方法
    return mount.call(this, el, hydrating)
  }
  ```

> 总结：对 `vue.$mount` 做了扩展
>
> 我们的目的是为了找到vue构造函数，我们发现这个文件引入了 `import Vue from './runtime/index'`



### mount扩展的功能

**主要是判断`render`、`template`、`el` 三种模板的优先级, 然后将模板编译为渲染函数**

**通过阅读源码的结果是：render > template > el**

```JS
// render > template > el
// 创建实例
const app = new Vue({
    el: '#demo',
    // template: '<div>template</div>',
    // template: '#app',
    // render(h){return h('div','render')},
    data:{foo:'foo'}
}
```



  ```JS
  // 扩展$mount
  const mount = Vue.prototype.$mount
  Vue.prototype.$mount = function (
    el?: string | Element, // ？表示可选参数，：后面表示类型
    hydrating?: boolean
  ): Component { // 返回值
    el = el && query(el)
  
    /* istanbul ignore if */
    if (el === document.body || el === document.documentElement) {
      process.env.NODE_ENV !== 'production' && warn(
        `Do not mount Vue to <html> or <body> - mount to normal elements instead.`
      )
      return this
    }
  
    // 获取选项
    const options = this.$options
    // resolve template/el and convert to render function
    // 没有render选项，才执行编译
    // render优先级最高
    if (!options.render) {
      let template = options.template
      // template优先级次之
      // 将template编译为render函数
      if (template) {
        if (typeof template === 'string') {
          if (template.charAt(0) === '#') {
            template = idToTemplate(template)
            /* istanbul ignore if */
            if (process.env.NODE_ENV !== 'production' && !template) {
              warn(
                `Template element not found or is empty: ${options.template}`,
                this
              )
            }
          }
        } else if (template.nodeType) {
          template = template.innerHTML
        } else {
          if (process.env.NODE_ENV !== 'production') {
            warn('invalid template option:' + template, this)
          }
          return this
        }
      } else if (el) {
        template = getOuterHTML(el)
      }
      // 执行编译
      if (template) {
        /* istanbul ignore if */
        if (process.env.NODE_ENV !== 'production' && config.performance && mark) {
          mark('compile')
        }
  
        // 编译得到render
        const { render, staticRenderFns } = compileToFunctions(template, {
          outputSourceRange: process.env.NODE_ENV !== 'production',
          shouldDecodeNewlines,
          shouldDecodeNewlinesForHref,
          delimiters: options.delimiters,
          comments: options.comments
        }, this)
        // 重新设置到选项上
        options.render = render
        options.staticRenderFns = staticRenderFns
  
        /* istanbul ignore if */
        if (process.env.NODE_ENV !== 'production' && config.performance && mark) {
          mark('compile end')
          measure(`vue ${this._name} compile`, 'compile', 'compile end')
        }
      }
    }
    // render：获取vdom
    // 挂载：将vdom转换为dom
    return mount.call(this, el, hydrating)
  }
  ```



> 总结：解读入口文件后，找到了其中重要的部分代码
>
> **主要是判断`render`、`template`、`el` 三种模板的优先级, 然后将模板编译为渲染函数**
>
> **通过阅读源码的结果是：render > template > el**
>
> 
>
> 并且找到了重要的引入` import Vue from './runtime/index'`，这个文件实现了  `$mount` 方法





## $mount 方法的实现

+ `platforms` 目录下

+ 路径：`src\platforms\web\runtime\index.js`

+ 作用：将虚拟 `DOM` 转化为真实 `DOM`

+ 做了什么

  + 安装web平台补丁函数 `patch`，用于初始化和更新
  + 实现了 `$mount`

  ```JS
  Vue.prototype.$mount = function (
    el?: string | Element,
    hydrating?: boolean
  ): Component {
    el = el && inBrowser ? query(el) : undefined
    // 真正的挂载函数
    return mountComponent(this, el, hydrating)
  }
  ```



> 总结：我们的目的是为了找到vue构造函数，我们发现这个文件引入了 `import Vue from 'core/index'`



## core/index初始化全局 API

+ `core`  目录下, 进入 `vue` 核心代码

+ 目录：`src\core\index.js`

+ 作用：初始化全局 API

> 总结：我们的目的是为了找到vue构造函数，我们发现这个文件引入了 `import Vue from './instance/index' `



## vue构造函数

在这个文件下我们发现了 vue的构造函数

+ `core` 目录下, 进入 `vue` 核心代码
+ 路径：`src\core\instance\index.js`
+ 调用了 `_init` 方法
+ 作用：**定义了构造函数 以及相关的实例属性和方法**

```js
import { initMixin } from './init'
import { stateMixin } from './state'
import { renderMixin } from './render'
import { eventsMixin } from './events'
import { lifecycleMixin } from './lifecycle'
import { warn } from '../util/index'

// 构造函数
function Vue (options) {
  if (process.env.NODE_ENV !== 'production' &&
    !(this instanceof Vue)
  ) {
    warn('Vue is a constructor and should be called with the `new` keyword')
  }
  // 初始化_init从哪来
  this._init(options)
}

// 定义实例属性和方法
initMixin(Vue) // 实现_init
stateMixin(Vue) // $data,$watch...
eventsMixin(Vue) // $on,$off,$emit
lifecycleMixin(Vue) // _update,$forceUpdate,$destroy
renderMixin(Vue) // _render, $nextTick

export default Vue

```



> // 初始化_init从哪来
>   this._init(options)

> 大胆猜测下是否在 `initMixin(Vue)`  混入 `init` 中？