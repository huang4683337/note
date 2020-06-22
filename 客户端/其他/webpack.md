[参考地址](https://www.jianshu.com/p/42e11515c10f)



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
```



## 开始使用 WebPack

