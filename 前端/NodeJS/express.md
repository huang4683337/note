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

$ npm install --save art-template  express-art-template 	# 安装模板
```



## 2、使用

### 2.1、创建服务

> 创建index.js文件

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



###2.1、响应

```javascript
server.get('/', (req, res)=>{
    res.send('hello')
})

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

```shell
$ npm install path --save  # 安装node 
```



> 在 index.js 文件中

```javascript
// 公开指定目录 我们通过文件路径可以访问公开目录下的所有文件

const path = require('path');

// http://localhost:3000/static/xx  ==> 起别名
server.use('/public/', express.static(path.join(__dirname, './public/')));


// http://localhost:3000/xx
server.use( express.static('./public/') );


```



## 3、配置模板引擎

[art-tempalte 官方文档](http://aui.github.io/art-template/zh-cn/)

### 3.1、安装

```shell
$ npm install --save art-template 
$ npm install --save express-art-template

# 或者

$ npm install --save art-template  express-art-template 	
```



### 3.2、配置为 .html结尾的

```javascript
/*
在 express 中使用 art-template, 
.html 后缀的文件使用 art-template 模板解析  
默认 html文件 在 viws 文件夹中
*/

// 第一个参数表示, 渲染以 .html 结尾的文件时, 使用模板引擎
server.engine('html', require('express-art-template'));

// 将默认的 views 文件夹为 htmls
// 	server.set('views', xxx); 将默认的模板文件目录 修改为 xxx
server.set('views', path.join(__dirname, './htmls'))
```

### 3.3、使用

```html
<!-- 新建文件夹views -->
<!-- 新建文件 views/index.html -->

<h1>{{name}}</h1>
```



> 在 index.js 文件中

```javascript
//  res.render('模板文件名字', {模板数据});  art-template会默认去项目的views目录中查找
server.get('/', (req, res)=>{
    res.render('index.html',{
      name:'我是名字'
    });
})
```



## 4、中间件处理post 请求

### 4.1 安装中间件

```bash
$ npm install body-parser --save
```



### 4.2 引入

```javascript
const bodyParser = require('body-parser');
```



### 4.3 配置

```javascript
server.use(bodyParser.urlencoded({ extended: false }))

server.use(bodyParser.json())
```



### 4.4 使用

```html
<form action="/post" method="post">
  名字: <input type="text" name='name'>

  <br>

  内容: <textarea name="cont" id="" cols="30" rows="10"></textarea>


  <button type="submit">提交</button>
</form>
```



```javascript
// rea.body 就能拿到 form 表单提交的数据

server.post('/post', (req, res)=>{
  console.log(req.body);
  res.end(req.body);
})
```



## 5、路由配置

router.js

```javascript
const express = require('express');

// 创建一个路由容器
var router = express.Router();

// 路由挂在到容器中
router.get('/', (req, res)=>{
  res.end('你看到我了吗')
})

// 导出 router
module.exports = router;
```



index.js

```javascript
// 引入路由文件
const router = require('./router');

// 引入 express
const express = require('express');
const server = express();

// 将导出的 router 挂载到服务中
server.use(router)
```



