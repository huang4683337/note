## 起步

### 1. 文档

[art-tempalte 官方文档](http://aui.github.io/art-template/zh-cn/)

### 2. 安装

```shell
$ npm install art-template --save
```

### 3. 在html中使用

```html
<!DOCTYPE html>
<html lang="zh-cn">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>html渲染</title>
</head>
<body>
  	<!-- 渲染容器 -->
    <div id="cont"></div>
</body>
</html>


<!-- 引入 art-template-->
<script type="text/javascript" src="./node_modules/art-template/lib/template-web.js"></script>

<!-- 定义渲染模板 -->
<script type="text/template" id="tpl">
    <p>我的名字叫 {{ name }} </p>
</script>


<!-- 渲染逻辑 -->
<script>
    var htmlStr = template("tpl",{
        name:'名字',
    })
    console.log(htmlStr);

    document.getElementById('cont').innerHTML = htmlStr;

</script>
```

### 4. 在node中使用

```html
<!--ejs.html-->

<!DOCTYPE html>
<html lang="zh-cn">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>模板引擎</title>
</head>
<body>
    <p>我的名字叫 {{ name }} </p>
    <p>我的年龄是 {{ age }} </p>
    <p>{{ each arr }} {{ $value }} {{ /each }}</p>
</body>
</html>
```

```javascript
// ejs.js

var template = require('art-template');

// template.render('模板字符串', {'替换模版字符串'}, options);


var tplStr;
var fs = require('fs');
fs.readFile('./ejs.html', (err, data) => {
    if (err) {
        return console.log('读取失败')
    }
    tplStr = data.toString();
    var ret = template.render(tplStr, {
        name: '魔神',
        age: '18',
        arr: ['sda', 'aaaaaa', 'dddddd']
    });
		res.end(ret);
    console.log(ret);
})
```



### 5、公共部分的处理 — 模板继承

> 如何实现头部、底部公共部分的处理

> 自行查看 [模板继承]([https://aui.github.io/art-template/zh-cn/docs/syntax.html#%E6%A8%A1%E6%9D%BF%E7%BB%A7%E6%89%BF](https://aui.github.io/art-template/zh-cn/docs/syntax.html#模板继承))

> 可以继承 <style></style>、<div></div>、<script></script>



#### 继承

> Index.html

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>首页</title>
</head>

<body>

    {{ include './header.html'}}  <!-- 将 header.html 放入 index.html中 -->
    <h1>{{name}}</h1>

</body>

</html>
```



#### 写一个插槽，用于插入不断变化的部分

> Layout.html

```html
<!-- 写一个插槽，用于插入不断变化的部分 -->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>内容部分的插槽</title>
</head>
<body>

        {{ include './header.html'}}

        <!-- 插槽 -->
    
        {{ block 'content'}}
        <h1>我是默认的，如果不传过来自定义的内容就用我</h1>
        {{ /block}}

        {{ include './footer.html'}}
    
</body>
</html>
```



> 修改 index.html 中的内容 利用 extend 去继承 layout的

```html
<!-- 去继承layout定义的 -->
{{ extend './layout.html' }}

<!-- 我要填写自己的坑 -->
{{ block 'content' }}

<div>
    <h1>我是index.html页面，我要填写自己的坑</h1>
</div>

{{ /block }}
```



#### 继承 js脚本和静态的

```htm
{{ block 'css'}}  {{ /block}}

{{ block 'scripts'}}
	<script src=""></script>  <!-- 默认给个空的插槽 -->
{{ /block}}
```

> 填坑

```html
{{ block 'css' }}
	<link rel="stylesheet" href="/node_modules/bootstrap/dist/css/bootstrap.css">
{{ /block }}


{{ block 'scripts' }}
	<script src="/node_modules/jquery/dist/jquery.js"></script>
  <script src="/node_modules/bootstrap/dist/js/bootstrap.js"></script>
{{ /block }}
```

