## 目录结构

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



## 结构搭建

**根目录下新建 bundle.js**

```js
// 启动构建的入口

// 引入自定义 webpack
const webpack = require('./lib/webpack.js');

// 引入 webpack 配置文件
const options = require('./webpack.config');


new webpack(options).run()
```



**根目录下 lib/webpack.js 定义 webpack 的类**

```js
module.exports = class webpack {
    constructor(options) {
        console.log(options);	
    }
  	// 入口函数
    run() { }
  
  	// 入口文件解析函数
  	parse(){ }
}
```

```shell
$ node bundle.js

# 得到 webpack 配置, 也就是 webpack.config.js 的配置内容
```



**src/index.js**

```js
import { a } from "./a.js";
console.log(`hello ${a}`);
```



**src/a.js**

```js
import { b } from 'b.js';
export const a = `这是a.js文件 ${b}`;
```



**src/b.js**

```js
export const b = '这是b.js文件';
```





## webpck类获取到配置文件信息

将解析到的 `entry` 、`output` 信息挂载到 `webpack` 类上

```js
// lib/webpack.js 中的 webpack 类

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

```shell
$ node bundle.js
# 输出
```





## webpack 对 entry 的解析

### 得到入口模块路径

```js
// lib/webpack.js 中的 webpack 类

module.exports = class webpack {

    constructor(options) {
        // 得到入口配置、出口配置
        // 将其挂载到 this 下
        const { entry, output } = options;
        this.entry = entry;
        this.output = output;
    }

    // 入口函数
    run() {
        this.parse(this.entry);
    }

    // 入口文件解析
    parse(entryFile) {
        console.log(entryFile);
    }
}
```

```shell
$ node bundle.js
# 输出入口模块的路径
```



### 解析入口文件内容

#### **拿到入口文件内容**

```shell
# lib/webpack.js 中的 webpack 类下的 parse 方法
```

```js
const fs = require('fs');

// 入口文件解析
parse(entryFile) {
    const content = fs.readFileSync(entryFile, "utf-8");
    console.log(content);
}
```

```shell
$ node bundle.js
# 输出入口模块内容
```



#### 解析入口文件中的内容

**通过 @babel/parser 将文件内容 `静态解析（解析时不会执行代码）` 成 AST(抽象语法树)**

```shell
# 安装 @babel/parser
$ npm i @babel/parser -D
```

```shell
# lib/webpack.js 中的 webpack 类下的 parse 方法
```

```js
const parser = require('@babel/parser');

// 入口文件解析
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



#### 解析入口文件中的依赖路径

**通过 @babel/traverse 来对 babel/parser 转化的 AST 进行过滤处理，获取依赖文件路径**

> 当解析的文件中引入了其他依赖时，如何在 ast 中获得依赖文件的路径？

```shell
# 安装
$ npm i @babel/traverse -D
```

```shell
# lib/webpack.js 中的 webpack 类下的 parse 方法
```

```js
const path = require('path');
const traverse = require('@babel/traverse').default;


// 入口文件解析
parse(entryFile) {
    
  	// ......
  
    // 用来保存依赖文件地址的对应关系
    // 相对于入口文件的地址 ：相对于项目根目录的地址
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



#### 对解析后的入口文件的内容进行编译

**通过 `@babel/core` 下的 `babel/preset-env`  对  `AST`  进行编译处理**

```shell
# 安装 @babel/core
$ npm install @babel/core -D

# 安装  @babel/preset-env
$ npm install @babel/preset-env -D
```

```shell
# lib/webpack.js 中的 webpack 类下的 
```

```js
const { transformFromAst } = require("@babel/core");

run() {
    const info = this.parse(this.entry);

    // 文件解析生成的对象
    console.log(info);
}

