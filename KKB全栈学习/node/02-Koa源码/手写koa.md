+ 洋葱圈模型又叫责任链模式
+ koa 引入了中间件机制，实现了切面编程的可能
+ 只要说koa 就是说的 2.0版本
  + koa 1.0 基于 es6 geeter函数
  + koa 2.0 基于 async 
+ [koa源码response](https://github.com/koajs/koa/blob/master/lib/response.js)
+ [koa源码request](https://github.com/koajs/koa/blob/master/lib/request.js)
+ [夏老师掘金](https://juejin.im/post/6887844088335302670)
+ [责任链模式（洋葱圈）compose](https://github.com/su37josephxia/wheel-awesome/tree/master/compose)
+ 策略模式？ github  kaikeba-code 里面有

  





1. koa 原理

2. context
3. Application剖析
4. 中间件机制
5. 常见中间件

```
业务程序
- 顺序 A-100 B+100
- 切面（AOP）描述需要 
	- 鉴权 有没有这个权限 在什么之前xxx
	- LOG 记账  在什么之后xxx
	- 路由守卫
```

