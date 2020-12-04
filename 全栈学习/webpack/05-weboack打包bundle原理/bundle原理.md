## 构建结果解析

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





## bundle打包原理分析

![1606999830052](C:\Users\Amd\AppData\Roaming\Typora\typora-user-images\1606999830052.png)







+ 通过 babel/parser 将文件路径转化成 AST(抽象语法树)

+ 通过 babel/traverse 来对 babel/parser转化的AST  进行解构处理
+ 通过 babel/core --> babel/preset-env  AST 进行编译处理
+ 