// 文件解析
parse(entryFile) {

    // ......

    // 通过 @babel/core 下的 babel/preset-env  对 AST 进行编译处理 
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

```shell
$ node bundle.js
# 输出入口文件解析生成的对象
```



#### 对入口文件的依赖处理

**入口文件中引入的文件以及文件内容进行处理**

```shell
# lib/webpack.js 中的 webpack 类下的 
```

```js
constructor(options) {

    // ......

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
    console.log(this.module);
}
```

```shell
$ node bundle.js
# 输出解析后的所有的文件对象
```



**文件路径和文件的内容转换成 key：value 的形式**

```shell
# lib/webpack.js 中的 webpack 类下的 run 函数
```

```js
run(){
	
  // ......
  
  // 格式转换为对象
	const obj = {};
	this.module.forEach(item => {
    obj[item.entryFile] = {
        yilai: item.yilai,
        code: item.code,
    }
	});
	// console.log(obj);
}
```



## webpack 对  output 解析

### 生成 index.js 到 dist 目录

```js
run() {
  
  //...
  
  this.file(obj);

}


/**
 * 生成 bundle 文件
 * 
 * 1 - 生成 index.js 到 dist 目录
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

    // bundle 文件内容
    const bundle = `()(${newCode})`

    // 根据配置将 bundle 文件写入到 dist
    fs.writeFileSync(filePath, bundle, "utf-8");
}
```



### 生成 bundle 内容

bundle 文件内容，是一个自执行函数。接收的参数为编译后的 `{文件路径：文件内容}`

```js
const bundle = `()(${newCode})`
```



补充自执行函数

```js
const bundle = `(function(modules){
    
})(${newCode})`
```



自执行函数中添加缺失的 require 函数，

自执行函数中添加一个 load 函数，从 webpack.config.js 的入口配置中开始读取文件内容。

```js
const bundle = `(function(modules){
    
    function load(entryModule){

    }
    load('${this.entry}')

})(${newCode})`
```



load 函数中添加一个自执行函数，使用 eval 对当前模块的内容进行处理

```js
const bundle = `(function(modules){
    
    function load(entryModule){

        (function(code){
            eval(code)
        })(modules[entryModule].code)

    }
    load('${this.entry}')

})(${newCode})`
```



**对于打包后的内容中的依赖引入 require 函数的处理**

```shell
因为在我们使用 import 依赖其它文件时，打包后会将 import 转成 require。

但是在我们在打包后的文件中并没有发现 require 函数的定义，所以在使用打包内容时，会发现因为缺失了 require 方法无法使用。

所以需要在 bundle 文件生成时，定义 require 方法
```

因为当前模块中可能会依赖于其它文件，所以当前模块中编译后的内容会通过 require 来依赖其它文件，`需要将文件内容的 require 函数补充上`，相对路径作为参数调用 `pathRequire 函数` 就会得到一个相当于项目根目录的地址，根据地址获取到对应文件的内容，然后 return。

因为文件中的依赖地址存储的是相对路径 (相对于入口文件的地址，参考入口解析时对于依赖的处理，映射关系存储在 yilai 对象里)，所以需要转换成相对于项目根目录的地址。

也就是说我们需要一个方法将 `./a.js` 处理成 `./src/a.js`，因为我们在处理依赖关系时，在 yilai 中做了一个全局路径和相对路径的映射关系。

```js
const bundle = `(function(modules){
    
    function load(entryModule){

       function pathRequire(relativePath){
            return load(modules[entryModule].yilai[relativePath])
        }

        (function(require,code){
            eval(code)
        })(pathRequire,modules[entryModule].code)

    }
    load('${this.entry}')

})(${newCode})`
```



**对于打包后的内容中的依赖导出 exports 的处理**

```shell
因为在我们使用 export 导出时

在我们在打包后的文件中并没有发现 export 的定义，但是在文件内容中，有些通过 export 导出的内容需要挂载到 export 对象上，

所以需要在 bundle 文件生成时，定义 export 对象
```



```js
const bundle = `(function(modules){

    function load(entryModule){

        function pathRequire(relativePath){
            return load(modules[entryModule].yilai[relativePath])
        }

        const exports = {};

        (function(require,code){
            eval(code)
        })(pathRequire,modules[entryModule].code)

        return exports;
    }
    load('${this.entry}')

})(${newCode})`
```

```shell
# 执行构建
$ node bundle.js

# 将构建好的 dist/index.js 的内容复制、粘贴到浏览器的控制台
# 输出结果为 ' hello 这是a.js文件 这是b.js文件 '
```



