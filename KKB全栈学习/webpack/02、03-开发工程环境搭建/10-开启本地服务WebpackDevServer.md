## WebpackDevServer

**提升开发效率的利器**

每次改完代码都需要重新打包⼀次，打开浏览器，刷新⼀次，很麻烦,我们可以安装使用 webpackdevserver 来改善这块的体验



## 开启一个本地服务

### 安装

```shell
$ npm install webpack-dev-server@3.11.0 -D
```



### 配置

**修改下 package.json**

```js
"scripts": {
 	"server": "webpack-dev-server"
},
```



**在 webpack.config.js 配置：**

```js
devServer: {
    contentBase: "./dist",	// 内容路径在哪里
    open: true,	// 自动打开浏览器
    port: 8081	// 端口号
},
```



### 运行

```shell
$ npm run server

# 浏览器输入 http://localhost:8081/
```

启动服务后，会发现 dist 目录没有了，这是因为 devServer 把打包后的模块不会放在 dist 目录下，而是放到内存中，从而提升速度。





## 本地数据 mock

### 安装

```shell
$ npm i express -D
```



### 根目录新建 server.js

```js

// 引入 express
const express = require('express');

// 创建服务
const server = express();

// 监听端口
server.listen(3000, () => {
    console.log("http://localhost:3000")
})

// get 请求
server.get('/api/info', (req, res) => {
    res.json({
        name: '张三',
        age: 5,
        msg: '欢迎'
    })
})
```



### 启动

```shell
$ node server.js

# 或者安装 nodemon
$ npm install --global nodemon

# 通过 nodemon 启动
$ nodemon server.js
```

```shell
浏览器输入：http://localhost:3000/api/info
```



### axios 前端请求

```shell
$ npm install axios -D
```

```js
// src/index.js

import axios from 'axios'
axios.get('http://localhost:3000/api/info').then(res => {
    console.log(res)
})
```

这里直接访问 http://localhost:8081/ 会报跨域



### 解决跨域

**修改webpack.config.js 设置服务器代理**

```js
devServer: {
  	...
    proxy: {
        "/api": {
            target: "http://localhost:3000"
        }
    },
},
```



**修改 src/index.js**

```js
axios.get('/api/info').then(res => {
    console.log(res)
})
```

