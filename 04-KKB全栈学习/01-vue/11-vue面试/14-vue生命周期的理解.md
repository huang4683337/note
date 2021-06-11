## new Vue 到 beforeCreate

![1623338631207](C:\Users\Amd\AppData\Roaming\Typora\typora-user-images\1623338631207.png)

```
源码路径: src/core/instance/index.js

初始化事件、生命周期
```





## beforCreate 到 create

![1623338717935](C:\Users\Amd\AppData\Roaming\Typora\typora-user-images\1623338717935.png)

```
给数据添加属性据响应，
初始化 inject、provide
```



## created 到 beforMount

```
源码路径: src/core/instance/init.js  initMixin
```

```
源码路径: src/core/instance/init.js

确定根实例的 el 节点
- 有就通过 el 对应的 template 
- 没有就在 vm.$mount(el) 时使用 el 对应的 template 
```

```
源码路径: src/platforms/web/entry-runtime-with-compiler.js  $mount

是否有 template
- 有就执行 render 函数
- 没有就使用 el 外部的 template
```



![1623338928114](C:\Users\Amd\AppData\Roaming\Typora\typora-user-images\1623338928114.png)







## beforMount 到 mounted

```
源码路径: 
src/platforms/web/runtime/index.js $mount 
->  
src/core/instance/lifecycle.jsmountComponent

```



![1623339010946](C:\Users\Amd\AppData\Roaming\Typora\typora-user-images\1623339010946.png)







## beforUpdate 到 updated

```
beforUpdate 
源码路径: src/core/instance/lifecycle.js
```

```js
updated
源码路径: src/core/observer/scheduler.js  callUpdatedHooks 
```



![1623339063106](C:\Users\Amd\AppData\Roaming\Typora\typora-user-images\1623339063106.png)







## beforDestroy 到 destroyed

```
源码路径: src/core/instance/lifecycle.js  lifecycleMixin $destroy 
```



![1623339099262](C:\Users\Amd\AppData\Roaming\Typora\typora-user-images\1623339099262.png)



## 新增钩子

```
- activated：keep-alive 组件激活时调用。
	- 类似 created 没有真正创建，只是激活

- deactivated：keep-alive 组件停用（退出）时调用。
	- 类似 destroyed 没有真正移除，只是禁用
```

```js
页面第一次引入 keep-alive 时: created-> mounted-> activated

再次激活组件时: activated ->
```





