## 同构开发 SSR 应用

对于同构开发，我们依然使⽤ `webpack` 打包，我们要解决两个问题：服务端⾸屏渲染和客户端激活。



### 构建流程

⽬标是⽣成⼀个「服务器 bundle」⽤于服务端⾸屏渲染，和⼀个「客户端bundle」⽤于客户端激活。

![同构ssr](F:\桌面\同构ssr.png)



### 代码结构

除了两个不同⼊⼝之外，其他结构和之前vue应⽤完全相同。

```shell
src
├── router
├────── index.js # 路由声明
├── store
├────── index.js # 全局状态
├── main.js # ⽤于创建vue实例
├── entry-client.js # 客户端⼊⼝，⽤于静态内容“激活”
└── entry-server.js # 服务端⼊⼝，⽤于⾸屏内容渲染
```



## 全局状态管理

`store.js`

到处一个方法，`createStore`，用于创建 `store`  的工厂实例。

+ 此处使用工厂模式创建多实例，为了多用户访问各自实例

```js
import Vue from "vue";
import Vuex from "vuex";

Vue.use(Vuex);

export function createStore() {
  return new Vuex.Store({
    state: {
      count: 0,
    },
    mutations: {
      init(state, count) {
        state.count = count;
      },
      add(state) {
        state.count += 1;
      },
    },
    actions: {
      // 加一个异步请求count的action
      getCount({ commit }) {
        return new Promise(resolve => {
          setTimeout(() => {
            commit("init", Math.random() * 100);
            resolve();
          }, 1000);
        });
      },
    },
  });
}

```



## 路由

`router.js`

```js
import Vue from 'vue'
import Router from 'vue-router'
import Home from './views/Home.vue'
import About from './views/About.vue'

Vue.use(Router)

// 返回一个工厂函数，它可以创建路由实例
export default function createRouter() {
  return new Router({
    mode: 'history',
    routes: [
      {
        path: '/',
        name: 'home',
        component: Home
      },
      {
        path: '/about',
        name: 'about',
        component: About
      }
    ]
  })
}
```



## 主文件

`main.js`

跟之前不同，主⽂件是负责创建 `vue` 实例的⼯⼚，每次请求均会有独⽴的 `vue` 实例创建。创建 `main.js`

```js
import Vue from "vue";
import App from "./App.vue";
import createRouter from "./router";
import { createStore } from './store'

Vue.config.productionTip = false;

// 加一个全局混入，处理客户端asyncData调用
Vue.mixin({
  beforeMount() {
    const {asyncData} = this.$options
    if (asyncData) {
      asyncData({
        store: this.$store,
        route: this.$route
      })
    }
  }
})

// 需要返回一个应用程序工厂: 返回Vue实例和Router实例、Store实例
export default function createApp(context) {
  // 处理首屏，就要先处理路由跳转
  const router = createRouter()
  const store = createStore()
  const app = new Vue({
    router,
    store,
    context,
    render: (h) => h(App)
  })
  return { app, router, store }
}
```





## 服务端⼊⼝

上⾯的 `bundle` 就是 `webpack` 打包的服务端 `bundle`，我们需要编写服务端⼊⼝⽂件 `src/entry-server.js`
它的任务是创建 `Vue` 实例并根据传⼊ `url` 指定⾸屏

```js
import createApp from "./main";

// 用于首屏渲染
// context有renderer传入
export default (context) => {
  return new Promise((resolve, reject) => {
    // 1.获取路由器和app实例
    const { app, router, store } = createApp(context);
    // 获取首屏地址
    router.push(context.url);
    router.onReady(() => {
      // 获取当前匹配的所有组件
      const matched = router.getMatchedComponents();

      // 404
      if (!matched.length) {
        return reject({code: 404})
      }
      
      // 遍历matched，判断他们内部有没有asyncData
      // 如果有就执行他们，等待执行完毕在返回app
      Promise.all(
        matched.map((Component) => {
          if (Component.asyncData) {
            return Component.asyncData({
              store,
              route: router.currentRoute,
            });
          }
        })
      )
        .then(() => {
          // 约定将app数据状态放入context.state
          // 渲染器会将state序列化变成字符串, window.__INITIAL_STATE__
          // 未来在前端激活之前可以再恢复它
          context.state = store.state;
          resolve(app);
        })
        .catch(reject);
    }, reject);
  });
};
```



## 客户端入口

客户端⼊⼝只需创建 `vue` 实例并执⾏挂载，这⼀步称为激活。创建`entry-client.js`

```js
import createApp from "./main";

// 客户端激活
const {app, router, store} = createApp()

// 还原state
if (window.__INITIAL_STATE__) {
  store.replaceState(window.__INITIAL_STATE__)
}

router.onReady(() => {
  // 挂载激活
  app.$mount('#app')
})
```



## webpack 配置

```shell
# 安装依赖
$ npm install webpack-node-externals lodash.merge -D
```



### 具体配置

`vue.config.js`

