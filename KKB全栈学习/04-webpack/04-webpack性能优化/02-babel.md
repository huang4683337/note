## 什么是 Babel

+ js的编译器

+ 可以将 ES6+ 的代码转化成目标版本的代码（例如ES5）， 从此不用担心js兼容的问题

+ 可以通过插件机制根据需求灵活的扩展

[Babel 中文网](https://www.babeljs.cn/)

[Babel 官⽅⽹站](https://babeljs.io/)



## babel功能

[Babel 中文网](https://www.babeljs.cn/) ==> 点击菜单设置 ==> 可以看到在各个地方如何使用 babel 构建 ==> 点击 webpack ==> 查看案例



## babel 使用

### 安装

```shell
$ npm i babel-loader @babel/core @babel/preset-env -D
```

```shell
babel-loader: webpack 沟通 babel 的工具
@babel/core: babel 的核心，相当于一个老板
```

```shell
preset：@babel/core 下的四个员工

@babel/preset-env : 原生 js 的 es6+ 语法转 es5
@babel/preset-typescript : ts 语法转 es5
@babel/preset-react : jsx 语法转 es5
@babel/preset-flow : flow 语法转 es5
```



### 配置

**webpack.config.js**

```js
{
    test: /\.js$/,
    use: {
        loader: "babel-loader",
        options: {
            presets: ["@babel/preset-env"]
        }
    }
}
```



### 测试代码

```js
// src/index.js

const arr = [new Promise(() => { }), new Promise(() => { })];
arr.map(item => {
    console.log(item);
});
```

```shell
发现语法：箭头函数发生了转化
但是特性： Promise 没有发生改变
```







