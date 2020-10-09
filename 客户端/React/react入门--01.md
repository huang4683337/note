[React官网](https://zh-hans.reactjs.org/)

[create-react-app](https://create-react-app.dev/docs/getting-started/)



## 使用  create-react-app

```shell
# 创建项⽬
$ npx create-react-app my-app

# 打开项⽬
$ cd my-app

# 启动项⽬
$ npm start

# 暴露配置项 - 此操作不可逆
$ npm run eject
```



## cra 创建项目的文件结构

```js
├── README.md 	⽂档
├── public 静态资源
│ ├── favicon.ico
│ ├── index.html
│ └── manifest.json
└── src 源码
 ├── App.css
 ├── App.js 根组件
 ├── App.test.js
 ├── index.css 全局样式
 ├── index.js ⼊⼝⽂件
 ├── logo.svg
 └── serviceWorker.js pwa⽀持
├── package.json npm 依赖
```



## 入口文件

⼊⼝⽂件定义，webpack.config.js

```js
entry: [
	// WebpackDevServer客户端，它实现开发时热更新功能
	isEnvDevelopment &&
		require.resolve('react-dev- utils/webpackHotDevClient'),
	// 应⽤程序⼊⼝：src/index
	paths.appIndexJs,
].filter(Boolean),
```

webpack.config.js 是webpack配置⽂件，开头的常量声明可以看出cra能够⽀持ts、sass及css模块化。

```js
// Check if TypeScript is setup
const useTypeScript = fs.existsSync(paths.appTsConfig);
// style files regexes
const cssRegex = /\.css$/;
const cssModuleRegex = /\.module\.css$/;
const sassRegex = /\.(scss|sass)$/;
const sassModuleRegex = /\.module\.(scss|sass)$/;
```



## React和ReactDom

删除src下⾯所有代码，新建index.js

```react
import React from 'react';
import ReactDOM from 'react-dom';
// 这⾥怎么没有出现React字眼？
// JSX => React.createElement(...)
ReactDOM.render(<h1>Hello React</h1>, document.querySelector('#root'));
```

>React 负责逻辑控制，数据 ==> VDOM
>ReactDom 渲染实际 DOM，VDOM ==> DOM
>React 使⽤ JSX 来描述 UI
>babel-loader 把 JSX 编译成相应的 JS 对象，React.createElement 再把这个 JS 对象构造成 React 需
>要的虚拟 dom。

