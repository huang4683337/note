## Symbol 类型

`Symbol`(符号) 是 ECMAScript 6 新增的数据类型。

符号是原始值，且符号唯一、不可变。

符号是为了创建唯一记号，用来标记非字符串对象的属性。



## 1、符号的基本用法

+ 符号 需要使用 `Symbol()` 初始化。
+ 符号是原始数据类型

```js
// 使用 Symbol 初始化函数
let sym = Symbol();
```

```js
// 符号的数据类型为 symbol
console.log(typeof sym); // symbol
```



**符号的描述**

```js
// 创建符号时，可以传一个参数作为符号的描述
// 用于调试、识别

let sym = Symbol('foo');
console.log(sym); // Symbol('foo')
```



**符号不能通过 new 关键字使用, 避免创建符号包装对象**

```js
console.log(typeof new Number());  // object
console.log(typeof new Boolean());  // object
console.log(typeof new String());  // object

console.log(typeof new Symbol());  // TypeError: Symbol is not a constructor
```



**创建符号包装对象,  通过 Object() 创建**

```js
let sym = Object(Symbol());
console.log(typeof sym);  // object
```



## 2、使用全局符号注册表





## 3、使用符号作为属性





## 4、常用内置符号



## 5、Symbol.asyncIterator



## 6、Symbol.hasInstance



## 7、Symbol.isConcatSpreadable



## 8、Symbol.iterator



## 9、Symbol.match



## 10、Symbol.replace



## 11、Symbol.search



## 12、Symbol.species



## 13、Symbol.split



## 14、Symbol.toPrimitive



## 15、Symbol.toStringTag



## 16、Symbol.unscopables



