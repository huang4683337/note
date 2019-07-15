**秒开   在有缓存的情况下秒开  也就是不加载数据**

```javascript
//从详情页返回 列表页

//列表页组件

montded(){

!this.listData.length && this.$store.disapatch("xx")

//  this.listData.length 代表listData的长度  对他取反代表listData没有长度也就是没有数据 ,我们就派发一个action请求数据

}
```







严格模式

```javascript
// 状态更新不是由mutation引起的 是不允许的

// 不允许使用 v-model
```

