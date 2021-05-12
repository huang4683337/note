## 安装

### 初始化一个 npm 仓库

```shell
# 初始化一个npm仓库
$ npm init -y
```

### 全局安装

全局安装：不推荐

```shell
# 安装webpack V4+版本时，需要额外安装webpack-cli
$ npm install webpack webpack-cli -g

# 检查全局安装版本
$ webpack -v

# 卸载 webpack、webpack-cli
$ npm uninstall webpack webpack-cli -g
```

不推荐的原因：全局安装 `webpack`，这会将你项⽬中的 `webpack` 锁定到指定版本，造成不同的项⽬中因为 `webpack` 依赖不同版本⽽导致冲突，构建失败





### 局部安装

局部安装（项目安装）：推荐

```shell
# 安装最新的稳定版本
$ npm i -D webpack

# 安装指定版本
$ npm i -D webpack@<version>
$ npm i -D webpack@4.44.1

# 安装webpack V4+版本时，需要额外安装webpack-cli
$ npm i -D webpack-cli@3.3.12
```

是否安装成功

```shell
# 查看全局安装版本
$ webpack -v

# 报错：webpck : 无法将“webpck”项识别为 cmdlet、函数、脚本文件或可运行程序的名称。请检查名称的拼写，如果包括路径，请确保路径正确，然后再试一次。


# 默认在全局环境中查找, 我们是在局部安装，所以会报错
```

```shell
# 查看局部安装版本

# 方法1
$ npx webpack -v
# npx帮助我们在项⽬中的node_modules/.bin ⾥查找webpack,
# 存在就执行，不存在则自动下载 webpack

# 方法2
$ ./node_modules/.bin/webpack -v
# 到当前的node_modules模块⾥指定webpack
```



## 执行构建

### 准备构建

```js
// 根目录下新建src目录 src/index.js
console.log(' hello webpack ')
```



### 执行构建

```shell
# npm 自带执行命令
$ npx webpack
```

```shell
# package.json 中配置

"scripts": {
 "build": "webpack"
},


# 构建命令
$ npm run build
```

```js
"scripts": {
 "build": "webpack --config ./webpack.xxx.config.js"
}

// 可以通过添加 --config 来告诉 webpack 使用哪个配置文件
```



### 构建结果

```js
// 根目录下出现 dist 目录
// dist/main.js
```

```js
// 控制台出现警告
WARNING in configuration
The 'mode' option has not been set, webpack will fallback to 'production' for this value. Set 'mode' option to 'development' or 'production' to enable defaults for each environment.
You can also set it to 'none' to disable any default behavior. Learn more: 
https://webpack.js.org/configuration/mode/


// 解决方式 - 根目录新建 webpack.config.js
const path = require("path");
module.exports = {
    mode: "development",
};

// 执行构建 - 警告消失
```



## webpack 默认配置

`webpack.config.js` 不存在时，实际上执行的就是以下的配置

```js
//webpack的配置文件
const path = require("path");
module.exports = {

  entry: "./src/index.js",	//entry:入口
  
  //output:出口
  output: {
    //指定输入资源的存放目录，位置
    //必须是绝对路径
    path: path.resolve(__dirname, "./dist"),
    filename: "main.js",
  },
  //mode
  mode: "development",
};
```

实际开发中需要自定义配置时，就在 `webpack.config.js`  中配置





## webpack.config.js 基本属性



### entry

指定 `webpack` 打包⼊⼝⽂件 `:Webpack`  执⾏构建的第⼀步将从 `Entry `开始，可抽象成输⼊

```js
//单⼊⼝ SPA，本质是个字符串
entry:{
 main: './src/index.js'
}
// ==相当于简写==
entry:"./src/index.js"



//多⼊⼝ entry是个对象
entry:{
 index:"./src/index.js",
 login:"./src/login.js"
}
```



### output

