



[http-proxy 在 npm](https://www.npmjs.com/package/http-proxy)



[http协议报文详情](https://www.processon.com/view/link/5ec52841e0b34d5f261e14e0)

查看电子书 ------ 图解HTTP

[http缓存机制--全部类型](https://juejin.im/post/6844904116972421128)

[预检请求](https://juejin.im/post/6844903765548466183)



[sql基础教程](https://www.josephxia.com/document/database/)



[git版本控制](https://juejin.im/post/6844904199189184525)





网络协议，七层网络模型

+ 



现在都用的时 tcp/ip 协议，一共四层

+ 



+ node 中的 net 包代表的就是 http 协议

+ curl 用于http请求

+ 平常用于测试一个接口，但是又不想用任何三方库，比如： axios等，应该怎么办

  ```js
  // 适合用于埋点操作 - 此响应不会有结果返回
  const img = new Image();
  img.src = '需要访问的地址'
  ```

+ ==跨域==：只有通过 xhr（ajax请求） 发起请求时才会有跨域，这是用为**`浏览器的同源策略`**引起的



## http 协议



 

## 跨域

+ 第一层封印，添加响应头

  ```js
  res.setHeader('Access-Control-Allow-Origin', 'http://localhost:3000')
  ```

+ 第二层封印，预检请求（options）

  添加 token， 可以在请求头 headers 中添加 {'X-Token':'jilei'}, 用 X-  的原因是因为  “x-“代表扩展的意思

  [预检请求](https://juejin.im/post/6844903765548466183)

  服务端 根据对应的 预检请求做一个响应的处理

+ 第三层封印，cookie（session）

  浏览器是默认不带 cookie。

  + cookie 本质上是一个约定：请求头添加  Set-Cookie

  + 请求头添加 Access-Control-Allow-Credentials 为 true
  + 客户端添加 设置将cookie带给服务端



## node proxy 反向代理解决跨域

浏览器访问地址，利用服务端没有同源策略限制的影响，通过服务代理访问存储信息的服务

```js
const proxy = require('http-proxy-middleware')
app.use('/api', proxy({ target: 'http://localhost:4000', changeOrigin: false
}));
```



## Bodyparser

post 请求是一个流，如何将流转化成 json？

```js
// 监听到有数据流，六进来时，将每滴数据流存起来
let reqData = [];
let size = 0;

req.on('data', data => {
  console.log('>>>req on', data);
  reqData.push(data);
  size += data.length;
});


```

```js
// 监听到数据流完毕之后
req.on('end', function () {
  console.log('end')
  const data = Buffer.concat(reqData, size);	// 合并每一滴流
  console.log('data:', size, data.toString()) // 将数据流转成字符串
  res.end(`formdata:${data.toString()}`)
});

```

每次请求都会进行这样一次处理，所以会在洋葱圈（责任链）中写一个中间件





## 上传下载文件

+ 流对接

  ```js
  // 数据流一滴一滴拼接
  // 传入一滴就会像浏览器返回成功，实际上是没有成功的
  request.pipe(fis)
  response.end()
  ```

+  Buffer connect

  ```js
  // 将每一滴数据流存起来，一次性写入
  // 不需要频繁的读取硬盘，但是会占用一定的内存
  // 相比上面的，这种方法会在传入完毕才会向客户端返回
  request.on('data',data => {
    chunk.push(data)
    size += data.length
    console.log('data:',data ,size)
  })
  
  
  request.on('end',() => {
    console.log('end...')
    const buffer = Buffer.concat(chunk,size)
    size = 0
    fs.writeFileSync(outputFile,buffer)
    response.end()
  })
  
  ```

+ 流事件写入

  ```js
  // 每碰到一滴就写入一次，写入完毕，向客户端响应成功
  
  request.on('data', data => {
    console.log('data:',data)
    fis.write(data)
  })
  
  request.on('end', () => {
    fis.end()
    response.end()
  })
  
  ```

  

**秒传的实现**

使用 hash 生成文件摘要，使用 hash 运算所需要上传的文件，判断服务上是否存在此文件，存在的话直接显示上传成功



