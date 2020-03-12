# 插槽

## 1、什么是slot

slot也就是大家俗称的'插槽'。是组件的一块==HTML模板==，这块模板显示不显示、以及怎样显示由==父组件来决定==。 ==> slot 核心就是**显示不显示**和**怎样显示**。

## 2、单个插槽(匿名插槽)

**单个插槽**是vue官方的叫法，也有些人叫**默认插槽**、**匿名插槽**(因为他不用设置name属性)。

它可以放在组件的任意一个位置，但是`一个组件只能有一个`该类型的插槽。



**父组件：**

```javas
<template>
     <div class="parent">
        <h3>这里是父组件</h3>
        <child>
            <div class="tmpl">
                <span>菜单1</span>
                <span>菜单2</span>
                <span>菜单3</span>
                <span>菜单4</span>
                <span>菜单5</span>
                <span>菜单6</span>
            </div>
        </child>
    </div>
</template>


<script>
    import child from './child.vue';
    export default {
        components:{
            child
        }
    }
</script>

<style>
.father{
    background-color: red;
}
</style>
```



**子组件：**

```html
<template>
    <div class="child">
        <h3>这里是子组件</h3>
        <slot></slot>
    </div>
</template>

<style>
.child{
    background-color: blue;
}
</style>

```

在父组件中将 需要插入的子组件放入到引入到组件标签==<child>==中，以此来控制组件中显示的内容



> 注意：为了方便观察，父组件中背景色为红色，子组件背景色为蓝色

![效果图](/Users/mrhuang/Downloads/笔记图片/slot1.png)



## 3、具名插槽

插槽中配置了name属性，可以出现在组件的任何位置。`可以在一个组件中出现多次`

> 父组件中出现  `slot='name'` => `<child> <div slot='parent'></div>  </child>`
>
> 子组件中通过 `name='name'接收` => `<slot name='parent'><slot>`

```html
<template>
  <div class="father">
    <h3>这里是父组件</h3>
    <child>
      <div class="tmpl" slot="up">
        <span>菜单1</span>
      </div>
      <div class="tmpl" slot="down">
        <span>菜单-1</span>
      </div>
      <div class="tmpl">
        <span>菜单->1</span>
      </div>
    </child>
  </div>
</template>
```

```css
<template>
  <div class="child">
    // 具名插槽
    <slot name="up"></slot>
    <h3>这里是子组件</h3>
    // 具名插槽
    <slot name="down"></slot>
    // 匿名插槽
    <slot></slot>
  </div>
</template>
```

![效果图](/Users/mrhuang/Downloads/笔记图片/solot2.png)





## 4、作用域插槽

**作用域插槽** 也就是 `带数据的插槽`。在有些大佬的头脑风暴下，现在vue中可以利用作用域插槽实现非渲染模式的开发。

+ render 函数，可以生成相当于`template`的内容，所以通过render函数就可以不写 `template`

**父组件**

```js
<template>
    <div>
        <BBBB v-slot='{name,add}'>
            <div @click="add">{{name}}</div>
        </BBBB>
    </div>
</template>

<script>
    import BBBB from './b.vue';
    export default {
        name: "aaa",
        data() {
            return {

            }
        },
        components: {
            BBBB,
        },
    }
</script>
```

**子组件**

```js
<script>
    export default {
        name: 'bbbb',
        data() {
            return {
                name: '我是b'
            }
        },
        methods: {
            add() {
                console.log('bbbbbbbbbbbbb');
            },
        },
        render() {
            return this.$scopedSlots.default({
                name: this.name,
                add: this.add
            });
        }
    }
</script>
```