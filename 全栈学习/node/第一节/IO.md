

+ 读取到的文件为什么是一个 <buffer>?

  因为文件不一定是文本，所以是一个<buffer>（二进制）格式

+ node 中回调实行错误优先，所有回调的第一个参数都是 error

+ const {promisify} = require(‘util’)  node将回调封装成promise，是为了向下兼容

+ allsck码        utf-8编码（三个字节表示一个汉字）

+ 原型链

+ node http请求都是二进制流

+ node 读取文件到响应结束，会占用文件对应的内存资源，当处理的文件数量或者大小比较大时，会占用大量的内存资源。

  这种问题，需要用流来解决

  + 流的概念，通过视频自己画图，并做笔记。
  + http 访问响应，都是通过流来响应的 ，这样占的内存少
  + 下载文件，可以将需要下载的文件创建一个流，然后将文件对应的流导入，响应流中



[上课课件](https://github.com/su37josephxia/kaikeba-code)

[实现cli](https://www.npmjs.com/settings/josephxia/packages)

[课件](https://github.com/su37josephxia/node_practice)

[模板引擎造轮子](https://juejin.im/post/6884138429181870093)

[造论子](https://github.com/su37josephxia/wheel-awesome)

[nodeJS 大纲](https://www.processon.com/view/link/5d4b852ee4b07c4cf3069fec)





## 异步I/O

**响水壶**
关于上面的概念，网上有一个很经典的响水壶解释，怪怪在这里引申给大家，并谈谈自己的理解。

------

**隔壁王大爷**（不是隔壁老王，hhhhh~~）有个水壶，王大爷经常用它来烧开水。

王大爷把水壶放到火上烧，然后啥也不干在那等，直到水开了王大爷再去搞别的事情。**（同步阻塞）**

王大爷觉得自己有点憨，不打算等了。把水壶放上去之后大爷就是去看电视，时不时来瞅一眼有没有开
**（同步非阻塞）**

王大爷去买了个响水壶，他把响水壶放在火上，然后也是等着水开，水开的时候水壶会发出声响**（异步
阻塞）**

王大爷又觉得自己有点憨，他把响水壶放在火上然后去看电视，这时他不用是不是来瞅一眼，因为水开
的时候水壶会发出声音通知大爷。**（异步非阻塞）**

上面四个栗子里，阻塞非阻塞说明的是大爷的状态，同步非同步说明的是水壶的调用姿势。水壶能在烧
好的时候主动响起，就等同于我们异步的定义，能在结束时通知主线程并且回调。所以**异步一般配合非
阻塞，才能发挥其作用。**



+ 同步读取文件 `readFileSync`

  ```js
  同步
  // 等待文件读取完毕之后，才会执行 console
  const data = fs.readFileSync('./conf.json');
  
  console.log(data);  // 结果为一个 <buffer>
  
  console.log(data.toString()); // 将读取到的 <buffe> 转换成 字符串
  ```

  ```js
  /**
   * 读取到的文件为什么是一个 <buffer> ？
   * 
   * 因为文件不一定是文本，所以是一个<buffer>（二进制）格式
   */
  ```

+ 异步读取文件 `readFile`

  ```js
  // 直接读取文件，然后执行后面的代码。文件读取完毕后执行对应的回调
  fs.readFile('./conf.json',(err, data)=>{
      if(err) throw err;
      console.log(data.toString());
  })
  console.log(1);
  
  // 输出顺序为： 1 ， 读取到的文件
  ```

  ```js
  /**
   * node 中回调实行错误优先，所有回调的第一个参数都是 error
   */
  ```

+ `promisiry`

  开发中回调函数使用非常麻烦，但是将每个回调封装成 `promise` 也很麻烦。

  node自己实现了 `promisiry`  ，将回调风格的函数封装成 `promise` 风格的函数。这是为了兼容 `node` 以前的一些 api

  ```js
  (async ()=>{
      const  {promisify} = require('util');       // 从 util 中引入 promisify
      const readFile = promisify( fs.readFile );  // 将异步函数：文件读取放入 promisify
      const data = await readFile('./conf.json');
      console.log(data.toString());
  })()
  ```

  



## Buffer缓冲区

> 读取数据类型为Buffer

> Buffer - 用于在 TCP 流、文件系统操作、以及其他上下文中与八位字节流进行交互。 八位字节组成的数组，可以有效的在 JS 中存储二进制数据



+ 创建一个空的 <buffer>  `Buffer.alloc`

  ```js
  // 创建一个长度为 10 字节的以 0 填充的 buffer
  const buf1 = Buffer.alloc(10);
  console.log(buf1);	// <Buffer 00 00 00 00 00 00 00 00 00 00>
  
  /**
   * 1 bit（比特）= 一个二进制位，也就是 2 个数字。比如 00
   * 1 byte（字节）= 8 个二进制位，也就是 16 个数字。比如：00 00 00 00 00 00 00 00
   * 1 byte（字节） = 2 个 16 进制位
   * 所以 一个 16 进制位 = 4 个二进制位。也就是 4 个二进制 8 个数字。 比如：00 00 00 00
   * 
   * 以 16 进制形式表示 二进制 存储的一个状况
   * 一个 16 进制数代表 8 位二进制数
   * 一个二进制数代表一个字节
   */
  ```

+ 创建一个 `Buffer` 包含 `ascii`

  ```js
  // ascii 查询 http://ascii.911cha.com/
  const buf2 = Buffer.from('a')
  console.log(buf2,buf2.toString()) // <Buffer 61> a
  ```

+  创建 `Buffer` 包含 `UTF-8` 字节

  ```js
  // UFT-8：一种变长的编码方案，使用 1~6 个字节来存储；
  // UFT-32：一种固定长度的编码方案，不管字符编号大小，始终使用 4 个字节来存储；
  // UTF-16：介于 UTF-8 和 UTF-32 之间，使用 2 个或者 4 个字节来存储，长度既固定又可变。
  const buf3 = Buffer.from('中');
  console.log(buf3);  // <Buffer e4 b8 ad>
  ```

+ 其他方法

  ```js
  // Buffer.concat 将两个 Buffer 组合起来
  const buf2 = Buffer.from('a');	// <Buffer 61>
  const buf3 = Buffer.from('中');	// <Buffer e4 b8 ad>
  
  const buf4 = Buffer.concat([buf2,buf3]);
  console.log(buf4);  // <Buffer 61 e4 b8 ad>
  
  console.log(buf4.toString());  // a中
  ```

  ```js
  // 写入 Buffer 数据
  const buf1 = Buffer.alloc(10);
  buf1.write('hello');
  console.log(buf1);  // <Buffer 68 65 6c 6c 6f 00 00 00 00 00>
  ```

  ```js
  // 读取 Buffer 数据
  const buf3 = Buffer.from('中');
  console.log(buf3.toString());   // 中
  ```

  

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





## cli 工具实现

