## 资源

+ vue-cli 4.5+

  [地址](https://cli.vuejs.org/zh/guide/installation.html)

  想要使用vue3需要删掉原来的 vue-cli, 重新安装4.5+的vue-vli

+ [vite地址](https://github.com/vitejs/vitel)

+ [源码地址](https://github.com/vuejs/vue-next)

+ 文档

  [中文](https://vue3js.cn/docs/zh/)

  [中文直接文档](https://vue3js.cn/docs/zh/)
  
  [英文](https://v3.vuejs.org/)





## vue3快速开始

[官方中文文档](https://vue3js.cn/docs/zh/guide/installation.html#%E5%91%BD%E4%BB%A4%E8%A1%8C%E5%B7%A5%E5%85%B7-cli)

### CDN

```html
<script src="https://unpkg.com/vue@next"></script>
```



### vue-cli

还会支持vue2

升级 vue-cli 到 4.5+

```shell
# 查看 vue-cli 版本
$ vue -V

# 卸载以前的 vue-cli

# 安装最新版本 vue-cli
$ npm install -g @vue/cli@next
# OR
$ yarn global add @vue/cli@next
```



### vite

直接使用vue3 , 不支持 vue2

```shell
$ npm init vite-app <project-name>
$ cd <project-name>
$ npm install
$ npm run dev
```

或者

```shell
$ yarn create vite-app <project-name>
$ cd <project-name>
$ yarn
$ yarn dev
```





## 调试环境搭建

+ 迁出 vue3 源码 

  [源码地址](https://github.com/vuejs/vue-next.git)

  如果嫌下载慢，可以将源码迁出到自己的 gitee 中

  ```shell
  # 源码迁入到 gitee
  
  # 登录 gitee
  # 右上角 + 
  # 从 GitHub/GitLab 导入仓库
  # 导入完成
  # git clone ...
  ```

+ 安装依赖

  ```shell
  $ yarn --ignore-scripts
  # OR
  $ npm i --ignore-scripts
  
  # 报错先别管，只要 node_modules 出现
  # npm run dev 不报错 就行
  ```

+ 生成 `sourcemap` 文件

  ```shell
  # 修改 package.json
  
  # "dev": "node scripts/dev.js --sourcemap"
  ```

+ 编译

  ```shell
  $ yarn dev
  # OR
  $ npm run dev
  ```
  
  > ⽣成结果：
  > packages\vue\dist\vue.global.js
  > packages\vue\dist\vue.global.js.map
  
  ```shell
  # 找到： packages\vue\examples\composition\todomvc.html
  # 浏览器打开
  # 93 行打断点
  # 验证是否成功
  ```
  
  ```shell
  # 后续调试测试可以仿照 todomvc.html 来写
  ```
  
  
  
  
  
  
  
  ## 需要重点关注的核心代码
  
  ![1610901701345](C:\Users\Amd\AppData\Roaming\Typora\typora-user-images\1610901701345.png)
  
  

+ 编译器
  + compiler-core 核⼼编译逻辑
  + compiler-dom 针对浏览器平台编译逻辑
  + compiler-sfc 针对单⽂件组件编译逻辑
  + compiler-ssr 针对服务端渲染编译逻辑
+ 运⾏时环境
  + runtime-core 运⾏时核⼼
  + runtime-dom 运⾏时针对浏览器的逻辑
  + runtime-test 浏览器外完成测试环境仿真
+ reactivity 响应式逻辑
+ template-explorer 模板浏览器
+ vue 代码⼊⼝，整合编译器和运⾏时
+ server-renderer 服务器端渲染
+ share 公⽤⽅法





