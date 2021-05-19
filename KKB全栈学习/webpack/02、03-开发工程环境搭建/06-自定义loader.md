## 准备工作

**根目录新建目录 myLoaders**

**myLoaders 下新建文件 replace-loader.js**

```js
// 实现一个替换
```

**webpack.config.js 下添加规则配置**

```js
// 自定义 loader
{
    test: /\.js$/,
    use: {
        // 指定自定义位置
        loader: path.resolve(__dirname, "./myLoaders/replace-loader.js")
    }
}
```



**loader的结构**

```js
// loader 结构很简单 就是一个函数，但是不可以是箭头函数
 
// loader 必须有返回值, 没有返回值时 npm run dev 构建时报错

module.exports = function () {
    return '';
}
```



## loader 接收匹配文件的内容

```js
module.exports = function (source) {
    /**
     * source
     * 就是webpack.config.js中匹配到的以 js 为后缀的模块的内容
     */
    // console.log(source);

    // 将 js 中的 hello 替换为 您好
    // 打包构建后在 dist/index.js 中可以插
    // return source.replace("hello", '您好');
}
```



## loader 配置参数

**webpack.config.js 匹配规则中的 options 中配置参数**

```js
{
    test: /\.js$/,
    use: {
        // 指定自定义位置
        loader: path.resolve(__dirname, "./myLoaders/replace-loader.js"),
        options: {
            name: 'loader测试'
        }
    }
}
```



## loader 接收参数

**通过 this 关键字调用 we'bpack 提供的 loader API 接口**

```shell
webpack官网 https://webpack.js.org/
右上角修改语言为中文
点击菜单中文文档
点击 API
左侧菜单寻找 loader interface
```

```shell
查看 API 发现所有的 API 都是挂在 this 下
这就是为什么不能使用箭头函数的原因
```

```js
module.exports = function (source) {
  // this.query 接收loader的配置
   console.log('loader配置', this.query)
}
```

```shell
# npm run dev 之后输出了三次，这是因为有三次引用
# 第一次是匹配到 index.js 时输出的
# 第二次是 index.js 中通过 requrie 引入 index.less 时输出的
# 第三次是 index.less 引入图片时输出的
```



## loader 返回多个信息

```shell
使用 this.callback() 替换 return
```

```js
第一个参数必须是 Error 或者 null
第二个参数是一个 string 或者 Buffer。
可选的：第三个参数必须是一个可以被 this module 解析的 source map。
可选的：第四个参数，会被 webpack 忽略，可以是任何东西（例如一些元数据）
```

```js
const info = source.replace('hello', this.query.name);
this.callback(null, info)
```



## loader 异步处理

```shell
使用 this.async() 代替 return
告诉 loader-runner 这是一个异步回调。

this.async() 会返回一个 this.callback()
```

```js
// 异步处理
const callback = this.async();
setTimeout(() => {
    const info = source.replace('hello', this.query.name);
    callback(null, info);
}, 2000);
```



## 处理自定义loader路径

**webpack.config.js 中添加属性 resolveLoader**

```js
module.exports = {
  ...
    // 处理 loader 时的地址
    // 将 path.resolve(__dirname, "./myLoaders/replace-loader.js"),
  	// 替换为
    // replace-loader
    resolveLoader: {
        // 从哪里找 loader
        modules: ["node_modules", "./myLoaders"]
    },
}
```

**自定义loader的引入路径就和规范的一样了**

```js
// 自定义 loader
{
    test: /\.js$/,
    use: {
        // 指定自定义位置
        loader: "replace-loader",
        options: {
            name: 'loader测试'
        }
    }
}
```



## 实战环节

### 准备工作

**myLoader 下新建文件**

```shell
- myLader
	- kkb-less-loader.js
	- kkb-css-loader.js
	- kkb-style-loader.js
```



### less-loader 的实现

**kkb-less-loader.js**

```js
// 将 less 语法转成 css 语法

// 使用 less 将 less 语法转化为 css 语法
const less = require('less')
const { output } = require('../webpack.config')

module.exports = function (source) {
    // less 提供的 render API
    // error: 错误信息
    // output: 将 source 处理后的结果输出
    less.render(source, (error, output) => {
        this.callback(error, output.css);
    })
}
```



### css-loader 的实现

**kkb-css-loader.js**

```js
// 将 css 内容序列化 

module.exports = function (source) {
    return JSON.stringify(source);
}
```



### style-loader 的实现

**kkb-less-loader.js**

```js
// 动态创建 style 标签
// 将序列化的 css 内容放入 style 标签中
// 将 style 标签放入 head 

module.exports = function (source) {

    return `const str = document.createElement('style');
            str.innerHTML = ${source};
            document.head.appendChild(str);
    `;

}
```



### 规则配置

**webpack.config.js 下添加规则**

```js
// 自定义 less-loader、 css-loader、style-loader 的处理规则
{
    test: /\.less$/,
    use: ["kkb-style-loader", "kkb-css-loader", "kkb-less-loader"]
},
```

```js
// 自定义 loader 地址处理
resolveLoader: {
    // 从哪里找 loader
    modules: ["node_modules", "./myLoaders"]
},
```

