## 分析

**一个基于nodejs的入门级http服务，类似下面代码**

```js
const http = require('http')
const server = http.createServer((req, res) => {
    res.writeHead(200)
    res.end('hi kaikeba')
})
server.listen(3000, () => {
    console.log('http://localhost:3000/')
})
```

![1622627767951](C:\Users\Amd\AppData\Roaming\Typora\typora-user-images\1622627767951.png)

> 希望将蓝框以内，红框以外的封装成一个框架







## Koa 基本结构

实现 listen 方法，开启一个服务

实现 use 方法，挂载传入的回调函数

```
根目录新建文件 source/kkb.js
```

```js
const http = require("http");

class KKB {
    listen(...args) {
        const server = http.createServer((req, res) => {
            this.cb(req, res);
        })
        server.listen(...args);
    }

    use(cb) {
        this.cb = cb;
    }
}

module.exports = KKB;
```



**使用手写的仿 Koa 框架**

```
根目录新建文件 source/index.js
```

```js
const KKB = require("./kkb");
const app = new KKB();
app.use((req, res) => {
    res.writeHead(200);
    res.end("hi kaikeba");
});
app.listen(3000, () => {
    console.log('http://localhost:3000/');
});
```

```shell
$ node ./source/index.js
# 浏览器输出 'hi kaikeba'
```



## context 上下文的实现

koa 为了能够简化 API，引入上下文 context 概念，将原始请求对象 req 和响应对象 res 封装并挂载到 context上，并且在 context 上设置 getter 和 setter，从而简化操作。

### 了解 getter、setter

```
根目录新建文件 source/test-getter-setter.js
```

```js
const obj = {
    info: { name: '名字', desc: '人物信息' }
}

// 正常访问 name 时
console.log(obj.info.name);
```

```js
// 使用 getter setter 使代码更为优雅

const obj = {
    info: { name: '名字', desc: '人物信息' },
    get name() {
        return this.info.name
    },
    set name(val) {
        console.log('new name is' + val)
        this.info.name = val
    }
}
console.log(obj.name)
obj.name = 'kaikeba'
console.log(obj.name)
```



### 封装request、response和context

![1622633247099](C:\Users\Amd\AppData\Roaming\Typora\typora-user-images\1622633247099.png)

```js
对于复杂的代码实现封装
第一层 将原生的 req、res 封装成 Request、Respnse
第二层 将 Request、Respnse 封装成 Context（上下文环境）
```



### request 封装

