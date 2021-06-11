## MutationObserver

MutationObserver是HTML5新增的属性，用于监听DOM修改事件，能够监听到节点的属性、文本内容、子节点等的改动，是一个功能强大的利器。

```js
var observer = new MutationObserver(function(){
//这里是回调函数
	console.log('DOM被修改了！');
});
var article = document.querySelector('article');
observer.observer(article);
```



## vue 中 nextTick 的实现

```
- 检测浏览器是否支持 MutationObserver
- 将DOM更新函数通过 MutationObserver 创建一个微任务
```

```js
//vue@2.2.5 /src/core/util/env.js
if (typeof MutationObserver !== 'undefined' && (isNative(MutationObserver) ||
    MutationObserver.toString() === '[object MutationObserverConstructor]')) {
    var counter = 1
    var observer = new MutationObserver(nextTickHandler)
    var textNode = document.createTextNode(String(counter))
    observer.observe(textNode, {
        characterData: true
    })
    timerFunc = () => {
        counter = (counter + 1) % 2
        textNode.data = String(counter)
    }
}
```

```
但是这个 API 在 IOS 上兼容不太行，最后抛弃它做了向下兼容
```





## 总结

```
1. vue 用异步队列的方式来控制 DOM 更新和 nextTick 回调先后执行
2. microtask 因为其高优先级特性，能确保队列中的微任务在一次事件循环前被执行完毕
3. 因为兼容性问题，vue 不得不做了 microtask 向 macrotask 的降级方案
	- Promise、setImmediate、MessageChannel、setTimeout
```

