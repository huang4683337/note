## 什么是 Babel

babel - js的编译器

- 打补丁： polyfill --> 使低版本浏览器能认识到许多新的特性，比如 promise

  - 安装在生产环境 npm i polyfill  -S

  - core-js: 特性xxxxx

  - .babekrc 文件

    ```js
    可以将 babel下的 options 放入其中
    ```



-D : 依赖安装到开发环境，例如 sorce-map

-S : 依赖安装到生产环境，部署到服务上使用



## 配置 react 打包环境

- 支持 jsx

  使用 Presets --> npm install @babel/preset-react



## 自己编写 plugin

- plugin 是通过 new 使用的，证明她是一个类

  在webpack构建的某个阶段去使用

- web构建过程分为多个阶段（多个生命周期 | 钩子）

- 插件基本结构

  ```js
  // apply -- 插件的运行方法
  // compiler -- webpack 对象 -- 可以提供 webpack 钩子
  ```

- webpack基本流程

![1606831970771](C:\Users\Amd\AppData\Roaming\Typora\typora-user-images\1606831970771.png)

- Compiler

  ![1606832005945](C:\Users\Amd\AppData\Roaming\Typora\typora-user-images\1606832005945.png)

  ![1606832022185](C:\Users\Amd\AppData\Roaming\Typora\typora-user-images\1606832022185.png)

  ![1606832032609](C:\Users\Amd\AppData\Roaming\Typora\typora-user-images\1606832032609.png)

- 查看 webpack 钩子

  ```js
  // 根目录下/ index.js
  ```

- 同步钩子，异步钩子

  hooks下同步的钩子通过 top（）触发

  hooks下异步的钩子通过 topAsync（）触发