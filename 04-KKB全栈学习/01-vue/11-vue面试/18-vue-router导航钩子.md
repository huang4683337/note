## 全局钩子函数

### beforeEach 路由改变前调用

+ 用于验证用户权限

+ 使用方式

  ```
  beforeEach(to, form, next) 
  - to: 即将要进入的目标路由对象
  - from: 当前正要离开的路由对象
  - next: 路由控制参数
  	- next(): 一切正常，进入目标路由
  	- next(false): 取消导航，路由不发生改变
  	- next('/login'): 当前导航中断，进入一个新的导航
  	- next(error): 如果是一个 error 实例，则导航终止，该错误会被传递给 router.onError() 
  ```



### afterEach 路由改变后的钩子

+ 常用让页面返回最顶端
+ 比  beforeEach 少了 next 参数



```js
router.beforeEach((to, from, next) => {
    console.log(to.fullPath);
    if (to.fullPath != '/login') {//如果不是登录组件
        if (!localStorage.getItem("username")) {//如果没有登录，就先进入login组件
            进行登录
            next('/login');
        } else {//如果登录了，那就继续
            next();
        }
    } else {//如果是登录组件，那就继续。
        next();
    }
})
```





## 路由配置中的导航钩子

### beforeEnter

```
beforeEnter (to，from，next)
```

```js
const router = new VueRouter({
    routes: [
        {
            path: '/foo',
            component: Foo,
            beforeEnter: (to, from, next) => {
                // ...
            },
            beforeEnter: (route) => {
                // ...
            }
        }
    ]
});
```





## 组件内的钩子函数

+ `beforeRouteEnter`

  ```
  beforeRouteEnter (to,from,next)
  ```

  ```
  - 该组件的对应路由被 comfirm 前调用。
  - 此时组件实例还没被创建，所以不能获取实例（this）
  ```

+ `beforeRouteUpdate`

  ```
  beforeRouteUpdate (to,from,next)
  ```

  ```
  - 当前路由改变，但该组件被复用时候调用
  - 该函数内可以访问组件实例(this)
  ```

+ `beforeRouteLeave`

  ```
  beforeRouteLeave (to,from,next)
  ```

  ```
  - 当导航离开组件的对应路由时调用。
  - 该函数内可以访问获取组件实例（this）
  ```

  

```js
const Foo = {
    template: `...`,
    beforeRouteEnter(to, from, next) {
        // 在渲染该组件的对应路由被 confirm 前调用
        // 不！能！获取组件实例 `this`
        // 因为当钩子执行前，组件实例还没被创建
    },
    beforeRouteUpdate(to, from, next) {
        // 在当前路由改变，但是该组件被复用时调用
        // 举例来说，对于一个带有动态参数的路径 /foo/:id，在 /foo/1 和 /foo/2 之间跳转的时候，
        // 由于会渲染同样的 Foo 组件，因此组件实例会被复用。而这个钩子就会在这个情况下被调用。
        // 可以访问组件实例 `this`
    },
    beforeRouteLeave(to, from, next) {
        // 导航离开该组件的对应路由时调用
        // 可以访问组件实例 `this`
    }
}
```



## 检测路由变化

监听到路由对象发生变化，从而对路由变化做出响应

```js
const user = {
    template: '<div></div>',
    watch: {
        '$route'(to, from) {
            // 对路由做出响应
            // to , from 分别表示从哪跳转到哪，都是一个对象
            // to.path ( 表示的是要跳转到的路由的地址 eg: /home );
        }
    }
}
// 多了一个watch，这会带来依赖追踪的内存开销，
// 修改
const user = {
    template: '<div></div>',
    watch: {
        '$route.query.id' {
            // 请求个人描述
        },
        '$route.query.page' {
            // 请求列表
        }
    }
}
```





## 路由传参

```vue
<router-link :to='home/参数'>home页面</router-link>

<router-link :to="{path:'/home',params: {userId: '11111'}}">跳转</router-link>

<router-link :to="{path:'/home',query: {userId: '11111'}}">跳转</router-link>
```

```
this.$router.push({ name: 'home', params: { userId: '123' }});
```

```
this.$router.push({ name: 'home', query: { userId: '123' }});
```

