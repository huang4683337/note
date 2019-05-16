# Express

原生的 http 在某些方面不能满足开发需求，需要使用框架来提升开发效率。

<http://expressjs.com/> 作者是 tj, 他写了很多库 ==> www.github.com/tj



## 1、安装

```bash
$ mkdir myapp		# 创建目录
$ cd myapp			# 进入目录

$ npm init [--yes]	# 初始化创建 package.json 文件  （--yes 默认包信息 否则需要填写）

$ npm install express --save	# 安装 express 并保存在项目依赖列表中
$ npm install express --no-save	# 临时使用 express 不保存在项目依赖列表中
```



## 2、使用

### 2.1、创建服务

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



<<<<<<< HEAD
### 响应
=======
###2.1、响应
>>>>>>> 51c7398bb0dabb0f80d13461f25248ddf6337674

```javascript
res.send();

//方法封装了 原生的 end、write、setHeader 等方法
```



### 2.3、路由

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



### 2.4、提供静态文件

```javascript
// 公开指定目录 我们通过文件路径可以访问公开目录下的所有文件

// http://localhost:3000/static/xx  ==> 起别名
server.use('/static/', express.static('./public/') );


// http://localhost:3000/xx
server.use( express.static('./public/') );


```



## 3、配置模板引擎

[art-tempalte 官方文档](http://aui.github.io/art-template/zh-cn/)

### 3.1、安装

```bash
$ npm install --save art-template 
$ npm install --save express-art-template
```



### 3.2、配置

```bash
server.engine('html', require('express-art-template'));

# 第一个参数表示, 渲染以 .html 结尾的文件时, 使用模板引擎
# express-art-template 是专门在 express 框架中 把 art-template 整合到 express 中, 这是因为 express-art-template 依赖于 art-template
```

### 3.3、使用

```javascript
server.get('/', (req, res)=>{
    res.render('404.art');
})

//  res.render('模板文件名字', {模板数据});  art-template会默认去项目的views目录中查找

// 	server.set('views', xxx); 将默认的模板文件目录 修改为 xxx
```

