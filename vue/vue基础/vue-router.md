

## 1、路由传参

```javascript
this.$router.push("home"); // 页面跳转

// 1 -- 利用路由配置的name
this.$router.push({ name: 'news', params: { userId: 123 }}); // 传参
this.$router.params.userId; // 获取参数
// 地址栏不会带 ?参数 这种，并且页面刷新后参数不存在
/*
	name 对应的是路由配置中的 name
	{
		path:'/path',
		name:'name',
		component:'cName'
	}
*/


// 2 -- 利用路由配置的path
this.$router.push({ path: '/news', query: { userId: 123 }}); // 传参
this.$router.query.userId; // 获取参数
// 地址栏带有 ?参数 这种，并且页面刷新后参数依旧存在
/*
	path 对应的是路由配置中的 path
	{
		path:'/path',
		name:'name',
		component:'cName'
	}
*/
```

```javascript
// 页面跳转
<router-link to="news">编译后就是个 a 标签 </router-link>


// params 传参  规则同上
<router-link :to="{ name: 'news', params: { userId: 1111}}">click to news page</router-link>

// query 传参  规则同上
<router-link :to="{ path: '/news', query: { userId: 1111}}">click to news page</router-link>
```

## 2、vue路由导航

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

### 具体实现

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



## 3、history 路由方法

### 3-1 路由中使用 history

+ 在 `new VueRouter()` 中开启 `history` 模式

  ```js
  const router = new VueRouter({
    mode: 'history',	// 开启 hisroty 模式
    routes: [...]
  })
  ```

+ `nginx` 配置

  ```shell
  location / {
    try_files $uri $uri/ /index.html;
  }
  ```

+ 如果项目通过 `ngixn` 代理来访问

  ```shell
  # 1- 项目中配置
  const router = new VueRouter({
    mode: 'history',	# 开启 hisroty 模式
    base: 'gsafetyWeb/iams/',	# nginx 代理地址
    routes: [...]
  })
  
  # 打包后的文件放在 D:/gsafetyWeb/iams
  |-- D:/
    |--  gsafetyWeb
    	|-- iams
    		|-- static
    		|-- index.html
    		|-- ...
    
  # 2 - nginx 配置 root
  root	D:/;
    
    
  # 3- nginx 配置地址代理
  location /gsafetyWeb/iams/ {
    try_files $uri $uri/ /gsafetyWeb/iams/index.html;
  }
  
  # 注意：
  #		loaction 的代理地址必须同开发环境中 'process.env.baseurl' 的值一样
  #		在 new VueRouter() 中的 base 属性值必须和 loaction 的代理地址一样
  #		try_files $uri $uri/ 后面的地址以 root 为参照，并最终指向 index.html
  ```

+ 如果通过 tomcat 访问

  **访问静态文件**

  ```xml
  tomcat ==> conf ==> server.xml ==> <host>
  	<Context path='/gsafetyweb/iams' docBase="静态文件存放地址，例如：D:\gsafetyWeb\iams" reloadable="true"
             crossContext="true"/>
  </host>
  
  path：为虚拟路径
  
docBase：为项目的路径
  
  启动 tomcat 后在浏览器输入 http://localhost:8888/gsafetyweb/iams/login
  
  
  ```
  > 注意： path 对应的值 ，需要和 访问的地址相匹配
  >
  > 比如 path 为：/a，访问地址定为 /b是不行的
  
  
  
  **刷新时重定向到页面**
  
  ```xml
  iams ==> WEB-INF 文件夹 ==> web.xml
  
  
  <?xml version="1.0" encoding="UTF-8"?>
  <web-app 
           xmlns="http://java.sun.com/xml/ns/javaee"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xsi:schemaLocation="http://java.sun.com/xml/ns/javaee 					http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd" 
         version="3.0" 
           metadata-complete="true">
  
    <display-name>Your Project Name</display-name>
    <description>
       Your Project Description
    </description>
    <error-page>  
     <error-code>404</error-code>  
     <location>/index.html</location>  
    </error-page>  
  </web-app>
  ```
  
  

### 3-2 history.pushState()

> 属于H5的一个方法， 存在浏览器兼容问题，并且需要后台配合配置nginx。
>
> 会创建一个新的历史并保存浏览器的文档状态（比如滚动条高度为10，在使用pushState方法后滚动页面假设滚动条高度为100，点击回退按钮发现滚动条高度回到10）

浏览器不刷新的情况下改变url地址，因为正常情况下改变url浏览器就会刷新一次页面。

```js
history.pushState(stateObject, tit, url);	// 在浏览器添加一个新的历史记录

//	stateObject : 当前页面的状态 {status:1}
//	tit : 标题
//	url : 新的地址
```

例如我们现在打开任何一个网站，F12打开控制台输入 `window.history` 。红色框的三个，其中length代表浏览记录队列长度，因为页面是刚打开的所以值是1； state是我们通过pushState添加记录所代表的第一个属性；

![](https://hkw-img.oss-cn-hongkong.aliyuncs.com/vue/vueRouter_history.png)



接下来通过`pushState`方法改变一次url。在控制台输入以下代码，发现浏览器并没有进行刷新。

但是 length和state属性发生了改变。

```js
window.history.pushState({status: 0} ,'' ,'?data=1');
```

![](https://hkw-img.oss-cn-hongkong.aliyuncs.com/vue/vueRouter_history1.png)

### 3-3 popstate 监听浏览器的前进和后退

> popstate 事件是`window`对象上的事件，配合 pushState() 和 replaceState() 方法使用。当`同一个文档`(可以理解为同一个网页，不能跳转，跳转了就不是同一个网页了)的浏览历史出现变化时，就会触发 popstate 事件。

> 可以理解为当触发了浏览器的 前进和后退 就会触发 popstate 事件

```js
window.addEventListener('popstate', function(evt){
  var state = evt.state;
  console.log(state);
})
```



### 3-4 history.replaceState()

```js
使用方法和 pushState 一样。 
区别在于 replaceState 会替换掉当前的历史记录
```



### 3-5 history 实现一个简单的单页面

```js
//1. 路由规则
const routes={
    '/user':user, //user是引入的视图   import user from './view/user' 
    '/about':about
}
//2. 路由控制类
class Router {
  start() {
    // 点击浏览器后退/前进按钮时会触发window.onpopstate事件, 我们在这时切换到相应页面
    // https://developer.mozilla.org/en-US/docs/Web/Events/popstate
    window.addEventListener('popstate', () => {
      this.load(location.pathname)
    })

    // 打开页面时加载当前页面 在单页面入口文件中要调用start方法
    this.load(location.pathname)
  }

  // 前往path, 变更地址栏URL, 并加载相应页面
  go(path) {
    // 变更地址栏URL
    history.pushState({}, '', path)
    // 加载页面
    this.load(path)
  }

  // 加载path路径的页面
  load(path) {
    // 首页
    if (path === '/') path = '/foo'
    // 创建页面实例
    const view = new routes[path]()
    // 调用页面方法, 把页面加载到document.body中
    view.mount(document.body)
  }
}
```



## 4、hash 路由方法

> 当 哈希值发生改变时，hashchange 监听到 url 的变化， 从而进行路由跳转。

