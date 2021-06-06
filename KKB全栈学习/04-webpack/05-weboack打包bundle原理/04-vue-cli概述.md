[vue2官网](vuejs.org)

[vue-cli官网](https://cli.vuejs.org/zh/)

vue.config.js就是对webpack的相关字段进行了一次二次封装并重新命名，目的是为了降低使用vue时配置webpack的门槛。



### configureWebpack

是一个对象，原生webpack怎么写，configureWebpack 就怎么写，最终通过 `webpack-merge` 这个包合并为真正的 webpack 配置

```js
module.exports = {
	configureWebpack:{
    entry:'',
    output:'',
    plugins:[]
  }  
}
```



### chainWebpack

接收一个基于 `webpack-chain` 的 configureWebpack 实例， 是一个链式操作。