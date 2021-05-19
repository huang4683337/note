## 分析

根据以往使用的 webpack 插件分析

+ plugin 是通过 new 使用的，证明它是一个类

+ plugin 在 webpack 构建的某个阶段去使用

  ```shell
  webpack 在打包前会读取 webpack.config.js 中的配置，从而走完整个构建过程
  ```

+ 在 webpack 构建过程会分为多个阶段（多个生命周期 | 钩子）

+ 插件的基本结构

  ```js
  // 插件名称
  class MyPlugin {
      constructor(options) { }
      // 插件运行方法 apply
      apply(compiler) {
          // 插件 hooks
          compiler.hooks.done.tap('My Plugin', (/* xxxx */) => {
              // 插件处理逻辑
          })
      }
  }
  ```

  ```shell
  # apply -- 插件的运行方法
  # compiler -- webpack 对象 -- 可以提供 webpack 钩子
  # 通过 hooks 提供了很多钩子 
  # done 代表某个阶段
  # tap	在当前阶段注册一个事件
  ```



## webpack 基本流程

webpack 基本流程可以分为 3 个阶段：

+ 准备阶段：主要任务是创建 `Compiler` 和 `Compilation` 对象

+ 编译阶段：这个阶段任务是完成  `modules` 解析，并且生成 `chunks`

  + `module` 解析：包含三个主要步骤（创建实例、loaders应用、依赖收集）

  + `chunks` 生成：主要步骤是找到每个 `chunks` 锁需要包函的 `modules` 

+ 产出阶段：这个阶段的主要任务是根据 `chunks` 生成最终文件，主要有三个步骤（模板 Hash 更新、模板渲染  chunk、生成文件）



## Compiler

**什么是 Compiler **

`Compiler` 模块是 webpack 最核心的模块。每次执行 webpack 构建的时候，在 webpack 内部，会首先实例化一个 `Compiler` 对象，然后调用它的 run 方法来开始一个完整的编译过程。



**得到 Compiler 对象**

我们直接使用 webpack API `webpack(options)` 的方式得到的就是一个 `Compiler` 实例化的对象。

这时候 webpack 并不会立即开始构建，需要我们手动执行 `compiler.run()` 才可以。

```js
// src/index.js

const webpack = require('webpack');
const webpackConfig = require('../webpack.config.js');

// 只传入 config
const compiler = webpack(webpackConfig);

// 开始执行
compiler.run();
```

```js
const compiler = webpack(webpackConfig);
compiler.run(callback);

// 上面两句等价于
webpack(webpackConfig, callback);
```

> Tips: 使用 webpack-dev-server API 方式是，只需要传入 compiler 对象给 dev server 即可，不需要手动执行 `compiler.run()`

我们如果要手动实例化一个 `Cpmpiler` 对象，可以通过`const Compiler = webpack.Compiler` 来获取它的类，一般只有一个父 `Compiler`，而子 `Compiler` 可以用来处理一些特殊的事件。

在 webpack plugin 中，每个插件都有一个 `apply` 方法。这个方法接受收到的参数就是 `Compiler` 对象，我们可以通过在对应的钩子触发时绑定处理函数来编写插件，下面主要介绍 `Compiler` 对象的钩子





## Compiler 钩子

**查看 webpack 中 compiler 的所有钩子**

