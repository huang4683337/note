![1622627464195](C:\Users\Amd\AppData\Roaming\Typora\typora-user-images\1622627464195.png)

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



## context 上下文的实现

koa 为了能够简化 API，引入上下文 context 概念，将原始请求对象 req 和响应对象 res 封装并挂载到 context上，并且在 context 上设置 getter 和 setter，从而简化操作。

**了解 getter、setter**

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



**封装request、response和context**

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
    }


   	// ......

    // 构建上下文环境
    createContext(req, res) {

        // 创建 context 实例
        const ctx = Object.create(context);
        // 创建 request 实例,挂载到 context 实例
        ctx.request = Object.create(request);
        // 创建 response 实例,挂载到 context 实例
        ctx.response = Object.create(response);

        // 封装第二步，原生的 req、res
        // 挂载到context 实例
        ctx.req = ctx.request.req = req;
        ctx.res = ctx.response.res = res;
        return ctx;
    }
}

module.exports = KKB;
```



## 中间件的实现

Koa中间件机制：Koa中间件机制就是函数式组合概念 Compose 的概念，将一组需要顺序执行的函数复合为一个函数，外层函数的参数实际是内层函数的返回值。

洋葱圈模型可以形象表示这种机制，是[源码](https://github.com/koajs/compose#readme)中的精髓和难点。

```
根目录新建文件 source/compose.js
```



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

































[application源码地址](https://github.com/koajs/koa/blob/master/lib/application.js)



