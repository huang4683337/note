# vue-cli 3 安装配置

## 1、vue-cli 3 初始化

```shell
$ npm install -g @vue/cli

或者

$ yarn global add @vue/cli
```

> 查看 vue-cle 版本  vue --version 或者 vue -V

==注意：==

- vue-cli 3 的包的名称从 `vue-cli`  改为 `@vue/cli` , 如果安装过3以下的版本你先通过`npm uninstall vue-cli -g`或 `yarn global remove vue-cli`卸载它
- vue-cli 3 需要 `node8.9`以上的版本

## 2、创建项目

### 2-1  使用`vue ui`图形化界面

> 请自行查看[cli.vuejs.org/](https://juejin.im/post/5c63afd56fb9a049b41cf5f4)，

###2-2、 命令创建

```bash
$ vue create vuedemo


Babel : 将ES6编译成ES5
TypeScript: javascript类型的超集
Progressive Web App (PWA) Support: 支持渐进式的网页应用程序
Router:vue-router
Vuex: 状态管理
CSS Pre-processors: CSS预处理
Linter / Formatter: 开发规范
Unit Testing: 单元测试
E2E Testing: 端到端测试
```



