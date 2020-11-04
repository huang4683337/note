vue-cli中 vue.config.js配置webpack的三种方式

+ 传统对象配置

  ```js
  a:{
    
  }
  ```

+ 函数式配置

  ```js
  a(config){
    // 此处可以对 config 做些处理
  }
  ```

+ 链式配置





```shell
vue inspect --rules  # 查看当前项目所有规则

vue inspect --rule svg  # 查看当前项目 svg 规则
```





![1600348745278](C:\Users\Amd\AppData\Roaming\Typora\typora-user-images\1600348745278.png)



```js
--mode xx 用来定义哪个文件生效

// .env.dev ---> --mode dev
```