```js
// 两个插件分别负责打包客户端和服务端
const VueSSRServerPlugin = require("vue-server-renderer/server-plugin");
const VueSSRClientPlugin = require("vue-server-renderer/client-plugin");
const nodeExternals = require("webpack-node-externals");
const merge = require("lodash.merge");

// 根据传入环境变量决定入口文件和相应配置项
const TARGET_NODE = process.env.WEBPACK_TARGET === "node";
const target = TARGET_NODE ? "server" : "client";

module.exports = {
  css: {
    extract: false
  },
  outputDir: './dist/'+target,
  configureWebpack: () => ({
    // 将 entry 指向应用程序的 server / client 文件
    entry: `./src/entry-${target}.js`,
    // 对 bundle renderer 提供 source map 支持
    devtool: 'source-map',
    // target设置为node使webpack以Node适用的方式处理动态导入，
    // 并且还会在编译Vue组件时告知`vue-loader`输出面向服务器代码。
    target: TARGET_NODE ? "node" : "web",
    // 是否模拟node全局变量
    node: TARGET_NODE ? undefined : false,
    output: {
      // 此处使用Node风格导出模块
      libraryTarget: TARGET_NODE ? "commonjs2" : undefined
    },
    // https://webpack.js.org/configuration/externals/#function
    // https://github.com/liady/webpack-node-externals
    // 外置化应用程序依赖模块。可以使服务器构建速度更快，并生成较小的打包文件。
    externals: TARGET_NODE
      ? nodeExternals({
          // 不要外置化webpack需要处理的依赖模块。
          // 可以在这里添加更多的文件类型。例如，未处理 *.vue 原始文件，
          // 还应该将修改`global`（例如polyfill）的依赖模块列入白名单
          whitelist: [/\.css$/]
        })
      : undefined,
    optimization: {
      splitChunks: undefined
    },
    // 这是将服务器的整个输出构建为单个 JSON 文件的插件。
    // 服务端默认文件名为 `vue-ssr-server-bundle.json`
    // 客户端默认文件名为 `vue-ssr-client-manifest.json`。
    plugins: [TARGET_NODE ? new VueSSRServerPlugin() : new VueSSRClientPlugin()]
  }),
  chainWebpack: config => {
    // cli4项目添加
    if (TARGET_NODE) {
        config.optimization.delete('splitChunks')
    }
      
    config.module
      .rule("vue")
      .use("vue-loader")
      .tap(options => {
        merge(options, {
          optimizeSSR: false
        });
      });
  }
};
```





## 脚本配置

```shell
# 安装依赖
$ npm i cross-env -D
```



**packag.json**

```js
"scripts": {
 "build:client": "vue-cli-service build",
 "build:server": "cross-env WEBPACK_TARGET=node vue-cli-service build",
 "build": "npm run build:server && npm run build:client"
}
```

> 执⾏打包：`npm run build`



## 宿主文件

最后需要定义宿主⽂件，修改 `./public/index.html`

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width,initial-scale=1.0">
    <title>Document</title>
  </head>
  <body>
    
    <!-- 核心就是下面这个注释 -->
    
    <!--vue-ssr-outlet-->
  </body>
</html>
```



## 服务器启动文件

`ssr.js`

修改服务器启动⽂件，现在需要处理所有路由

```js
// node服务器：koa,express,egg.js
const express = require('express')
const app = express()

// 获取文件绝对路径
const resolve = dir => require('path').resolve(__dirname, dir)

// 第 1 步：开放dist/client目录，关闭默认下载index页的选项，不然到不了后面路由
app.use(express.static(resolve('../dist/client'), {index: false}))

// 服务端渲染模块vue-server-renderer
const {createBundleRenderer} = require('vue-server-renderer')

// 获取渲染器
const bundle = resolve("../dist/server/vue-ssr-server-bundle.json");
const renderer = createBundleRenderer(bundle, {
  runInNewContext: false, // https://ssr.vuejs.org/zh/api/#runinnewcontext
  template: require('fs').readFileSync(resolve("../public/index.html"), "utf-8"), // 宿主文件
  clientManifest: require(resolve("../dist/client/vue-ssr-client-manifest.json")) // 客户端清单
})

// 路由
app.get('*', async (req, res) => {
  try {
    const context = {
      url: req.url
    }
    const html = await renderer.renderToString(context)
    res.send(html)
  } catch (error) {
    res.status(500).send('服务器内部错误')
  }
  
})

// 监听
app.listen(3000)
```



## 数据预取

服务器端渲染的是应⽤程序的"快照"，如果应⽤依赖于⼀些异步数据，那么在开始渲染之前，需要先预取和解析好这些数据。



**store**

```js
actions: {
  // 加一个异步请求count的action
  getCount({ commit }) {
    return new Promise(resolve => {
      setTimeout(() => {
        commit("init", Math.random() * 100);
        resolve();
      }, 1000);
    });
  },
},
```



**组件使用**

```js
export default {
  asyncData({ store, route }) {
    // 约定预取逻辑编写在预取钩⼦asyncData中
    // 触发 action 后，返回 Promise 以便确定请求结果
    return store.dispatch("getCount");
  },
};
```



**注意：**

> 服务端和客户端的数据预处理，分别在两者的入口文件中