打包转换后的⽂件输出到磁盘位置:输出结果，在 `Webpack` 经过⼀系列处理并得出最终想要的代码后输出结果。

```js
output: {
 filename: "bundle.js",//输出⽂件的名称
 path: path.resolve(__dirname, "dist")//输出⽂件到磁盘的⽬录，必须是绝对路径
},

//多⼊⼝的处理
output: {
 filename: "[name][chunkhash:8].js",//利⽤占位符，⽂件名称不要重复
 path: path.resolve(__dirname, "dist")//输出⽂件到磁盘的⽬录，必须是绝对路径
},
```



### mode

查看官网



### loader

[参考地址](https://v4.webpack.docschina.org/loaders/)

**加载器：用于解析各种类型的文件**

loader的作用：模块解析，模块转换器，⽤于把模块原内容按照需求转换成新内容。

`webpack` 是模块打包⼯具，⽽模块不仅仅是 `js` ，还可以是 `css` ，图⽚或者其他格式。

默认情况下 `webpack` 只会处理 `.json` 、`.js` 文件。那么其他格式的模块处理，和处理⽅式就需要
`loader`了

`loader` 的作用就是让 `webpack` 知道怎么处理其它后缀的文件，相当于翻译官 `.css .less .sass .....`



#### moudle

模块，在 `Webpack` ⾥⼀切皆模块，⼀个模块对应着⼀个⽂件。

`Webpack` 会从配置的 `Entry` 开始递归找出所有依赖的模块。

当 `webpack`处理到不认识的模块时，需要在 `webpack` 中的 `module`处进⾏配置，当检测到是什么格式的模块，使⽤什么 `loader` 来处理。

```js
// webpack.config.js 中添加
module: {
  rules: [
    {
      test: /\.xxx$/,//指定匹配规则
      use: {
        loader: 'xxx-load'//指定使⽤的loader
      }
    }
  ]
},
```



#### 处理样式文件

+ 使用 `css`

  ```js
  // src/index.css
  body{
      background-color: red;
  }
  
  // src/index.js 中引入
  import css from './index.css'
  
  // 控制台输入，执行构建
  npm run build
  // 报错： Can't resolve 'css-loader'
  ```

+ 安装配置 `css-loader`

  ```shell
  # 安装 
  $ npm install css-loader -S-D
  
  # 使用
  # webpack.config.js 中添加
  module: {
    rules: [
      {
        test: /\.css$/,//指定匹配规则
        use: {
          loader: 'css-loader'//指定使⽤的loader
        }
      }
    ]
  },
  
  # 控制台输入，执行构建
  $ npm run build
  # 不报错
  ```
  
  ```js
  // dist/main.js --> 搜索 background
  // 发现 css 编译到 main.js 中了
  ```

+ 查看是否成功

  ```html
  <!-- dist 下新建 index.html -->
  <script src="./main.js"></script>
  
  <!-- 浏览器打开 index.html -->
  ```

**没有看到整个页面变成红色，F12 查看也没有对应样式，竟然不胜生效 ！！！！**

+ 安装配置 `style-loader`

  ```shell
  $ npm install style-loader -D-S 
  
  module: {
    rules: [
      {
        test: /\.css$/,//指定匹配规则
        use: {
          loader: ['style-loader','css-loader']
        }
      }
    ]
  },
  ```



> 总结：
>
> ​	loader 执行顺序：从右到左、从下到上
>
> ​	css-loader：帮助 webpack 将 css 序列化、
>
> ​	stye -loader：将序列化的 css ，放入动态创建的 style 标签中



#### less、sass文件的处理

less-loader / sass-loader： 先将 less / sass 转化成 css

css-loader：帮助 webpack 将 css 序列化、

stye -loader：将序列化的 css ，放入动态创建的 style 标签中

```js
module: {
  rules: [
    {
      test: /\.css$/,//指定匹配规则
      use: ['style-loader', 'css-loader','less-loader']
    }
  ]
},
```



### plugin

对于 `webpack` 功能的补充

```txt
比如：

	第一次打包，在dist生成main.js，

	第二次打包，我们修改webpack配置，想要生成index.js

	这时我们发现，dist下同时存在 main.js、index.js，
	我们期望只有 index.js的存在，但是 webpack并没有此功能，
	所以我们需要一个插件来自动删除上一次打包的结果。 这就是plugins的作用  
```



#### HtmlWebpackPlugin

htmlwebpackplugin会在打包结束后，`通过定义到模板`⾃动⽣成⼀个html⽂件，并把打包⽣成的js模块引⼊到该html中。

```shell
# 安装 HtmlWebpackPlugin
$ npm install  html-webpack-plugin@4.5.0 -D
```

```js
// webpack.config.js 中
const HtmlWebpackPlugin = require('html-webpack-plugin')
plugins: [
  new HtmlWebpackPlugin()
]
```

+ 配置

  ```js
  title: ⽤来⽣成⻚⾯的 title 元素
  filename: 输出的 HTML ⽂件名，默认是 index.html, 也可以直接配置带有⼦⽬录。
  template: 模板⽂件路径，⽀持加载器，⽐如 html!./index.html
  inject: true | 'head' | 'body' | false ,注⼊所有的资源到特定的 template 或者
  templateContent 中，如果设置为 true 或者 body，所有的 javascript 资源将被放置到 body
  元素的底部，'head' 将放置到 head 元素中。
  favicon: 添加特定的 favicon 路径到输出的 HTML ⽂件中。
  minify: {} | false , 传递 html-minifier 选项给 minify 输出
  hash: true | false, 如果为 true, 将添加⼀个唯⼀的 webpack 编译 hash 到所有包含的脚本和
  CSS ⽂件，对于解除 cache 很有⽤。
  cache: true | false，如果为 true, 这是默认值，仅仅在⽂件修改之后才会发布⽂件。
  showErrors: true | false, 如果为 true, 这是默认值，错误信息会写⼊到 HTML ⻚⾯中
  chunks: 允许只添加某些块 (⽐如，仅仅 unit test 块)
  chunksSortMode: 允许控制块在添加到⻚⾯之前的排序⽅式，⽀持的值：'none' | 'default' |
  {function}-default:'auto'
  excludeChunks: 允许跳过某些块，(⽐如，跳过单元测试的块)
  ```



+ HtmlWebpackPlugin 配置多入口文件打包时，可以通过chunks配置当前文件自动引入那些css

  ```js
  // a.js
  import css from './a.css'
  
  // a.css
  body div{
      border: 3px solid blue;
  }
  
  // index.js
  import css from './index.css'
  
  // index.css
  body{
      background-color: red;
  }
  
  
  // webpack.config.js
  plugins: [
    new CleanWebpackPlugin(),
    new HtmlWebpackPlugin({
      title: "My App",
      filename: "app.html",
      template: "./src/index.html",
      // chunks: ['index']
    }),
    new HtmlWebpackPlugin({
      title: "a",
      filename: "a.html",
      template: "./src/a.html",
      // chunks: ['a']
    })
  ]
  
  // a.html 和 index.html 中同时引入了 a.css、index.css
  
  // 去掉 chunks 注释
  // a.html 只引入了 a.css
  // index.html 只引入了 index.css
  ```





#### clean-webpack-plugin

每次构建之前会清空 dist 目录，避免我们手动删除

```shell
$ npm install  clean-webpack-plugin@3.0.0 -D
```

```js
const { CleanWebpackPlugin } = require("clean-webpack-plugin");
...
plugins: [
 new CleanWebpackPlugin()
]
```



### chunk

与入口有关，根据入口加载相关的依赖，编译处理后的结果就是一个chunk



1个bundle 对应一个 chunk

1个chunk对应一个或者多个module
