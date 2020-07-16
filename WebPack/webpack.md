[参考地址](https://www.jianshu.com/p/5b69a7e61fe4)



## 什么是 WebPack

WebPack可以看做是**模块打包机**：它做的事情是，分析你的项目结构，找到JavaScript模块以及其它的一些浏览器不能直接运行的拓展语言（Scss，TypeScript等），并将其转换和打包为合适的格式供浏览器使用。



## 为什要使用WebPack

现今的很多网页其实可以看做是功能丰富的应用，它们拥有着复杂的JavaScript代码和一大堆依赖包。为了简化开发的复杂度，前端社区涌现出了很多好的实践方法

- **模块化**，让我们可以把复杂的程序细化为小的文件;
- 类似于TypeScript这种在JavaScript基础上拓展的开发语言：使我们能够实现目前版本的JavaScript不能直接使用的特性，并且之后还能转换为JavaScript文件使浏览器可以识别；
- Scss，less等CSS预处理器
- ...





## WebPack和Grunt以及Gulp相比有什么特性





## 准备工作

```js
创建文件 `webpack_project`

使用 `npm init`  初始化当前文件夹, 生成 `package.json` 文件

// 安装 webpack
npm i webpack -g	# 全局安装 webpack

npm i webpack --save-dev		# 安装到你的项目目录

在 `webpack_project` 下创建 `src`文件夹、`static`
```



**目录结构如下：**

```js
|-- webpack_project	// 跟路径
	|-- src	// 用于存放开发文件的文件夹
	|-- static	// 静态资源文件夹
	|-- dist	// 用于存放打包编译后的文件
	|-- webpack.config.js	// webpack 配置文件
```



## 开始使用 WebPack

### 简单使用

+ webpack_project 下创建 index.html

  ```html
  <!DOCTYPE html>
  <html lang="zh-cn">
  
  <head>
      <meta charset="UTF-8">
      <title>webpack</title>
  </head>
  
  <body>
  
      <script src="dist/bundle.js"></script>
      
  </body>
  
  </html>
  ```

+ src 下创建 bar.js

  ```js
  export default function bar(){
      document.body.innerText = 'hello world'
  }
  ```

+ src 下创建 mian.js 

  ```js
  import bar from './bar'
  
  bar();
  ```

+ 编译打包

  ```shell
  # 非全局安装的 webpack
  $ node_modules/.bin/webpack ./src/main.js -o ./dist/bundle.js
  ```



### 通过配置文件使用

#### 配置入口，出口文件

+ 当前根目录新建文件 `webpack.config.js`

  ```js
  module.exports = {
      entry: __dirname + "/src/main.js", //已多次提及的唯一入口文件
      output: {
          path: __dirname + "/dist",  //打包后的文件存放的地方
          filename: "bundle.js" //打包后输出文件的文件名
      }
  }
  
  // __dirname 是 nodeJs 的一个全局变量，指向当前脚本所在目录
  ```

+ 运行 `webpack` 脚本

  ```shell
  $ webpack		# 全局环境下安装的 webpack
  $ node_modules/.bin/webpack		# 当前项目安装
  ```

+ 在运行后会发现报错

  ```js
  WARNING in configuration
  The 'mode' option has not been set, webpack will fallback to 'production' for this value. Set 'mode' option to 'development' or 'production' to enable defaults for each environment.
  You can also set it to 'none' to disable any default behavior. Learn more: https://webpack.js.org/configuration/mode/
  ```

  ```js
  // 解决上面报错的方法
  // 第一种： 在 entry: __dirname + xxx 上面添加一行
  mode:"development"
  
  // 第二种：在 package.json 中添加
  "scripts":{
  	"dev": "webpack --mode development",
  	"build": "webpack --mode production",
  }
  ```



#### 通过 npm 进行打包

+ 在 `package.json`  中添加

  ```js
  "scripts": {
    "start": "webpack"
  },
  ```

+ 运行命令

  ```shell
  $ npm start
  ```

"scripts" 会按照一定的顺序寻找指令对应的包并且运行。本地的`node_modules/.bin` 就在寻找的清单中。无论是全局安装的还是局部安装的都不需要写详细点路径。



#### 生成 source Maps (调试)

需要在 `webpack.config.js` 配置 `devtool`  属性

```js
// 在 entry: __dirname + xxx 上面添加一行
devtool: 'eval-source-map',
```

| devtool属性                  | 功能描述                                                     |
| ---------------------------- | ------------------------------------------------------------ |
| source-map                   | 在一个单独的文件中产生一个完整且功能完全的文件。<br>这个文件具有最好的source map，但是它会减慢打包速度 |
| cheap-module-source-map      | 在一个单独的文件中生成一个不带列映射的map，<br/>不带列映射提高了打包速度，<br/>但是也使得浏览器开发者工具只能对应到具体的行，<br/>不能对应到具体的列（符号），会对调试造成不便； |
| eval-source-map              | 打包源文件模块，在同一个文件中生成干净的完整的source map  。<br/>这个选项可以在不影响构建速度的前提下生成完整的sourcemap  ，<br/>但是对打包后输出的JS文件的执行具有性能和安全的隐患。<br/>在开发阶段这是一个非常好的选项，在生产阶段则一定不要启用这个选项 |
| cheap-module-eval-source-map | 这是在打包文件时最快的生成source map的方法，<br/>生成的Source Map会和打包后的 JavaScript 文件同行显示，<br/>没有列映射，和 eval-source-map 选项具有相似的缺点 |



#### 构建本地服务

```shell
# 安装包
$ npm install webpack-dev-server	

# 需要在 webpack.config.js 中配置 devServer 属性
devServer:{
    contentBase: 'index.html',   # 本地服务加载页面所在的目录 或者文件
    historyApiFallback: true,    # 路由跳转
    inline: true,   # 实时刷新
}

# 需要在 package.json 中添加 dev 属性
"scripts": {
  "start": "webpack",
  "dev": "webpack-dev-server --open"
},
```

```shell
$ npm run dev		# 启动本地服务
```



| devServer配置属性  | 功能描述                                                     |
| ------------------ | ------------------------------------------------------------ |
| contentBase        | 默认是为根文件提供本地服务，想要为某个文件夹提供服务需要将值设置为文件夹路径 |
| port               | 监听端口，默认问 8080                                        |
| inline             | 值为`true`时，源文件改变会自动刷新页面                       |
| historyApiFallback | 在开发单页应用时非常有用，它依赖于HTML5 history API，如果设置为`true`，所有的跳转将指向index.html |



#### Loaders

loaders 是 webpack 核心功能之一，通过使用不同的 loaders， webpack 有能力调用外部的