[request源码地址](https://github.com/koajs/koa/blob/master/lib/request.js)

```
根目录新建文件 source/request.js
```



**第一层封装：**这里只封装了原生的 url、method 

```js
module.exports = {
    get url(){
        // 对原生的 req.url 封装
        return this.req.url;
    },

    
    get method(){
        // 对原生的 req.method 封装
        return this.req.method.toLowerCase();
    }
}
```



### response 封装

[response源码地址](https://github.com/koajs/koa/blob/master/lib/response.js)

```
根目录新建文件 source/response.js
```



**第一层封装：**这里封装返回了 body，因为原生没有 body，所以通过 set 给实例设置了 body。

```js
module.exports = {
    get body() {
        return this._body;
    },

    set body(val) {
        this._body = val;
    }
}
```



### context 封装

[context源码地址](https://github.com/koajs/koa/blob/master/lib/context.js)

```
根目录新建文件 source/context.js
```



**第二层封装：**将封装后的 request、response 封装到  context 上

```js
module.exports = {
    get url() {
        return this.request.url;
    },
    get method() {
        return this.request.method
    },
    get body() {
        return this.response.body;
    },
    set body(val) {
        this.response.body = val;
    }
};
```





### 框架上挂载封装后的三个类

> `Object.create()` 方法创建一个新对象，使用现有的对象来提供新创建的对象的  `__proto__`。 （请打开浏览器控制台以查看运行结果。）

```
根目下的 source/kkb.js
```

```js
const http = require("http");

// 引入封装的 request、response、context
const context = require('./context');
const request = require('./request');
const response = require('./response');


class KKB {
    listen(...args) {
        const server = http.createServer((req, res) => {
            // 创建上下文
            let ctx = this.createContext(req, res);
          
          	this.cb(ctx);
            
          	// 响应
            res.end(ctx.body);
        });
      	server.listen(...args);
    }


   	// ......

    // 构建上下文环境
    createContext(req, res) {
        // context 对象挂载到 ctx 原型上
        const ctx = Object.create(context);
        // request 对象挂载到 ctx.request 原型上
        ctx.request = Object.create(request);
        // response 对象挂载到 ctx.response 原型上
        ctx.response = Object.create(response);

        // 封装第一步
        // 原生的 req 挂载到 request 原型上
        // 原生的 rreseq 挂载到 response 原型上

        // 封装第二步
        // request、原生的 req 挂载到 ctx 原型上
        // responset、原生的 res 挂载到 ctx 原型上
        ctx.req = ctx.request.req = req;
        ctx.res = ctx.response.res = res;
        return ctx;
    }
}

module.exports = KKB;
```



### 执行校验

```
根目下的 source/index.js
```

```js
const KKB = require("./kkb");
const app = new KKB();
app.use(ctx => {
    ctx.body = 'hehe'
})
app.listen(3000, () => {
    console.log('http://localhost:3000/');
});
```

```shell
$ node ./source/index.js
# 浏览器输出 'hehe'
```



## 中间件的实现（责任链模式）

Koa中间件机制：Koa中间件机制就是函数式组合概念 Compose 的概念，将一组需要顺序执行的函数复合为一个函数，外层函数的参数实际是内层函数的返回值。

洋葱圈模型可以形象表示这种机制，是[源码](https://github.com/koajs/compose#readme)中的精髓和难点。

![1622627464195](C:\Users\Amd\AppData\Roaming\Typora\typora-user-images\1622627464195.png)



### 知识储备 - 函数组合

**例如有以下的函数**

```js
const add = (x, y) => x + y;
const square = z => z * z;
const fn = (x, y) => square(add(x, y));
console.log(fn(1, 2))
```

```shell
$ node  source/compose.js
# 9
```



**通过高阶函数（函数工厂）将上面两个函数组合调用**

```js
const add = (x, y) => x + y;
const square = z => z * z;

// 高阶函数：生成一个新的函数
const compose = (fn1, fn2) => (...args) => fn2(fn1(...args));

// fn 就是上面未组合的 fn
const fn = compose(add, square);

console.log(fn(1, 2));
```

```shell
$ node  source/compose.js
# 9
```



**多个函数组合**

多个函数组合：中间件的数目是不固定的，我们可以用数组来模拟

```js
const add = (x, y) => x + y;
const square = z => z * z;


const compose = (...[first, ...other]) => (...args) => {
    let ret = first(...args);
    other.forEach(fn => {
        ret = fn(ret);
    })
    return ret;
}

// 生成一个新的函数
const fn = compose(add, square, square);

console.log(fn(1, 2));
```



### 异步中间件原理

主要通过 promise 实现，函数返回一个执行承诺

**创建一个函数工厂**

```js
// 通过此函数生成一个新的函数
// 参数是可执行的函数组成的数组
function compose(middlewares) {
    return function () {
        // 递归调用，返回第一个执行承诺
        return dispatch(middlewares, 0);
    }
}
```



**通过递归来实现中间件功能**

```js
// 递归函数返回一个执行承诺
function dispatch(middlewares, i) {
    let fn = middlewares[i];
    // 如果函数不存在，返回一个空的执行承诺
    if (!fn) {
        return Promise.resolve();
    }
    return Promise.resolve(
        // next 下一级函数，或者下一级函数返回的执行承诺
        fn(function next() {
            return dispatch(middlewares, i + 1)
        })
    )
}
```



**执行调用**

```js
async function fn1(next) {
    console.log("fn1");
    await next();
    console.log("end fn1");
}

async function fn2(next) {
    console.log("fn2");
    await next();
    console.log("end fn2");
}

function fn3(next) {
    console.log("fn3");
}

const finalFn = compose([fn1, fn2, fn3]);
finalFn();
```

```shell
# 执行结果 就是一个 v 的过程
fn1
fn2
fn3
end fn2
end fn1
```



### 中间件实现

**实现中间件**

```
根目录新建文件 source/compose.js
# 添加 ctx 上下文参数
```

```js
const compose = function (middlewares) {
    return function (ctx) {
        // 递归调用，返回第一个执行承诺
        return dispatch(ctx, middlewares, 0);
    }
}

function dispatch(ctx, middlewares, i) {
    let fn = middlewares[i];
    // 如果函数不存在，返回一个空的执行承诺
    if (!fn) {
        return Promise.resolve();
    }
    return Promise.resolve(
        // next 下一级函数，或者下一级函数返回的执行承诺
        fn(ctx, function next() {
            return dispatch(ctx, middlewares, i + 1)
        })
    )
}

module.exports = compose;
```



**中间件引入到 kkb 类中**

```
根目录 source/kkb.js
```

```js
const compose = require('./compose');
class KKB {
  	
  	constructor() {
      	// 收集待执行函数
        this.middlewares = [];
    }
  
    listen(...args) {
        const server = http.createServer(async (req, res) => {

            // 创建上下文
            let ctx = this.createContext(req, res);

            // 合成中间件
            const fn = this.compose(this.middlewares)
            await fn(ctx);

            // 响应
            res.end(ctx.body);
        });
        server.listen(...args);
    }
  
   	use(cb) {
      	// 每次调用当前类的 use 函数时
      	// 将回调函数放入数组, 用于中间件调用
        this.middlewares.push(middlewares);
    }
  
    // 合成函数
    compose(middlewares) {
        return compose(middlewares);
    }
  
  	createContext(req, res) {
    	// ......
  	}
}
```



### 执行校验

```js
根目录 source/inidex.js
```

```js
const KKB = require("./kkb");
const app = new KKB();

const delay = () => new Promise(resolve => setTimeout(() => resolve(), 1000));

app.use(async (ctx, next) => {
    ctx.body = "1";
    await next();
    ctx.body += "5";
});

app.use(async (ctx, next) => {
    ctx.body += "2";
    await delay();
    await next();
    ctx.body += "4";
});

app.use(async (ctx, next) => {
    ctx.body += "3";
})

app.listen(3000, () => {
    console.log('http://localhost:3000/');
});
```

```shell
$ node ./source/index.js
# 浏览器输出 '12345'
```



[koa-compose源码](https://github.com/koajs/compose/blob/master/index.js)



## 常见的 Koa 中间件

### 常见中间件的实现

+ 一个 async 函数
+ 接收 ctx 和 next 两个参数
+ 任务结束后需要执行 next

```js
const mid = async(ctx, next)=>{
  // 来到中间件洋葱圈左边，自外而内
  next()
  // 中间件洋葱圈右边，自内而外
}
```



### 中间件常见的任务

+ 请求拦截
+ 路由
+ 日志
+ 静态文件服务



### Koa-router（策略模式）

```js
if(i=='a'){}
if(i=='b'){}
if(i=='c'){}
```

开发过程中经常会使用到路由，那么为什么要写路由呢？

因为如果有很多页面的话就需要写很多的 `if` 来进行判断，这样就显得很不优雅。



**通过引入策略问题来解决以上这个问题**

```
根目录新建文件 source/router.js
```

```js
class Router {
    constructor() {
        this.stack = [];
    }

    // 注册一个策略,放入栈中
    register(path, methods, middleware) {
        let route = { path, methods, middleware }
        this.stack.push(route);
    }

    get(path, middleware) {
        this.register(path, 'get', middleware);
    }

    post(path, middleware) {
        this.register(path, 'post', middleware);
    }

    routes() {
        let stock = this.stack;
        return async function (ctx, next) {
            let currentPath = ctx.url;
            let route;
            for (let i = 0; i < stock.length; i++) {
                let item = stock[i];
                // 判断path和method
                if (currentPath === item.path && item.methods.indexOf(ctx.method) >=  0) {
                    route = item.middleware;
                    break;
                }
            }
            if (typeof route === 'function') {
                route(ctx, next);
                return;
            }
            await next();
        };
    }
}
module.exports = Router;
```



**执行校验**

```
根目下的 source/index.js
```

```js
const KKB = require("./kkb");
const app = new KKB();

// router
const Router = require('./router')
const router = new Router();

router.get('/index', async ctx => {
    console.log('index,xx')
    ctx.body = 'index page';
});
router.get('/post', async ctx => { ctx.body = 'post page'; });
router.get('/list', async ctx => { ctx.body = 'list page'; });
router.post('/index', async ctx => { ctx.body = 'post page'; });
// 路由实例输出父中间件 router.routes()
app.use(router.routes());


app.listen(3000, () => {
    console.log('http://localhost:3000/');
});
```

```shell
$ node ./source/index.js
# 浏览器输出对应路由
```





### Koa-static

+ 配置绝对资源目录地址，默认为static
+ 获取文件或者目录信息
+ 静态文件读取
+ 返回

**实现 Koa-static**

```
根目录新建文件 source/router.js
```

```js
const fs = require("fs");
const path = require("path");

module.exports = (dirPath = "./public") => {
    return async (ctx, next) => {
        if (ctx.url.indexOf("/public") === 0) {
            // public开头 读取文件
            const url = path.resolve(__dirname, dirPath);
            const fileBaseName = path.basename(url);
            const filepath = url + ctx.url.replace("/public", "");
            console.log(filepath);
            // console.log(ctx.url,url, filepath, fileBaseName)
            try {
                stats = fs.statSync(filepath);
                if (stats.isDirectory()) {
                    const dir = fs.readdirSync(filepath);
                    // const
                    const ret = ['<div style="padding-left:20px">'];
                    dir.forEach(filename => {
                        console.log(filename);
                        // 简单认为不带小数点的格式，就是文件夹，实际应该用statSync
                        if (filename.indexOf(".") > -1) {
                            ret.push(
                                `<p><a style="color:black" href="${ctx.url
                                }/${filename}">${filename}</a></p>`
                            );
                        } else {
                            // 文件
                            ret.push(
                                `<p><a href="${ctx.url}/${filename}">${filename}</a></p>`
                            );
                        }
                    });
                    ret.push("</div>");
                    ctx.body = ret.join("");
                } else {
                    console.log("文件");
                    const content = fs.readFileSync(filepath);
                    ctx.body = content;
                }
            } catch (e) {
                // 报错了 文件不存在
                ctx.body = "404, not found";
            }
        } else {
            // 否则不是静态资源，直接去下一个中间件
            await next();
        }
    };
};
```



**执行校验**

```
根目录新建文件夹 source/public
放入图片 img.png
```

```
根目下的 source/index.js
```

```js
const KKB = require("./kkb");
const app = new KKB();
const static = require('./static')
app.use(static());


