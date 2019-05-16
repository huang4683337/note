# Express

原生的 http 在某些方面不能满足开发需求，需要使用框架来提升开发效率。

<http://expressjs.com/> 作者是 tj, 他写了很多库 ==> www.github.com/tj



## 安装

```bash
$ mkdir myapp		# 创建目录
$ cd myapp			# 进入目录

$ npm init [--yes]	# 初始化创建 package.json 文件  （--yes 默认包信息 否则需要填写）

$ npm install express --save	# 安装 express 并保存在项目依赖列表中
$ npm install express --no-save	# 临时使用 express 不保存在项目依赖列表中
```



## 使用

### 创建服务

```javascript
// 引入 express
const express = require('express');

// 创建服务
const server = express();

// 监听端口
server.listen(3000, ()=>{
    console.log("http://localhost:3000")
} )
```



### 响应

```javascript
res.send();

//方法封装了 原生的 end、write、setHeader 等方法
```



### 路由

路由器

+ 请求方法
+ 请求路径
+ 请求处理函数



```javascript
//请求方式是get  请求地址是 /about ==> http://localhost:3000/about
server.get('/about', (req, res)=>{
    res.send("我是 about 页面");
})

server.post();
server.put();
server.delete();
```



```javascript
路由就是一张表，表中存在着具体的映射关系
```



链式调用

```javascript
server
	.get()
	.get()
	.post()
	.put()
	.delete()
```



### 提供静态文件

```javascript
// 公开指定目录 我们通过文件路径可以访问公开目录下的所有文件

// http://localhost:3000/static/xx  ==> 起别名
server.use('/static/', express.static('./public/') );


// http://localhost:3000/xx
server.use( express.static('./public/') );


```



## 配置模板引擎

[art-tempalte 官方文档](http://aui.github.io/art-template/zh-cn/)

