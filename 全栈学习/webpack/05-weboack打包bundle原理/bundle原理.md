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



## 解析 bundle 文件执行步骤

+ bundle 是一个模块，而我们又知道模块是通过匿名函数生成的

  ```js
  (function(){})()
  ```

+ 这个匿名函数的参数就是是一个对象

  ```js
  {''}
  ```

  



## bundle打包原理分析

使用`webpack`打包时会执行 `webpack` 这个类
`webpack` 这个类会接收 `webpack.config.js`作为一个参数。

+ 通过 `entry` 可以得到从哪个文件开始打包

  + 对入口模块内容进行编译处理

  + 如果在入口模块中还引入了其他的模块，进入依赖模块进行编译处理

    若依赖模块中还引入了其他模块，则重复此步骤

+ 通过 `output` 可以得到生成的文件写入的位置

  + 因为编译后的代码存在一些特定的函数和对象，所以需要通过一个自执行函数将其引入处理

    ```js
    (fuunction(modules){
    // require
    // export
    })({
    
    })
    ```

    

##  bundle代码实现

### 目录结构

```shell
|- dist								# 打包后的文件目录
|- lib								# webpack 核心源码实现
	|- webpack.js				# webpack 核心源码实现
|- src								# 
	|- index.js					
	|- a.js
|- bundle.js					# 启动构建的入口
|- webpack.config.js	# webpack 配置文件
```



### 结构搭建

```js
// 启动构建的入口

// 引入自定义 webpack
const webpack = require('./lib/webpack.js');

// 引入 webpack 配置文件
const options = require('./webpack.config');


new webpack(options).run()
```

```js
// lib/webpack.js
module.exports = class webpack {
    constructor(options) {
        console.log(options);	// 得到 webpack 配置
    }
    run() { }
}
```

```js
// index.js
import { str } from "./a.js";
console.log(`hell ${str}`);
```

```js
// a.js
export const str = 'aaa';
```



### webpack 的 entry 解析

#### 解析出配置文件的入口、出口，并且挂载到 `webpack` 类下面

```js
constructor(options) {

    // 得到入口配置、出口配置
    // 将其挂载到 this 下
    const { entry, output } = options;
    this.entry  = entry;
    this.output = output;
    console.log("entry", entry);
    console.log("output", output);

}
```



#### 通过入口文件开始解析

```js
const fs = require('fs');


run() {
    this.parse(this.entry);
}

// 文件解析
parse(entryFile) {

    // 读取入口文件内容
    const content = fs.readFileSync(entryFile, 'utf-8');
    console.log(content);
    
}
```



#### 解析入口文件内容

+ 通过 @babel/parser 将文件内容`静态解析（解析时不会执行代码）`成 AST(抽象语法树)

  ```shell
  # 安装 @babel/parser
  $ npm install @babel/parser -D
  ```

  ```js
  const parser = require('@babel/parser');
  // 文件解析
  parse(entryFile) {
  
      // 读取入口文件内容
      const content = fs.readFileSync(entryFile, 'utf-8');
  
      // 通过 @babel/parser 将文件内容转成 ast
      const ast = parser.parse(content, {
          sourceType: 'module'
      });
  
      // ast 中每一行的内容就是一个节点
      console.log(ast.program.body);
  
  }
  
  // 每个 ast 节点中会对应一行内容的信息
  // 如果是通过 import 引入的文件，节点中有一个参数 value，他的值就是引入的路径
  ```



+ 通过 @babel/traverse 来对 babel/parser 转化的 AST 进行过滤处理，获取依赖文件路径

  **当解析的文件中引入了其他依赖时，如何在 ast 中获得依赖文件的路径**

    ```shell
  # 通过 @babel/traverse 来对 babel/parser 转化的 AST 进行过滤处理
    $ npm install --save @babel/traverse -D
  ```
  
    ```js
    const traverse = require('@babel/traverse').default;
    const path = require("path");
    
    // 文件解析
    parse(entryFile) {
    
        // 读取入口文件内容
        const content = fs.readFileSync(entryFile, 'utf-8');
    
        // 通过 @babel/parser 将文件内容转成 ast
        const ast = parser.parse(content, {
            sourceType: 'module'
        });
    
        // ast 中每一行的内容就是一个节点
        // console.log(ast.program.body);
    
    
        // 用来保存依赖文件地址的对应关系
        // 相对于入口文件的地址 ： 相对于项目根目录的地址
        const yilai = {};
    
        // 对 AST 进行过滤处理
        traverse(ast, {
            // 内容节点的类型作为一个函数使用
            ImportDeclaration({ node }) {
                // 节点类型为 ImportDeclaration 的
                // console.log(node);
    
                // 得到的路径是相对于入口文件的路径
                // console.log(node.source.value);
    
                // 获得到入口文件的目录
                // console.log(path.dirname(entryFile));
    
                // window 系统下的地址是 \, 需要将 \ 换成 /
                const newPath = './' + path.join(path.dirname(entryFile), node.source.value).replace(/\\/g, '/');
    
                // 建立地址对应关系
                yilai[node.source.value] = newPath;
    
            }
        })
    
    }
    ```



