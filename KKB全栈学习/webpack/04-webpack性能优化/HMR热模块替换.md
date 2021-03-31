HMR热模块替换 --- 不需要新浏览器，保存之后自动改变

+ 替换对象

  + css - 不需要新浏览器，保存之后自动改变

    不支持 抽离出来的css，所以要使用 `style-loader`

    ```js
    // 使用webpack 内置插件 
    const { HotModuleReplacementPlugin } = require('webpack');
    plugins: [ new HotModuleReplacementPlugin() ] 
    ```
  
    ```js
    // devServer中开启 hot：true
  devServer: {
        hot: true
  }
    ```

    
  
  + js - 保存之后，会刷新浏览器
  
    ```js
    // 让浏览器不自动刷新
    devServer下开启 hotOnly：true 关闭刷新浏览器
    
    // 如何实现修改哪个模块更新哪个模块（实现热替换）
  if( module.hot ){
      module.hot.accept('监控的文件',()=>{
        // 模块一旦发生改变，就会进行回调
        // 获取当前文件在 dom 中的位置，删除当前 Dom，重新生成一下
      })
    }
    ```
    
    ```js
    // 每个模块都需要监听不够人性化，那么该怎么处理
    
    // 浏览器和服务直间是通过 wds 来进行通信的
    
    // 开启 HMR 之后，每个模块都会有一个对应 ID
    ```




babel - js的编译器

+ 打补丁： polyfill --> 使低版本浏览器能认识到许多新的特性，比如 promise

  + 安装在生产环境 npm i polyfill  -S

  + core-js: 特性xxxxx

  + .babekrc 文件
  
    ```js
    可以将 babel下的 options 放入其中
    ```



-D : 依赖安装到开发环境，例如 sorce-map

-S : 依赖安装到生产环境，部署到服务上使用




##  配置 react 打包环境

+ 支持 jsx

  使用 Presets --> npm install @babel/preset-react




## 自己编写 plugin

+ plugin 是通过 new 使用的，证明她是一个类

  在webpack构建的某个阶段去使用

+ web构建过程分为多个阶段（多个生命周期 | 钩子）

+ 插件基本结构

  ```js
  // apply -- 插件的运行方法
  // compiler -- webpack 对象 -- 可以提供 webpack 钩子
  ```

+ webpack基本流程

![1606831970771](C:\Users\Amd\AppData\Roaming\Typora\typora-user-images\1606831970771.png)

+ Compiler

  ![1606832005945](C:\Users\Amd\AppData\Roaming\Typora\typora-user-images\1606832005945.png)

  ![1606832022185](C:\Users\Amd\AppData\Roaming\Typora\typora-user-images\1606832022185.png)

  ![1606832032609](C:\Users\Amd\AppData\Roaming\Typora\typora-user-images\1606832032609.png)

+ 查看 webpack 钩子

  ```js
  // 根目录下/ index.js
  ```

+ 同步钩子，异步钩子

  hooks下同步的钩子通过 top（）触发

  hooks下异步的钩子通过 topAsync（）触发