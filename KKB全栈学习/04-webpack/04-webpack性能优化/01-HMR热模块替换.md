## HMR热模块替换 

+ 修改代码，不需要新浏览器，保存之后自动改变

+ `css` 热模块替换、`js` 热模块替换

+ 不支持 抽离出来的 `css` , 所以要使用 `style-loader`



**注意**

```shell
修改css：保存后会修改样式，浏览器操作的痕迹还在（DOM不会发生改变）
修改js：保存后浏览器会直接刷新，重新加载
```



## 开启 css 的 HMR

**webpack.config.js**

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



**js测试代码**

```js
// src/index.js

var btn = document.createElement("button");
btn.innerHTML = "新增";
document.body.appendChild(btn);
btn.onclick = function () {
    var div = document.createElement("div");
    div.innerHTML = "item";
    document.body.appendChild(div);
};
```



**index.css**

```css
div:nth-of-type(odd) {
    background: yellow;
}
```



**操作**

```js
浏览器多次点击新增
修改 css 的 yellow 为 blue
保存 ==> 查看浏览器 ==> 发现刚刚添加的 DOM 都还存在 ==> 只是颜色变为 blue
```



## 开启 js 的 HMR（JS的HMR原理） 

> 需要使用 `module.hot.accept` 来观察模块更新 从而更新

```js
修改 js 不希望直接触发浏览器的刷新，希望触发的是对应模块的刷新
```



**关闭浏览器自动刷新**

```js
// webpack.config.js 下的 devserver 下添加  hotOnly:true,

// 关闭浏览器 js 的自动刷新
```



修改 js 代码后，发现浏览器不会发生刷新了，证明浏览器 js 的自动刷新已经关闭了。

那么我们如何保证在修改 js 文件之后，对应的模块刷新呢？



**保障  js 修改，对应模块刷新**

使用 `module.hot`  API, 这个 API 对应的就是 webpack.config.js 下 de'vServer 下的 hot 属性。 

```js
// 是否开启 HMR 热模块替换
if (module.hot) {
    // 监听 number.js 发生改变
    module.hot.accept("./jsHMR/number.js", () => {
        // 删除当前模块生成的 div
        document.body.removeChild(document.getElementById("number"));
        
        // 重新创建一个 div,实现热模块替换
        number();
    });
}
```



**监听多个模块的 HMR 热替换**

```js
// 每个模块都需要监听不够人性化，那么该怎么处理

// 浏览器和服务直间是通过 wds 来进行通信的

// 开启 HMR 之后，webpack会给每个模块一个对应 ID

// webpack 会监听对应 ID 实现模块更新
```

[官方文档](https://webpack.docschina.org/guides/hot-module-replacement/#other-code-and-frameworks)



## vue-loader 实现 vue HMR

[vue 官网](https://cn.vuejs.org/)

[生态系统菜单 - Vue Loader](https://vue-loader.vuejs.org/zh/)



## 示例 React 的 HMR

[React Hot Loader 实现HMR](https://github.com/gaearon/react-hot-loader)

