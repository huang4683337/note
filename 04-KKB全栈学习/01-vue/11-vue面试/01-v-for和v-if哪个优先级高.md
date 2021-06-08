## **v-if 和 v-for 哪个优先级更高？ 如果两个同时出现，应该怎么优化得到更好的性能？**

```
源码中找答案 compiler/codegen/index.js
```



## v-for、v-if 同级

```
<p v-for="item in items" v-if="condition">{{item}}</p>
```



**测试代码**

```vue
<!DOCTYPE html>
<html>

<head>
    <title>Vue源码剖析</title>
    <script src="../../dist/vue.js"></script>
</head>

<body>
    <div id="demo">
        <p v-for="child in children" v-if="isFolder">{{child.title}}</p>
    </div>
    <script>
        // render > template > el
        // 创建实例
        const app = new Vue({
            el: '#demo',

            data: {
                children: [
                    { title: "1" },
                    { title: "2" },
                ],
            },
            computed: {
                isFolder() {
                    return this.children && this.children.length > 0
                }
            }
        })
        console.log(app.$options.render);
    </script>
</body>

</html>
```

```
ƒ anonymous(
) {
with(this){return _c('div',{attrs:{"id":"demo"}},_l((children),function(child){return (isFolder)?_c('p',[_v(_s(child.title))]):_e()}),0)}
}
```

```
渲染函数 l 先执行传染
然后判断 isFolder 是否显示
```

+ 先执行 v-for
+ 然后通过 v-if 判断



## v-if 在 v-for 外面

```
<template v-if="isFolder">
	<p v-for="child in children">{{child.title}}</p>
</template>
```



**测试代码**

```html
<!DOCTYPE html>
<html>

<head>
    <title>Vue源码剖析</title>
    <script src="../../dist/vue.js"></script>
</head>

<body>
    <div id="demo">
        <template v-if="isFolder">
            <p v-for="child in children">{{child.title}}</p>
        </template>
    </div>
    <script>
        // render > template > el
        // 创建实例
        const app = new Vue({
            el: '#demo',

            data: {
                children: [
                    { title: "1" },
                    { title: "2" },
                ],
            },
            computed: {
                isFolder() {
                    return this.children && this.children.length > 0
                }
            }
        })
        console.log(app.$options.render);
    </script>
</body>

</html>
```

```
ƒ anonymous(
) {
with(this){return _c('div',{attrs:{"id":"demo"}},[(isFolder)?_l((children),function(child){return _c('p',[_v(_s(child.title))])}):_e()],2)}
}
```

```
先判断 isFolder
然后再通过 l 函数渲染
```

+ v-if 先判断
+ 然后再进行 v-for



## 结论

+ v-for 的优先级大于 v-if  （直接告诉面试官，自己通过测试并且查看了源码）
+ 如果同时出现，每次渲染都会先执行循环再判断条件，无论如何循环都不可避免，浪费了性能
+ 要避免出现这种情况，则在外层嵌套template，在这一层进行v-if判断，然后在内部进行v-for循环
+ 如果条件出现在循环内部，可通过计算属性提前过滤掉那些不需要显示的项



**源码**

```js
  if (el.staticRoot && !el.staticProcessed) {
    return genStatic(el, state)
  } else if (el.once && !el.onceProcessed) {
    return genOnce(el, state)
  } else if (el.for && !el.forProcessed) {
    return genFor(el, state)
  } else if (el.if && !el.ifProcessed) {
    return genIf(el, state)
  } else if (el.tag === 'template' && !el.slotTarget && !state.pre) {
    return genChildren(el, state) || 'void 0'
  } else if (el.tag === 'slot') {
    return genSlot(el, state)
  } else {
    ......
  }
```

