## 安装

```shell
# 初始化 npm
$ npm init -y 

# 安装 webpack 在 4 版本中 webpack 需要安装一下两个依赖
$ npm i webpack webpack-cli
```



## webpack 可以进行 0 配置

- 打包工具 => 输出后的结果（ js 模块 ）
- 打包（ 支持 js 模块化 ） 

零配置的意思就是支持安装即用，不要进行多余的配置

```shell
# 新建文件夹 src 目录
# src 下 新建文件 index.js 写入以下内容 console.log('hello world')

# 打包文件， npx：是 webpack 新生成的一个语法
$ npx webpack
# 默认在 node_modules => bin => webpack.cmd
# 这个文件是判断当前目录（bin）下是否存在 node.exe，存在则使用，
# 不存在时，则寻找当前目录上一级（node_modules）下的 webpack => bin => webpack.js

# 可以发现在当前目录下多了一个 dist 文件
# dist 下有一个 main.js
```



```js
// 推荐 vscode 的一个插件，可以直接执行node 代码
```



```js
/*
当我们使用 npx webpack 打包时会报警告

WARNING in configuration
The 'mode' option has not been set, webpack will fallback to 'production' for this value. Set 'mode' option t option to 'development' or 'production' to enable defaults for each environment.
You can also set it to 'none' to disable any default behavior. Learn more: https://webpack.js.org/configuratinfiguration/mode/
*/

/*
解决方案就是需要在 webpack.config.js 下添加 mode:production 或者 mode:development
*/
```



## 手动配置 webpack

```js
// 当前工程的根目录下新建文件 webpack.config.js

let path = require('path'); // node 内置模块，用于路径处理

module.exports = {
    mode: 'development', // 模式： production 、development
    entry: './src/index', // 入口文件
    output: {
        filename: 'bundle.js', // 打包后的文件名
        path: path.resolve(__dirname, 'dist'), // 打包后的路径, 必须是一个绝对路径
    }
}
```

