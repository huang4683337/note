# 插槽

## 1、什么是slot

slot也就是大家俗称的'插槽'。是组件的一块==HTML模板==，这块模板显示不显示、以及怎样显示由==父组件来决定==。 ==> slot 核心就是**显示不显示**和**怎样显示**。

## 2、单个插槽

**单个插槽**是vue官方的叫法，也有些人叫**默认插槽**、**匿名插槽**(因为他不用设置name属性)。

它可以放在组件的任意一个位置，但是一个组件中只能有一个该类型的插槽。



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

```javascript
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

[效果图]()