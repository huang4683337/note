```js
var a = 10;
console.log(this.a); 		// 10
console.log(window.a);	// 10
```

```js
let a = 10;
console.log(this.a); 		// undefined
console.log(window.a);	// undefined

// 同时 let、const、class 也存在此问题
```



## 为什么 ES6 中定义的变量不能在 window 下找到

