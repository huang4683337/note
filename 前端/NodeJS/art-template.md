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



### 5、公共部分的处理

> 如何实现头部、底部公共部分的处理

> 自行查看 [模板继承]([https://aui.github.io/art-template/zh-cn/docs/syntax.html#%E6%A8%A1%E6%9D%BF%E7%BB%A7%E6%89%BF](https://aui.github.io/art-template/zh-cn/docs/syntax.html#模板继承))

> 可以继承 <style></style>、<div></div>、<script></script>

