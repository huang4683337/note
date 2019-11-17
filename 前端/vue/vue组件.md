# vue 组件

## 为什么在组件中 data 必须是一个函数

```javascript
/*
这是为了 data 的重复使用，因为在每个组件中都会有 data
*/

var obj = { a:1 };
console.log(obj==obj); // true;

function createObj(){
  return{ a:1 };
}
console.log( createObj()==createObj() ); // false

/* 这是因为在js中只有函数才会存在作用域，当data是一个函数时，每个函数都有自己的作用域，是独立的个体 */
```



## keep-alive

`keep-alive` 是 Vue 内置的一个组件，可以使被包含的组件保留状态，或避免重新渲染

```html
 <!--  可以实现组件的动态切换， 类似于选项卡的那种情况, 点一个加载一个， 加载过的会有缓存 -->

<keep-alive>
  <component>
    <!-- 该组件将被缓存！ -->
  </component>
</keep-alive>


<!-- 配合路由使用 -->
<keep-alive>
    <router-view>
        <!-- 所有路径匹配到的视图组件都会被缓存！ -->
    </router-view>
</keep-alive>

```



## vue更新数组时触发视图更新的方法

vue内部存在一些变异方法，通过这些方法操作数组会触发试图更新

```javascript
push()  pop()  shift()  unshift()  splice()  sort()  reverse()
```

```javascript
/* 
除了上述方法以外，我们还可以使用 set 方法

调用方法：Vue.set( target, key, value )
target：要更改的数据源(可以是对象或者数组)
key：要更改的具体数据
value ：重新赋的值
*/

// vue.set 或者 this.$set();
this.$set(this.testObj, 'hobby', '自行车');

// Object.assign
this.testObj = Object.assign({}, this.testObj, { 'hobby': '单车' });

```

