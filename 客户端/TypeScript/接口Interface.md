# 接口 Interface

TypeScript 的核心原则之一，可以对所具有的结构进行类型检查。

Interface 也被称作 `鸭式辨型法`  或 `结构性子类型化` 。



## 简单使用

```js
interface data {
    name: string;
}

function sayName(obj: data){
    console.log(obj.name)
}
sayName({name:'名字'})
```

