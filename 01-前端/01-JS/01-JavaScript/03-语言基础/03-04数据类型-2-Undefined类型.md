## Undefined 类型

Undefined 类型只有一个值，就是特殊值 undefined。

声明了变量但没有初始化时，就相当于给变量赋予了 undefined 值。

```js
var a;
console.log(a); // undefined

let b;
console.log(b); // undefined
```



> 增加 undefined 这个特殊值的目的就是为 了正式明确空对象指针(null)和未初始化变量的区别