在 [weboack](https://webpack.docschina.org/api/compiler-hooks/)工作流程中，我们通过下面的代码获取对应钩子的名称：

```shell
# 进入官网 ==> 最上面菜单选择中文 ==> 菜单选择中文文档 ==> API ==> 左侧菜单选中Plugins ==> compiler 钩子
```

```js
// src/index.js 文件中

const webpack = require('webpack');
const webpackConfig = require('../webpack.config.js');

// 只传入 config
const compiler = webpack(webpackConfig);

// 开始执行，触发 webpack 编译流程
compiler.run();

// 查看所有的钩子
Object.keys(compiler.hooks).forEach((hookName) => {
    // 在当前钩子上注册一个事件
    // 当前钩子触发时，会执行当前事件
    compiler.hooks[hookName].tap("anyEvent", () => {
        console.log(`run -----> ${hookName}`);
    })
})
```

```shell
# 执行文件
# 项目根目录下执行
$ node src/index.js
```



得到 `compiler.run()` 之后的工作流程

```
run -> brforeRun
run -> run
run -> normalModuleFactory
run -> contextModuleFactory
run -> beforeCompile
run -> compilation
run -> thisCompilation
run -> compilation
run -> make
run -> afterCompile
run -> shouldEmit
run -> emit
run -> afterEmit
run -> done
```



## Compiler Hooks

`Compiler` 编译器模块是创建编译实例的主引擎。大多数面向用户的插件都首先在 `Compiler` 上注册。

`Compiler` 上暴露的一些常用的钩子：

| 钩子         | 类型              | 什么时候调用                                                 |
| ------------ | ----------------- | ------------------------------------------------------------ |
| run          | AsyncSeriesHook   | 在编译器开始读取记录前执行                                   |
| compile      | SyncHook          | 在一个新的 compilation 创建之前执行                          |
| compilation  | SyncHook          | 在一次 compilation 创建后执行插件                            |
| make         | AsyncParallelHook | 完成一次编译之前执行                                         |
| emit         | AsyncSeriesHook   | 在生成文件到 output 目录之前执行，回调参数：`compilation`    |
| afterEmit    | AsyncSeriesHook   | 在生成文件到 output 目录之后执行                             |
| assetEmitted | AsyncSeriesHook   | 生成文件的时候执行，提供访问产出文件信息的入口，回调参数：`file`, `info` |
| done         | AsyncSeriesHook   | 一次编译完成后执行，回调参数：`stats`                        |



+ `AsyncSeriesHook` 异步钩子
+ `SyncHook` 同步钩子



## 案例

### 准备工作

**创建 txt-webpack-plugin.js**

```js
// src/myPlugns

/**
 * 在 dist 目录生成一个 txt 文件
 */

class TxtWebpackPlugin {
    constructor(options) {
        console.log(options);
    }

    //compiler：webpack实例
    apply(compiler) {
			// 
    }
}
module.exports = TxtWebpackPlugin;
```



**配置文件中使用 webpack.config.js**

```js
// dist 目录中生成一个 txt 文件
const TxtWebpackPlugin = require("./myPlugns/txt-webpack-plugin");


plugins: [
  // dist 目录中生成一个 txt 文件
  new TxtWebpackPlugin({
      name: "hello-plugin",
  })
]
```



**构建**

```shell
$ npm run build
# 控制台输出：{ name: 'hello-plugin' }
```



### 实现功能

**txt-webpack-plugin.js 中的 apply 方法**

```js
/**
 * 在 dist 目录生成一个 txt 文件
 */

class TxtWebpackPlugin {
    constructor(options) {
        // webpack.config.js 中插件的配置项
        // console.log(options);
    }

    apply(compiler) {
        // compilation: webpack 运行到 emit 钩子时
        // 代码（chunk）被处理成什么样子
        compiler.hooks.emit.tapAsync('TxtWebpackPlugin', (compilation, cb) => {
            // 将要输出到 dist 目录下的资源列表
            // console.log(compilation.assets);

            // 资源列表添加 test.txt
            compilation.assets["test.txt"] = {
                // test.txt 内容
                source: function () {
                    return "hello txt";
                },

                // test.txt 大小
                size: function () {
                    return 1024;
                }
            };

            // 异步处理完毕
            // 通知 webpack 可以执行后续了
            cb();
        })
    }
}
module.exports = TxtWebpackPlugin;
```



**构建**

```shell
$ npm run build
# dist 目录下出现了 test.txt 文件，内容为 hello txt
```



**注意**

```js
// 异步钩子注册事件

compiler.hooks.emit.tapAsync('TxtWebpackPlugin', (compilation, cb) => {
		// 插件处理
  	...
    
    // 异步处理完毕
    // 通知 webpack 可以执行后续了
    cb();
})
```

```js
// 同步钩子注册事件

compiler.hooks.compile.tap('xxx', (compilation) => { 
		// 插件处理
  	...
})
```

