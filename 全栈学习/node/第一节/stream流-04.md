## 流 stream

> stream - 是用于与node中流数据交互的接口



假设一个场景，需要在浏览器访问图片应该怎么做？

首先第一反应就是通过 `fs.readFile` 来读取文件然后，然后输入返回到浏览器

```js
const http = require("http");
const fs = require("fs");
const server = http.createServer((req, res) => {
  const pngInfo = fs.readFileSync("./1.png").toString();
  res.end(pngInfo);
});

server.listen(3000);

```

> 但是这种处理方式有一个问题，node 读取文件到响应结束，会占用文件对应的内存资源，当处理的文件数量或者大小比较大时，会占用大量的内存资源。

那么怎么解决这种问题呢？可以通过流 （stream）来解决

```js
// stream.js
const fs = require('fs')
const rs2 = fs.createReadStream('./1.png')
const ws2 = fs.createWriteStream('./2.png')
rs2.pipe(ws2);

// 就会发现文件夹下多了一个 2.png 图片
```

```js
const rs2 = fs.createReadStream('./1.png')
// 相当于在图片 1.png 上插了一个读取的管道，另一端不知道插在哪里
// 在建立管道的过程中图片并没有加载到程序中，也就是没有占用程序内存
```

```js
const ws2 = fs.createWriteStream('./2.png')
// 建立一个读取的管道，一端插在一个不存在图片 2.png，另一端不知道插在哪里
// 建立的过程中不占用内存
```

```js
rs2.pipe(ws2); 
// 将两个管道连接，二进制数据流一滴一滴缓缓地从 rs2 流入 ws2，这时会占用极少的内存
```



**通过 http 访问一个图片时怎么处理**

访问 `http://localhost:3000`会发起两次请求

+ 请求 index.html 页面
+ index.html 页面中 img 标签请求图片

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <img src="1.png" alt="">
</body>
</html>
```

```js
const http = require("http");
const fs = require("fs");
const server = http.createServer((req, res) => {
  const { url, method, headers } = req;
  
  // 访问 index.html
  if (url === "/" && method === "GET") {
    fs.readFile("index.html", (err, data) => {
      res.statusCode = 200;
      res.setHeader("Content-Type", "text/html");
      res.end(data);
    });
  } else if (method === "GET" && headers.accept.indexOf("image/*") !== -1) {
    // 访问 index.html 中的 img 标签
    // 图片文件服务
    fs.createReadStream("./" + url).pipe(res);
  }

});

server.listen(3000, () => {
  console.log("http://localhost:3000");
});

```

>Accept代表发送端（客户端）希望接受的数据类型。 比如：Accept：text/xml; 代表客户端希望
>接受的数据类型是xml类型。
>Content-Type代表发送端（客户端|服务器）发送的实体数据的数据类型。 比如：ContentType：text/html; 代表发送端发送的数据格式是html。
>二者合起来， Accept:text/xml； Content-Type:text/html ，即代表希望接受的数据类型是xml格
>式，本次请求发送的数据的数据格式是html。