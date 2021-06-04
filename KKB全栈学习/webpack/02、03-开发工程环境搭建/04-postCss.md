## postcss对于css的扩展

```js
postcss 对于 css 来说，就像 babel 对于 javaScript 来说
```

[postcss官网gitHub](https://github.com/postcss/postcss/blob/master/docs/README-cn.md)



## 准备工作

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



## 使用postcss

**安装两个 postcss 的插件**

autoprefixer ： 添加了 vendor 浏览器前缀，它使用 Can I Use 上面的数据

cssnano： 是一个模块化的 CSS 压缩器。

```shell
$ npm i autoprefixer cssnano -D
```



### autoprefixer 的使用

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



### cssnano 的使用

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