app.listen(3000, () => {
    console.log('http://localhost:3000/');
});
```

```shell
$ node ./source/index.js
# 浏览器输入地址  http://localhost:3000/public/img.png
```



### 请求拦截

```
根目录新建文件 source/iptable.js
```

```js
module.exports = async function (ctx, next) {
    const { res, req } = ctx;
    const blackList = ['127.0.0.1'];
    const ip = getClientIP(req);
    console.log(ip);
    if (blackList.includes(ip)) {//出现在黑名单中将被拒绝
        ctx.body = "not allowed";
    } else {
        await next();
    }
};
function getClientIP(req) {
    return (
        req.headers["x-forwarded-for"] || // 判断是否有反向代理 IP
        req.connection.remoteAddress || // 判断 connection 的远程 IP
        req.socket.remoteAddress || // 判断后端的 socket 的 IP
        req.connection.socket.remoteAddress
    );
}

```



**执行校验**

```js
根目下的 source/index.js
```

```js
const KKB = require("./kkb");
const app = new KKB();
app.use(require("./iptable"));
app.listen(3000, '0.0.0.0', () => {
    console.log('http://localhost:3000/');
});
```

```shell
$ node ./source/index.js
# 本机 IP 无法访问
```



[application源码地址](https://github.com/koajs/koa/blob/master/lib/application.js)



