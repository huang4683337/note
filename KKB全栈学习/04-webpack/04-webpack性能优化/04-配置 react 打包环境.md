## **安装**

```shell
# 安装
$ npm install react react-dom --save
```



## **测试代码**

```react
// src/index.js
import React, { Component } from "react";
import ReactDom from "react-dom";
class App extends Component {
    render() {
        return <div>hello world</div>;
    }
}
ReactDom.render(<App />, document.getElementById("app"));
```



## **安装babel与react转换的插件**

```shell
# @babel/preset-react 解析 jsx
$ npm install  @babel/preset-react --save-dev
```



## **配置 .babelrc**

```json
{
    "presets": [
        [
            "@babel/preset-env",
            {
                "targets": {
                    "edge": "17",
                    "firefox": "60",
                    "chrome": "67",
                    "safari": "11.1"
                },
                "useBuiltIns": "usage"
            }
        ],
        "@babel/preset-react"
    ]
}
```



## **构建**

```shell
$ npm run dev
# 打开浏览器
# 发现我们模板 html 中的 '举头望明月' 替换为 'hello world'
```



## npm 安装 -D -S

```shell
-D | --dev : 开发环境，只有在本地服务才能使用
-S | --save : 生产环境，会被打包到 build 文件中，在远程服务上也能使用下载的 node_modules 的包
```

