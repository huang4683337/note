#  vue常见问题

## 解决非工程化项目初始化页面闪动问题

[vue](https://www.baidu.com/s?wd=vue&tn=24004469_oem_dg&rsv_dl=gh_pl_sl_csd)页面在加载的时候闪烁花括号{}}，v-cloak指令和css规则如[v-cloak]{display:none}一起用时，这个指令可以隐藏未编译的Mustache标签直到实例准备完毕。

```
/*css样式*/
[v-cloak] {
	display: none;
}

<!--html代码-->
<div id="app" v-cloak>
    <ul>
      <li v-for="item in tabs">{{item.text}}</li>
    </ul>
</div>
```



## vue-cli开发环境和生产环境分别如何使用全局常量

```javascript
/*
开发环境的全局常量定义在.env里，
生产环境的全局常量定义在.env.production里
*/

config文件中
/*
  创建普通全局变量 .env
  创建开发环境变量 .env.development
  创建生产环境变量 .env.production
*/
```

