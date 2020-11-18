## http

http请求都是以流（二进制）的形式进行信息传输

```js
const http = require("http");
const server = http.createServer((req, res) => {
  const { url, method } = req;
  console.log("url", url); // 浏览器通过哪个地址访问服务
  console.log("method", method); //访问服务的方式 GET、POST ...

    // 一次性设置响应头
    // response.writeHead(500, { "Content-Type": "text/plain;charset=utf-8" });

  res.end("1");
});

server.listen(3000);
```

