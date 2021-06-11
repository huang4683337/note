## 准备

+ 通过 `vue-cli` 构建一个 `vue` 工程
+ 根目录下新建文件夹 `kvue`

**kvue/index.html**

```html
<!-- 引入 vue -->
<script src="../node_modules/vue/dist/vue.js"></script>
<div id="app">
    {{counter}}
</div>
```

```js
const app = new Vue({
    el: "#app",
    data() {
        return {
            counter: 1
        }
    }
})

setInterval(() => {
    app.counter++;
}, 1000);
```



```
浏览器运行上面代码，如果能正常运行。
那么就手写一个 vue 运行上面代码
```



## 原理分析

`new Vue` 进行初始化

+ `Observe` 对数据进行拦截，添加数据绑定属性

+ 同时`Compile` 对模板进行编译，找到其中动态绑定的数据，初始化视图

  同时定义`Watcher` 和更新函数

+ `Watcher` 调用更新函数`updaer`更新视图

+ `Dep` 响应数据发生改变，通知 `watcher` 更新

  ```
  - data 中的每个属性对应一个 dep
  - 每个模板表达式 {{}} 对应一个 watcher
  - data中的一个属性可能在多个地方调用，所以一个 dep 对应多个 watcher
  - 一个表达式可能对应多个属性{{a+b+c}}, 所以一个 watcher 对应多个 dep
  ```

  

![1623216230970](C:\Users\Amd\AppData\Roaming\Typora\typora-user-images\1623216230970.png)



## 涉及类型介绍

+ Observer：执⾏数据响应化（分辨数据是对象还是数组）
+ Compile：编译模板，初始化视图，收集依赖（更新函数、watcher创建）
+ Watcher：执⾏更新函数（更新dom）
+ Dep：管理多个Watcher，批量更新





## 创建框架壳子

```
根目下 kvue/kuve.js
```

```js
class Kvue {
    constructor(options) {
        this.options = options;
        this.$data = options.data;

        // 劫持 data 中的所有属性
        // 对其做响应式处理
        observe(this.$data);
    }
}
```



## Observe

```
根目下 kvue/kuve.js
- 判断是对象还是数组
- 遍历给对象添加响应属性
```

```js
function observe(obj) {
    // obj 必须是对象
    if (typeof obj !== 'object' || obj === null) {
        return obj;
    }
  
		// 创建 Observe 实例
    new Observe(obj);
}

class Observe {
    constructor(value) {
        // obj 对象
        this.value = value;

        if (Array.isArray(value)) {
            // 数组
        } else {
            // 对象
            this.walk(value);
        }
    }

    // 给 data 中的数据添加响应属性
    walk(obj) {
        Object.keys(obj).forEach(key => defineReactive(obj, key, obj[key]));
    }

}
```

```js
function defineReactive(obj, key, val) {

    // 递归遍历，对嵌套属性做数据响应
    observe(val);

    Object.defineProperty(obj, key, {
        get() {
            console.log(`get--${key}`);
            return val;
        },
        set(newVal) {
            if (newVal !== val) {
                console.log(`set--${key}`);
                observe(newVal);  // 心值是对象，对其做响应式处理
                val = newVal;
            }
        }
    })
}
```



**测试**

```
根目录下 src/kuve.html
```

```html
<!-- 引入 vue -->
<script src="./kvue.js"></script>
<div id="app">
    {{counter}}
</div>
```

```vue
<script>

const app = new Kvue({
    el: "#app",
    data: { counter: 1 }
})

setInterval(() => {
    app.$data.counter++;
}, 1000);

</script>
```

```
浏览器控制台持续输出
get--counter
set--counter
```



## 实现访问data属性代理

```
为甚么不直接把data数据挂到 vm 上？
因为这样修改数据就不会触发set了，也就无法就行后续更新操作了。
所以需要通过代理实现
```



```
根目录下 src/kuve.js
```

