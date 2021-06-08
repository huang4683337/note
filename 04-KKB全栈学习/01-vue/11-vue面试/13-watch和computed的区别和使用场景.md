## 定义和语义区别

**Watch**

是一个监听器，用于监听响应数据的变化。

是一个对象 `{ 需要观察的表达式 : function(oldV, newV) }`

```vue
<div id="app">
    <input type="text" v-model="foo">
</div>

<script>
var vm = new Vue({
    el: "#app",
    data: { foo: 'foo' },
    watch: {
        foo: function (oldV, newV) {
            console.log(oldV + '------' + newV);
        }
    }
})

vm.foo = 2
</script>
```



**computed**

计算属性，通过某些数据算出一个新的数据。将新数据缓存。

依赖数据没有发生改变时，不会发生新的计算。

```vue
<script>

    var vm = new Vue({
        el: "#app",
        data: {
            firstName: 'first',
            lastName: 'last'
        },
        computed: {
            newName: function () {
                return this.firstName + '-------' + this.lastName;
            }
        }
    })


    console.log(vm.newName); // 访问的是缓存属性

</script>
```



## 功能区别

computed 能实现的 watch 基本上都能实现

computed 的底层来自于 watch，在 watch 的基础上加了些功能，例如：缓存



## 用法区别

优先使用 computed，因为 cpmputed 更加简洁、高效。

需要监听某些属性时，再使用 watch



## 使用场景

watch：数据变化时需要执行异步操作、开销较大的数据（搜索条件改变）

computed：复杂逻辑在一个依赖的数据发生改变时，也要发生改变（购物车的一个物品的数量改变，总价改变）