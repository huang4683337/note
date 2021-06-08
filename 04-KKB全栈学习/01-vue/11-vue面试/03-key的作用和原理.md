## 源码

```
源码中找答案：src\core\vdom\patch.js - updateChildren()
```

```
patch 过程中 updateChildren 来对比新老 DOM, 
通过 key 进行同首同尾对比，
先从头部开始：key 相同，代表是同一个元素，元素内容不同则进行更新
key 不同时，从尾部开始对比

新的数据没了，就删除老数据中多余的
老数据没了，就将新数据中多的进行添加
```





## 测试代码

```html
<!DOCTYPE html>
<html>

<head>
    <title>03-key的作用及原理?</title>
</head>

<body>
    <div id="demo">
        <p v-for="item in items" :key="item">{{item}}</p>
    </div>
    <script src="../../dist/vue.js"></script>
    <script>
        // 创建实例
        const app = new Vue({
            el: '#demo',
            data: { items: ['a', 'b', 'c', 'd', 'e'] },
            mounted() {
                setTimeout(() => {
                    // 在 c 的前面插入 f
                    this.items.splice(2, 0, 'f')
                }, 2000);
            },
        });
    </script>
</body>

</html>
```

```
vue2_org/vue-study-web22-source/src/core/vdom/patch.js
updateChildren 打断点 424行
```



**不使用key**

![1623053498250](C:\Users\Amd\AppData\Roaming\Typora\typora-user-images\1623053498250.png)

```
先对于新数据来说，旧数据的 FCD 发生更新，新创建了 e
```



**使用 key - 同首同尾对比 **

```
// 首次循环patch A - 从头开始新旧相同 
A B C D E
A B F C D E

// 第2次循环patch B - 从头开始新旧相同 
B C D E
B F C D E

// 第3次循环patch E - 从头开始新旧不同，从尾部开始找新旧相同
C D E
F C D E

// 第4次循环patch D - 从尾部开始找新旧相同
C D
F C D

// 第5次循环patch C - 从尾部开始找新旧相同
C
F C

// oldCh全部处理结束，newCh中剩下的F，创建F并插入到C前面

```





## 结论

```
1. key的作用主要是为了高效的更新虚拟DOM，其原理是vue在patch过程中通过key可以精准判断两个节点是否是同一个，从而避免频繁更新不同元素，使得整个patch过程更加高效，减少DOM操作量，提高性能。

2. 另外，若不设置key还可能在列表更新时引发一些隐蔽的bug

3. vue中在使用相同标签名元素的过渡切换时，也会使用到key属性，其目的也是为了让vue可以区分它们，否则vue只会替换其内部属性而不会触发过渡效果。
```







