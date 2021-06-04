## source-map

### 为什么使用 source-map

+ 为了开发时，快速的定位问题
+ 通过sourceMap定位到源代码：线上代码，我们有时候也会开启前端错误监控，快速的定位问题。
+ 源代码与打包后的代码的映射关系，通过sourceMap定位到源代码。
+ 在dev模式中，默认开启，关闭的话可以在配置⽂件 `webpack.config.js` ⾥



### 开启

```js
module.exports = {
	...
  devtool:"source-map"  
}
```



### 关闭

```js
module.exports = {
	...
  devtool:"none"  
}
```





## devtool

[devtool的介绍](https://webpack.docschina.org/configuration/devtool/)

| 属性              | 功能                                                         |
| ----------------- | ------------------------------------------------------------ |
| eval              | 速度最快,使⽤eval包裹模块代码,                               |
| source-map        | 产⽣ .map ⽂件 外部产⽣ 错误代码的准确信息和位置             |
| cheap             | 较快，不包含列信息                                           |
| Module            | 第三⽅模块，包含loader的sourcemap（⽐如jsx to js ，babel的sourcemap） |
| inline-source-map | 将 .map 作为DataURI嵌⼊，不单独⽣成 .map ⽂件                |



## 配置推荐：

```js
devtool:"cheap-module-eval-source-map",// 开发环境配置

//线上不推荐开启
devtool:"cheap-module-source-map", // 线上⽣成配置
```