```js
function proxy(vm) {
    Object.keys(vm.$data).forEach(key => {
        Object.defineProperty(vm, key, {
            get() {
                return vm.$data[key]
            },
            set(v) {
                vm.$data[key] = v;
            }
        })
    })
}

class Kvue {
    constructor(options) {
        this.options = options;
        this.$data = options.data;

        // 劫持 data 中的所有属性
        // 对其做响应式处理
        observe(this.$data);

        // 访问代理 app.$data.counter++ ==> app.counter++;
        proxy(this);
    }
}
```



**测试**

```
根目录下 src/kuve.html
```

```html
<!-- 引入 vue -->
<script src="./kvue.js"></script>
<div id="app">
    {{counter}}
</div>
```

```vue
<script>

const app = new Kvue({
    el: "#app",
    data: { counter: 1 }
})

setInterval(() => {
    app.counter++;
}, 1000);

</script>
```

```
浏览器控制台持续输出
get--counter
set--counter
```



## Compile 实现

![1623219169233](C:\Users\Amd\AppData\Roaming\Typora\typora-user-images\1623219169233.png)

```
根目录下 src/kuve.js
```

```js
// 遍历 DOM 树
// 找到动态表达式、指令
class Compile {
    constructor(el, vm) {
        this.$vm = vm;
        this.$el = document.querySelector(el);

        if (this.$el) {
            this.compile(this.$el);
        }
    }

    // 递归传入的节点
    // 根据节点类型做操作
    compile(el) {
        // 获取所有孩子节点
        const childNodes = el.childNodes;
        childNodes.forEach(node => {
            if (node.nodeType === 1) {
                // 元素节点
                console.log('元素节点', node.nodeName);
                // 递归: 如果还有字节点
                if (node.hasChildNodes) {
                    this.compile(node);
                }
            } else if (node.nodeType === 3) {
                // 文本节点
                console.log('文本节点', node.nodeName);
            }
        })
    }
}
```

```js
class Kvue {
    constructor(options) {
        this.options = options;
        this.$data = options.data;

        // 劫持 data 中的所有属性
        // 对其做响应式处理
        observe(this.$data);

        // 访问代理 app.$data.counter++ ==> app.counter++;
        proxy(this);

        // 执行编译
        new Compile(options.el, this);
    }
}
```



**测试**

```
浏览器查看节点输出
```



### 文本节点解析

```
根目录下 src/kuve.js
Compile 类中
```

```js
class Compile {
    
  	// ......

    // 递归传入的节点
    // 根据节点类型做操作
    compile(el) {
        // 获取所有孩子节点
        const childNodes = el.childNodes;
        childNodes.forEach(node => {
            if (node.nodeType === 1) {
                // 元素节点
                console.log('元素节点', node.nodeName);
                // 递归: 如果还有字节点
                if (node.hasChildNodes) {
                    this.compile(node);
                }
            } else if (this.isInter(node)) {
                // 文本节点
                // console.log('文本节点', node.nodeName);
                this.compileText(node);
            }
        })
    }

    isInter(node) {
        // {{xx}}
        return node.nodeType === 3 && /\{\{(.*)\}\}/.test(node.textContent);
    }

    compileText(node) {
        console.log(RegExp.$1);
        node.textContent = this.$vm[RegExp.$1];
    }
}
```





## 依赖收集

### 分析

+ data 中每个属性对应一个 dep

+ 每个模板表达式（`{{xx}}`）对应一个 watcher

  watcher 中执行对应节点更新的回调函数

+ 模板解析`{{name}}`创建 `watcher`时，

  + 将当前 `watcher` 实例保存在 `Dep.target` 上

  + 触发数据响应的 `getter`, 将 `watcher` 添加到 name 属性对应的 `dep` 中

  + 置空 `Dep.target`

+ name 变化时，触发响应式的 setter，通过 dep 通知对应 watcher 更新



### compile发现动态引用时创建 Watcher

```
src/kuve.js 下 compile 类
- 解析每个模板表达式{{xx}} 时创建一个 watcher 实例
- 当前表达式的更新函数，传给 watcher 实例
```

