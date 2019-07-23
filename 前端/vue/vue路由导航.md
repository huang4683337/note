## 前言

在每个项目中，我们的头部导航栏是必不可少的，一般情况下都需要实现路由导航。意思就是导航的高亮状态会根据地址栏的变更来改变。

想要实现路由导航核心就是==active-class==这个属性，他的功能是设置激活链接时class属性，也就是当前页面所有与当前地址所匹配的的链接都会被添加class属性

[vue-router官方地址](https://router.vuejs.org/zh/api/#active-class)



```html
<router-link :to="/home" active-class="active">Home</router-link>

.active{
	color:red;
	font-size:44px;
}
```



## 具体实现

html

```html
<ul>
  <li v-for="item in nav">
    <router-link :to='item.path' style="display: inline-block;" active-class='active'>
      {{item.name}}
    </router-link>
  </li>
</ul>
```



js

```javascript
data() {
  return {
    nav: [
      { name: "首页", active: true, path:'/home'},
      { name: "银行", active: true, path:'/back'},
    ]
  };
},
```

