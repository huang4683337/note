## 怎么看 vue 中的 diff 算法

diff 算法并不是 vue 专用的。

用到虚拟 DOM 的一般都是用了 diff 算法



## diff 的必要性 - 知道谁发生改变

```
src/core/instance/lifecycle.js - mountComponent()
```

```
1、每个组件挂载时会调用 $mount()
2、调用 $mount() 必然会创建 Watcher
3、所以 Watcher 和 组件实例是一一对应的
4、组件中可能存在很多个 data 中的 key 使用，但是一个组件对应一个 Watcher，那我们怎么确定哪个 key 发生改变了
5、key 发生改变时，使用 diff 进行新旧虚拟 DOM 对比，就知道了
```





## diff 的执行方式 -深度优先，同层比较

```
src/core/vdom/patch.js - patchVnode()
```

```js
patchVnode是diff发生的地方，整体策略：深度优先，同层比较
```

![1623056266960](C:\Users\Amd\AppData\Roaming\Typora\typora-user-images\1623056266960.png)

```
1、先比较根节点，
2、判断是否有子节点
深度优先：先通过根节点找到左叶子节点，然后新旧进行对比
同层比较：然后对比又叶子节点，然后对比父节点同层的兄弟节点，如果兄弟节点有孩子，先左孩子，再右孩子，然后再找父节点的父，一直到根节点
```





## diff 的高效性

```
src/core/vdom/patch.js - updateChildren()
```

```
新老虚拟DOM的头尾各有一个指针
1、如果新、老头部，或者新、老尾部满足sameVnode直接依次向后比较
2、如果老头、新尾满足 sameVnode ,真实DOM移动到老尾后面
3、如果老尾 、新头满足 sameVnode , 真实DOM移动到老头前面
4、以上情况不符合，在老中找和新头符合sameVnode，真实DOM移动到老头前面
```





## 测试代码

```html
<!DOCTYPE html>
<html>

<head>
    <title>Vue源码剖析</title>
    <script src="../../dist/vue.js"></script>
</head>

<body>
    <div id="demo">
        <h1>虚拟DOM</h1>
        <p>{{foo}}</p>
    </div>
    <script>
        // 创建实例
        const app = new Vue({
            el: '#demo',
            data: { foo: 'foo' },
            mounted() {
                setTimeout(() => {
                    this.foo = 'fooooo'
                }, 1000);
            }
        });
    </script>
</body>

</html>
```



## 总结

```
1.diff算法是虚拟DOM技术的必然产物：通过新旧虚拟DOM作对比（即diff），将变化的地方更新在真实DOM上；另外，也需要diff高效的执行对比过程，从而降低时间复杂度为O(n)。

2.vue 2.x中为了降低Watcher粒度，每个组件只有一个Watcher与之对应，只有引入diff才能精确找到发生变化的地方。

3.vue中diff执行的时刻是组件实例执行其更新函数时，它会比对上一次渲染结果oldVnode和新的渲染结果newVnode，此过程称为patch。

4.diff过程整体遵循深度优先、同层比较的策略；两个节点之间比较会根据它们是否拥有子节点或者文本节点做不同操作；比较两组子节点是算法的重点，首先假设头尾节点可能相同做4中比对尝试，如果没有找到相同节点才按照通用方式遍历查找，查找结束再按情况处理剩下的节点；借助key通常可以非常精确找到相同节点，因此整个patch过程非常高效。
```

