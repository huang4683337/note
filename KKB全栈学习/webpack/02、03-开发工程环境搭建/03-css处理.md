## 准备工作

**css文件**

```css
/* 新建文件 src/css/index.css */

body{
    background-color: red;
}
```



**入口文件 js**

```js
// 新建文件 src/index.js

import css from '.css/index.css'
console.log("hello webpack");
```



**安装 css 处理的依赖**

```shell
# 安装style-loader css-loader 依赖
$ npm i style-loader css-loader -D
```



**webpack 配置文件添加配置规则**

```js
// webpack.config.js 中配置 style-loader css-loader
// 规则是自后向前：先使用 css-loader 在使用 style-loader 处理

module: {
    rules: [
        {
            test: /\.css$/,
            use: ["style-loader", "css-loader"]
        },
    ],
}
```



**package.json 中添加打包命令**

```js
"scripts": {
  ...
  "dev": "webpack"
}
```



**编译打包**

```shell
# 打包命令
$ npm run dev
```



**查看打包结果**

```shell
# 新建 dist/index.html
# 引入打包后的 index.js <script src="./index.js"></script>
```



## less 的处理

**官网查看less处理**

[地址](https://webpack.docschina.org/)

```shell
右上角选择语言为中文 ==> 点击中文文档 ==> 点击 LOADER ==> 寻找关键字 less
```



**新建 less 文件**

```less
// src/css/index.less

body{
    background-color: blanchedalmond;
    div{
        width: 200px;
        height: 200px;
        background-color: greenyellow;
    }
}
```



**引入 less 文件**

```js
// src/index.js

import less from './css/index.less'
console.log("hello webpack");
```



**安装 less 处理**

```shell
# 安装
$  npm install less less-loader --save-dev

# 用于将 less 转化为 css
```



**webpack 配置文件添加 less 配置规则**

```js
// webpack.config.js

// module 下的 rules:[] 中添加以下

{
    test: /\.less$/,
    use: ["style-loader", "css-loader","less-loader"]
},
```

```shell
规则是自后向前：
先使用 less-loader 处理， 
然后使用 css-loader 处理, 
最后使用 style-loader 处理

less-loader：通过 less 包将 less 转为 css
css-loader：将 css 序列化打包到 build 文件中
style-loader: 将序列化的 css 提取出来，放入到一个动态的创建一个 style 标签中
```

```shell
浏览器访问结果编译后的结果 ==> F12 在控制台中可以看到 <head> 中有动态创建的 <style>, 其中就是我们写的样式
```



**执行构建、查看**

```shell
# 执行构建
$ npm run dev

# 新建 dist/index.html 引入 <script src="./index.js"></script>

# 打开浏览器查看
```



## sass 的处理

```shell
$ npm install sass-loader node-sass webpack --save-dev

# 其他操作同上，参考 less 处理
```



## postcss对于css的扩展

```js
postcss 对于 css 来说，就行 babel 对于 javaScript 来说
```

[postcss官网gitHub](https://github.com/postcss/postcss/blob/master/docs/README-cn.md)



### 准备工作

**安装 postcss**

```shell
# 安装
$ npm install  postcss@8.2.0 postcss-loader@4.1.0 -D
```



**webpack 配置文件添加配置规则**

```shell
使用的时间点是能拿到 css 的地方
也就是通过 css-loader 序列化之前
```

```js
// webpack.config.js

// module 下的 rules:[] 中添加以下

{
    test: /\.less$/,
    use: ["style-loader", "css-loader","postcss-loader","less-loader"]
},
```



### 使用postcss

**安装两个 postcss 的插件**

autoprefixer ： 添加了 vendor 浏览器前缀，它使用 Can I Use 上面的数据

cssnano： 是一个模块化的 CSS 压缩器。

```shell
$ npm i autoprefixer cssnano -D
```



#### autoprefixer 的使用

autoprefixer ： 添加了 vendor 浏览器前缀，它使用 Can I Use 上面的数据

**配置 postcss 的插件**

```js
// 根目录新建文件 postcss.config.js
module.exports = {
    plugins: [
        require('autoprefixer')
    ]
}
```



**css 文件中添加兼容css**

```less
// src/css/index.less
// 添加 display: flex;
body{
    background-color: blanchedalmond;
    div{
        c
        width: 200px;
        height: 200px;
        background-color: greenyellow;
    }
}
```

```shell
# 打包编译，在浏览器中查看样式，发现 display: flex; 并没有添加浏览器前缀

# 这是因为 autoprefixer 不知道到需要兼容哪些浏览器
```



**配置 css 需要兼容的目标浏览器**

[ browserslist - 样式自动添加前缀](https://caniuse.com/)

```js
查看本节04教程
```



在 `package.json` 中的配置是增加一个 `browserslist` 数组属性：

```js
module.exports = {
  ...
  "browserslist":["last 2 version", "> 1%"]
}
```

或者在项目根目录创建一个 `.browserslistrc` 文件：

```shell
# 注释是这样写的，以#号开头
# 每行一个浏览器集和描述
last 2 version
> 1%
```



**构建、查看**

```shell
# 构建
$ npm run dev

# 查看
# 浏览器中打开，发现 display: flex; 添加了前缀
```



**插件自己配置，覆盖项目中的配置**

```shell
在postcss.config.js中
使用某个插件的后面将配置作为参数传给插件
实现自定一配置覆盖项目全局配置
```

```js
module.exports = {
    plugins: [
        require('autoprefixer')({
            overrideBrowserslist: ['last 2 versions', '>1%']
        })
    ]
}
```



#### cssnano 的使用

cssnano： 是一个模块化的 CSS 压缩器

**使用插件**

```js
// postcss.config.js 中引入使用
// cssnano 有自己的默认配置，这里暂时就是用默认配置
require('cssnano')
```



**构建、查看**

```shell
# 构建
$ npm run dev

# 查看
# 浏览器中打开，发现样式被压缩成一行
```



## css的抽离

### **安装依赖**

```shell
$ npm i mini-css-extract-plugin -D
# 该插件将CSS提取到单独的文件中。它为每个包含CSS的JS文件创建一个CSS文件
# 也就是说每个js文件会对应一个css文件，会将js中引入的css合并成一个css文件
```



### **使用**

+ 引入插件

  ```js
  // webpack.config.js 中引入
  const MiniCssExtractPlugin = require("mini-css-extract-plugin");
  ```

- 这是一个插件需要在 plugin 中 new 一下进行配置

  ```js
  plugins: [
     new MiniCssExtractPlugin({
      // 抽出的 css 叫什么
      // name 是占位符,打包后的文件名使用当前文件名
      filename: "css/[name].css"
  	})
  ],
  ```

- module 中的 style-loader 需要替换成 miniCssExtractPlugin.loader 

  ```js
  {
      test: /\.less$/,
      use: [MiniCssExtractPlugin.loader, "css-loader", "postcss-loader", "less-loader"]
  },
  ```



### **构建、查看**

```shell
# 构建
$ npm run dev

# 查看
# dist 目录中出现了 index.css

# 在 index.html 中引入 <link rel="stylesheet" href="index.css">
# 浏览器打开查看
```



## css 打包后的地址拼接

### HtmlWebpackPlugin 插件

```shell
通过模板自动生成 html并自动引入打包后 js、css 文件
参考第一节 plugin 中的 HtmlWebpackPlugin 
```



### 配置全局css、js在html中的引入路径

```js
// 打包后在 index.html 中自动引入打包后的 js、css

// <script src="http://www.baidu.com/打包后的js、css 路径"></script>

// 可以通过修改此属性在本地访问远程服务的静态文件
```

```js
module.exports = {
  ...
  output:{
    // 作用于整个 html
    // 打包后在本地访问远程服务的静态文件
    // publicPath: 'xxx.con/assets/'
  	publicPath:'http://www.baidu.com'
	}
}
```



### css中的图片在打包后正常访问

```shell
# 处理图片地址
$ npm i file-loader -D
```

```js
module: { rules: [ 以下代码在这个位置 ] }

// 图片编译 
{
    test: /\.png$/,
    use: {
        loader: "file-loader",
        options: {
            // 使用 name 作为文件名占位符
            // 使用 ext 作为文件后缀占位符
            name: "[name].[ext]",
            // 图片打包后存放的文件夹
            outputPath: "images"
        }
    }
}
```



**css、less、sass 中引用了图片**

```js
// 对于css中图片的处理
use: [
    {
        // 使用minicss-extract-plugin抽离css
        loader: MiniCssExtractPlugin.loader,
        options: {
            // 以 css 所在的文件按夹文标准找到根目录
              // css 中通过 background:url('xxx.png') 引入的文件
            	// 通过插件对图片编译处理后的地址拼接 xxx
            	// 保证编译后能文件路径不会出错
            	// background:url('../xxx.png')
            publicPath: '../'
        },
    },
    "css-loader",
    "postcss-loader",
    "less-loader"
]
```


