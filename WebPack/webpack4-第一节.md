## 安装

```shell
# 初始化 npm
$ npm init -y 

# 安装 webpack, 在 4 版本中 webpack 需要安装以下两个依赖
$ npm i webpack webpack-cli
```



## webpack 可以进行 0 配置

- 打包工具 => 输出后的结果（ js 模块 ）
- 打包（ 支持 js 模块化 ） 

零配置的意思就是支持安装即用，不要进行多余的配置

```shell
# 新建一个文件夹 webpack4
# webpack4 下新建文件夹 src 目录
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
code run
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



### 默认 webpack 文件名

```js
// webpack 默认配置文件名
node_modules => webpack-cli => config-yargs.js => defaultDescription
```

```shell
# 使用 webpack 再带命令执行 webpack 配置文件
$ npx webpack --config '自定义的 webpack 配置文件名字'
```



## 手动配置 webpack

```js
// 当前工程的根目录（webpack4）下新建文件 webpack.config.js

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



### 示例

```js
// src/index.js
console.log('hello world')
```

```shell
$ npx webpack		# 使用 webpack 编译
```

```js
/*
build.js 简化后的代码
*/

/*
实现步骤：

*/

(function (modules) {

  // 先定义一个缓存
  var installedModules = {};

  /**
   * 实际上就是
   * "./src/index.js": module
   */


  // 配置了（实现了） require 方法
  function __webpack_require__(moduleId) { 

    // 不在缓存中
    if (installedModules[moduleId]) {
      return installedModules[moduleId].exports;
    }

    // 安装
    var module = installedModules[moduleId] = {
      i: moduleId,
      l: false, // 是否加载完成
      exports: {}
    };

    // moduleId: ./src/index.js"
    // 通过 call 执行文件对应的匿名自执行函数
    modules[moduleId].call(module.exports, module, module.exports, __webpack_require__);

    module.l = true;

    return module.exports;
  }

  // 入口模块 默认为 "./src/index.js"
  return __webpack_require__(__webpack_require__.s = "./src/index.js");
})
  ({

    "./src/index.js":  // key: -> 当前模块路径（每个文件都是一个模块）

      (function (module, exports) { // value：函数

        eval("console.log('hello world')\n\n//# sourceURL=webpack:///./src/index.js?");

      })
  });
```



### package.json 

可以在 `package.json `  中配置一些脚本， 通过这些脚本来执行 `webpack`

在 package.json 添加 scripts 属性， 在 scripts 属性中配置 webpack 脚本

```json
/*
假设我们现在要配置 webpack 打包
a: npm 调用 webpack 脚本编译时 所用的指令 => npm run a

webpack 				--config		weboack.config.js
weboack编译指令	  配置命令		 webpack配置文件名字
*/


"scropts":{
  "a":"webpack --config webpack.config.js",
}
```

```shell
$ npm run a 	# 使用 webpack 打包
```



当我们在 `package.json`  中将 `webpack` 的打包命令配置为以下，并且 `webpack` 配置文件名称是我们自定义的

```shell
# webpack 打包命令
"scropts":{
  "a":"webpack",
}

  
# webpack 配置文件名为 a.js


$ npm run a -- --config a.js
# -- 的作用是将其后面的变成一个字符串来处理

# npm run a --config a.js
# 报错 webpack4@1.0.0 a: `webpack "webpack.config.js"`
# 指令中少了 --config
# 这是因为将 --config 直接当作指令处理了
# 如果我们再加一个 -- 就会把 '-- config'  以及后面的一起拼成一个字符串
# 在脚本中运行时就会转化成 "a":"webpack --config a.js",
```

