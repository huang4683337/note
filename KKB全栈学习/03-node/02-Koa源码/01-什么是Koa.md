## 概述

Koa 是一个新的 web 框架， 致力于成为 web 应用和 API 开发领域中的一个更小、更富有表现力、更健壮的基石。

koa 是 Express 的下一代基于 Node.js 的 web 框架

koa2 完全使用 Promise 并配合 async 来实现异步



- koa 1.0 基于 es6 geeter函数
- koa 2.0 基于 async 



## 特点

+ 轻量，无捆绑
+ 中间件架构
+ 优雅的 API 设计
+ 增强的错误处理



## 使用

### 安装

```shell
$ npm i koa -S
```



### 基本使用

```js
const Koa = require('koa');

const app = new Koa();

app.use((ctx, next) => {
    ctx.body = [
        {
            name: 'tom'
        }
    ]
});

app.listen(3000,()=>{
    console.log('http://localhost:3000/')
})
```

```shell
$ node xx.js

# 浏览器输入 localhost:3000
```



### 切面描述使用

中间件的使用

```js
const Koa = require('koa');

const app = new Koa();


app.use(async (ctx, next) => {
    console.log('url', ctx.url);
    const start = Date.now();
    await next();
    const end = Date.now();
    console.log(`请求${ctx.url}耗时${parseInt(end - start)}ms`);
});

app.use((ctx, next) => {
    ctx.body = [
        {
            name: 'tom'
        }
    ]
});

app.listen(3000,()=>{
    console.log('http://localhost:3000/')
})
```