```js
// 遍历 DOM 树
// 找到动态表达式、指令
class Compile {
    constructor(el, vm) {
        this.$vm = vm;
        this.$el = document.querySelector(el);

        if (this.$el) {
            this.compile(this.$el);
        }
    }

    // 递归传入的节点
    // 根据节点类型做操作
    compile(el) {
        // 获取所有孩子节点
        const childNodes = el.childNodes;
        childNodes.forEach(node => {
            if (node.nodeType === 1) {
                // 元素节点
                // console.log('元素节点', node.nodeName);
                // this.compileElement(node);

                // 递归: 如果还有字节点
                if (node.hasChildNodes) {
                    this.compile(node);
                }
            } else if (this.isInter(node)) {
                // 文本节点
                // console.log('文本节点', node.nodeName);
                this.compileText(node);
            }


        })
    }

    isInter(node) {
        // {{xx}}
        return node.nodeType === 3 && /\{\{(.*)\}\}/.test(node.textContent);
    }


    updata(node, exp, dir) {
        /**
         * node: 节点
         * exp：在data中国匹配当前属性的正则
         * dir：对应的更新函数的前缀
         */

        // 初始化
        const fn = this[dir + 'Updater'];
        fn && fn(node, this.$vm[exp])

        // 更新函数
        new Watcher(this.$vm, exp, val => {
            fn(node, val)
        })
    }

    // 处理{{xx}}
    compileText(node) {
        this.updata(node, RegExp.$1, 'text');
    }

    textUpdater(node, value) {
        node.textContent = value;
    }
}
```



### Watcher 实现 - 更新 DOM

```
src/kuve.js 下
- 调用对应属性的更新函数 （更新函数在 compile中）
- 保存当前 watchr 实例到 Dep.target
- 获取当前属性，触发 getter
- getter 中将 watcher 实例添加到对应的 dep 实例
- 添加完毕置空 Dep.target
```

```js
class Watcher {
    constructor(vm, key, updater) {
        this.vm = vm;
        this.key = key;
        this.updateFn = updater;

        // 创建实例时,把当前实例保存到Dep.target上
        Dep.target = this;
        // 触发 getter
        vm[key];
        // 置空
        Dep.target = null;
    }

    // 遇到绑定的表达式或指令
    // 首先初始化他们的值
    // 创建当前表达式对应的 watcher，管理更新
    update() {
        this.updateFn.call(this.vm, this.vm[this.key]);
    }
}
```



### Dep 实现 - 依赖收集

```
src/kuve.js 下
- 当前属性对应的 watcher
- 通知 watcher 更新
```

```js
// 依赖: data 中的每个key对应一个 dep 实例
class Dep {
    constructor() {
        this.deps = [];
    }

    // 保存对应的 watcher
    addDep(watcher) {
        this.deps.push(watcher);
    }

    // 通知更新
    notify() {
        this.deps.forEach(watcher => watcher.update());
    }
}
```



## 建立 Dep、Watcher 关系

```
src/kvue.js 下
- data 中的属性做响应式处理前，为每个属性创建一个 Dep 实例
- compile 解析模板时，为每个表达式创建一个 Watcher 实例
- wacher 实例将自身保存在 Dep.target 上
	- 触发对应属性的 getter
	- getter 中对应的 Dep 实例会进行依赖收集
- 改变 data 中某个属性式，会触发 setter
	- setter 中执行当前属性对应的 Dep 实例
	- Dep 实例通知当前属性对应的 watcher 实例
	- watcher 实例调用 compil 中对应的更新函数
```

```js
function defineReactive(obj, key, val) {

    // 递归遍历，对嵌套属性做数据响应
    observe(val);

    // 每个 key 对应一个 dep 实例
    const dep = new Dep();

    Object.defineProperty(obj, key, {
        get() {
            console.log(`get--${key}`);

            // 建立 dep 和 watche 关系
            Dep.target && dep.addDep(Dep.target)
            return val;
        },
        set(newVal) {
            if (newVal !== val) {
                console.log(`set--${key}`);
                observe(newVal);  // 心值是对象，对其做响应式处理
                val = newVal;

                // 通知更新
                dep.notify();
            }
        }
    })
}
```

