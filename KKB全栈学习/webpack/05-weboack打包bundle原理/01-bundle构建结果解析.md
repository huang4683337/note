## 准备工作

**webpack环境**

```shell
$ npm i -D webpack@4.44.1  webpack-cli@3.3.12
```



**入口文件**

```js
// src/index.js
console.log('hello world')
```



**webpack.config.js  入口文件、出口文件配置**

```js
const path = require("path");

module.exports = {
    entry: "./src/index.js",	
    output: {
        //指定输入资源的存放目录，位置
        //必须是绝对路径
        path: path.resolve(__dirname, "./dist"),
        filename: "index.js",
    },
    //mode
    mode: "development",
};
```



**package.json**

```json
"scripts": {
  "build": "webpack"
},
```



**打包构建**

```shell
$ npm run build
```



**dist 下新建 index.html 引入打包后的 js 文件**

```html
<script src="./index.js"></script>
```



**浏览器输出 hello world**



## 构建结果分析

**构建结果是一个一匿名自执行函数**

```js
(function(modules){
  
})({...})
```



**自执行函数的参数是一个对象**

每个 key 对应一个 模块

```shell
key: 是入口文件的路径
```

```shell
value：是一个函数 function(module, exports){}
这个函数的内容是用 eval('入口文件内容打包后的结果')
```



**bundle 简化后的代码**

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

