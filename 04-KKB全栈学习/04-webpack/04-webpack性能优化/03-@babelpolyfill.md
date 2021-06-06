### **作用**

打补丁： polyfill （包含所有 ES6+ 新特性的库）

使低版本浏览器能认识到许多新的特性，比如 promise



### **安装**

```shell
$ npm install  @babel/polyfill -D
```



### **使用**

```js
// 入口文件中引入

import "@babel/polyfill";
```



### **构建**

```shell
$ npm run build
# 发现打包后的文件非常大，所一需要按需加载
```



### **@babel/polyfill 按需加载**

`useBuiltIns` 选项是 babel 7 的新功能，这个选项告诉 babel 如何配置 `@babel/polyfill` 。 

| 属性  | 作用                                                         |
| ----- | ------------------------------------------------------------ |
| entry | 需要在 webpack 的入口文件里 `import "@babel/polyfill"`<br />babel 会根据你的使用情况导入垫片，没有使⽤的功能不会被导⼊相应的垫片 |
| usage | 不需要 import ，全自动检测，但是要安装 `@babel/polyfill` 。（试验阶段） |
| false | 如果你 `import "@babel/polyfill"` ，它不会排除掉没有使用的垫片，程序体积会庞大。(不推荐) |



**配置**

```json
// webpack.config.js
{
    test: /\.js$/,
    use: {
        loader: "babel-loader",
        options: {
            presets: [["@babel/preset-env", {
                // 目标浏览器集和
                targets: {
                    edge: "17",
                    chrome: "67"
                },
                // 指定 corejs 的版本 2 | 3
                corejs: 2,
                useBuiltIns: 'usage'
            }]]
        }
    }
}
```





**分离 @babel/polyfill 配置**

```js
// 根目录新建.babelrc⽂件，把 options 的值移⼊到该⽂件中
```



### **注意**

```js
因为 @babel/polyfill 基于 core-js@2.6.5 、regenerator-runtime@0.13.4 实现的.

但是好多新特性是在 core-js@2.x.x 实现的。

所以使用 @babel/polyfill 可以直接安装 core-js 、regenerator-runtime。而不用安装 @babel/polyfill
```