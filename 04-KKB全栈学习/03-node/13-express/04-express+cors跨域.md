## 安装

```shell
$ npm i cors -D
$ npm i express -D
```



## 配置

```js
const express = require('express');
const cors = require('cors');
const app = express();


app.use(cors());  //  不加上这句代码跨域访问时会出现错误，加上就不会出现跨域错误情况
```



## 完整版

```js
const express = require('express');
const cors = require('cors');
const app = express();


app.use(cors());  //  不加上这句代码跨域访问时会出现错误，加上就不会出现跨域错误情况


app.get("/login", (req, res) => {
    const data = {
        code: 200,
        msg: "登录成功",
        data: '登录成功--返回信息'
    }
    res.send(200, JSON.stringify(data))
})

app.get("/user", (req, res) => {
    const data = {
        code: 401,
        msg: "未登录，请登录",
        data: '未登录，请登录--返回信息'
    }
    res.send(401, JSON.stringify(data))
})

app.get("/info", (req, res) => {
    const data = {
        code: 200,
        msg: "请求成功",
        data: '请求成功--返回信息'
    }
    res.send(200, JSON.stringify(data))
})

app.get("/error", (req, res) => {
    const data = {
        code: 400,
        msg: "请求异常",
        data: '请求异常--返回信息'
    }
    res.send(400, JSON.stringify(data))
})

app.listen(3000, () => {
    console.log('http://localhost:3000/')
})
```

