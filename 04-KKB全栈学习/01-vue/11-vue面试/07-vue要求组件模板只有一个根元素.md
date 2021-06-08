

## **1、`new Vue({el:'#app'})`  指定 SPA 单文件的入口**

```html
<body>
  <div id="app"></div>
</body>

<script>
	var vm = new Vue({
    el:'#app'
  })
</script>
```



## **2、单文件组件中，template下的元素 div。其实就是 `树` 状数据结构中的 `根`，作为遍历的起始点**

在 webpack 搭建的 vue 开发环境下，使用单文件组件时

```html
<template>
	<div>
    ......
  </div>
</template>
```

`template` 标签有三个特性：

+ 隐藏性：该标签不会在页面显示，即使标签里内容也不会显示，设置了`display:none`
+ 任意性：可以写在任何地方
+ 无效性：该标签的任何 HTML 内容都是无效的，不会起任何作用，只能通过 innerHTML 来获取标签里的内容

一个单文件组件，就是一个 vue 实例，如果 template 下有多个 div，无法确定哪个才是实例的根入口。

如果只有一个 div，会把这个 div 当作这个单文件组件的根，遍历 div 下的所有节点，生成 vnode，再渲染成真实DOM，插入指定的 HTML 下。



## **3、 `diff` 算法要求的，源码 `patch.js` 里的  `patchVnode()`**

diff 算法中需要一个 根 来遍历比对新老 vnode，来进行处理。