+ 通过 @babel/core --> babel/preset-env  对 AST 进行编译处理

  ```shell
  # 安装 @babel/core
  $ npm install @babel/core -D
  
  # 安装  @babel/preset-env
  $ npm install @babel/preset-env -D
  ```

  ```js
  run() {
      const info = this.parse(this.entry);
  
      // 文件解析生成的对象
      // console.log(info);
  }
  
  // 文件解析
  parse(entryFile) {
  
      // 读取入口文件内容
      const content = fs.readFileSync(entryFile, 'utf-8');
  
      // 通过 @babel/parser 将文件内容转成 ast
      const ast = parser.parse(content, {
          sourceType: 'module'
      });
  
      // ast 中每一行的内容就是一个节点
      // console.log(ast.program.body);
  
  
      // 用来保存依赖文件地址的对应关系
      // 相对于入口文件的地址 ： 相对于项目根目录的地址
      const yilai = {};
  
      // 对 AST 进行过滤处理
      traverse(ast, {
          // 内容节点的类型作为一个函数使用
          ImportDeclaration({ node }) {
              // 节点类型为 ImportDeclaration 的
              // console.log(node);
  
              // 得到的路径是相对于入口文件的路径
              // console.log(node.source.value);
  
              // 获得到入口文件的目录
              // console.log(path.dirname(entryFile));
  
              // window 系统下的地址是 \, 需要将 \ 换成 /
              const newPath = './' + path.join(path.dirname(entryFile), node.source.value).replace(/\\/g, '/');
  
              // 建立地址对应关系
              yilai[node.source.value] = newPath;
  
          }
      })
  
  
      // 通过 @babel/core --> babel/preset-env  对 AST 进行编译处理 
      const { code } = transformFromAst(ast, null, {
          // @babel/preset-env 对 ast 进行语法上的转换
          presets: ['@babel/preset-env'],
      })
  
      // 编译后的代码
      // console.log(code);
  
  
      return {
          entryFile,  // 入口文件地址
          yilai,      // 地址对应关系
          code,       // 编译后的代码
      }
  
  }
  ```



+ 对依赖的处理

  ```js
  constructor(options) {
  
      // 得到入口配置、出口配置
      // 将其挂载到 this 下
      const { entry, output } = options;
      this.entry = entry;
      this.output = output;
  
      // 存储所有解析后的文件对象
      this.module = [];
  }
  
  
  run() {
      const info = this.parse(this.entry);
  
      // 文件解析生成的对象
      // console.log(info);
  
      this.module.push(info);
  
      // 伪递归, 如果有依赖,则对依赖文件进行解析,存入到 this.module
      // 此处不考虑模块之间互相依赖的问题
      for (let fileObj of this.module) {
  
          const { yilai } = fileObj;
  
          if (yilai) {
              for (let j in yilai) {
                  this.module.push(this.parse(yilai[j]))
              }
          }
      }
  
      // 解析后的所有的文件对象
      // console.log(this.module);
  }
  ```

  



### webpack 的  output 解析

+ 生成 main.js 到 dist 目录

  ```js
  run() {
    ...
    this.file(obj);
  }
  
  
  /**
   * 生成 bundle 文件
   * 
   * 1 - 生成 main.js 到 dist 目录
   * 2 - 生成 bundle 内容 
   * @param {*} code 编译后的文件对象
   */
  file(code) {
  
      // 得到 bundle 文件输出位置
      // window 需要的文件路径做处理: \ 替换成 /
      const filePath = path.join(this.output.path, this.output.filename).replace(/\\/g, '/');
      // console.log(filePath);
      
      // 编译后的文件对象序列化
      const newCode = JSON.stringify(code)
      // console.log(newCode);
  
      const bundle = `()(${newCode})`
      fs.writeFileSync(filePath, bundle, "utf-8");
  }
  ```

  

+ 生成 bundle 内容

  ```js
  const bundle = `(function(modules){
      function load(module){
          function pathRequire(relativePath){
              return load(modules[module].yilai[relativePath]);
          }
  
          const exports = {};
  
          (function(require,code){
              eval(code)
          })(pathRequire,modules[module].code)
  
          return exports;
      }
      load('${this.entry}')
  })(${newCode})`
  ```

  ```shell
  # 执行构建
  $ node .\bundle.js
  
  # 将构建好的 dist/mian.js 的内容赋值、粘贴到浏览器的控制台
  # 输出结果为 hello aaa
  ```

  

  