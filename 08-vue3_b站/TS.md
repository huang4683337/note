## 安装 TS

```shell
# 安装 TS
$ npm i -g typescript

# ts 是否安装成功
$ tsc -V
# Version 4.5.4 证明安装成功

# 编译 Ts 文件
$ tsc 文件名

# 浏览器不能直接运行 ts 代码，所以 Ts 需要编译成 js 能在浏览器访问
```



## vscode 自动编译 ts 代码生成 js

```shell
# 1 - 根目录生成配置文件 tsconfig.json
$ tsc --init

# 2 - 修改 tsconfig.json 配置
"outDir": "./js"       # TS 文件编译后最终放入 js 目录中
"strict": "false"			 # 关闭严格模式

# 3 - 启动监视任务
点击 vscode 菜单上的终端 => 选择 运行任务 => 点击显示所有任务 => tsc: 监视 tsconfig.json 
```

```js
// 根目录新建 index.ts
// 修改 ts 代码， 保存
// 过一会 根目录下自动出现 js 文件夹，里面就是 ts 编译后的 js 文件 
```



## 类型注解

```
1 - 什么是类型注解
	是一种 轻量级、为函数或者变量添加约束

2 - 类型注解错误，也可以编译成 js 
```



## 接口 interface

```
1 - 应用于对象
2 - xx. 是会提示当前对象中的 key
3 - 赋值时，对象中必有字段没有时，会报错
```



## 类

```ts
(() => {

  // 定义一个接口
  interface Iperson {
    name: string,
    age: number
  }

  // 定义一个类
  class Person {
    name: string
    age: number
    all: string

    // 定义一个构造函数
    constructor(name: string, age: number) {
      this.name = name;
      this.age = age;
      this.all = this.name + '_' + this.age
    }
  }


  // 实例化对象
  const person = new Person('Tom', 18);

  // 定义一个函数, 参数类型为 Iperson
  function say(person: Iperson) {
    return person.name + '__' + person.age
  }


  console.log(say(person));

})()
```



## webpack 打包 ts

### 建文件夹

```shell
# 根目录为 webpack打包ts

# webpack打包ts 新建
- build
	- webpack.config.js
- public
	- index.html
- src
	- main.ts

# 初始化项目
$ npn init -y

# 生成 ts 配置文件
$ tsc --init
```



### 安装依赖

```shell
# 下载相关依赖
$ yarn add -D typescript
$ yarn add -D webpack@4.41.5 webpack-cli@3.3.10
$ yarn add -D webpack-dev-server@3.10.2
$ yarn add -D html-webpack-plugin clean-webpack-plugin
$ yarn add -D ts-loader
$ yarn add -D cross-env
```



### 入口文件： src/main.ts



### index页：public/index.html



## build/webpack.config.js



## 配置打包命令

```json
"dev": "cross-env NODE_ENV=development webpack-dev-serve --config build/webpack.config.js",
"build": "corss-env NODE_ENV=production webpack --config build/webpack.config.js"
```



## 运行与打包