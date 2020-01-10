# Symbol

## 概述

> 一种新的数据类型 表示独一无二的。
>
> 不能和其他类型的值进行计算
>
> 不能转化成数值

```js
let s = Symbol();

typeof s
// "symbol"

console.log( Symbol('f')===Symbol('f') ); // false
```



```js
// 接收一个字符串作为参数，代表对Symbol实例的描述。 用于转化成字符串 或者在控制台显示
let s1 = Symbol('foo');
let s2 = Symbol('bar');

s1 // Symbol(foo)
s2 // Symbol(bar)

s1.toString() // "Symbol(foo)"
s2.toString() // "Symbol(bar)"
```



```js
// Symbol不能和其他类型的值进行计算
'a'+Symbol('s'); //TypeError: can't convert symbol to string

// Symbol可以转化为布尔值。但不能转化为数值
Number( Symbol('s') ); //TypeError
Boolean( Symbol('s') ); // true
```



## Symbol.prototype.description

> 直接返回 Symbol 的描述

```js
const sym = Symbol('foo');

String(sym) // "Symbol(foo)"

sym.description // "foo"
```

## 作为属性名的 Symbol

> 由于Symol的唯一性，可以用来作为属性名。
>
> Symbol 作为属性名时必须放在 [] 中, 不能使用点运算符。
>
> Symbol作为属性名时，这个属性是公开属性不是私有属性。

```js
let mySymbol = Symbol(); // 定义一个 Symbol 数据类型

// 第一种写法
let a = {};
a[mySymbol] = 'Hello!';

// 第二种写法
let a = {
  [mySymbol]: 'Hello!'
};

// 第三种写法
let a = {};
Object.defineProperty(a, mySymbol, { value: 'Hello!' });

// 以上写法都得到同样结果
a[mySymbol] // "Hello!"
```

## 属性名的遍历

> Symbol作为属性名时，不会出现在`fro...in`、`for...of`。
>
> 也不会被 `Object.keys()`、`Object.getOwnPropertyNames()`、`JSON.sstringify()`返回

### Object.getOwnPropertySymbols

> 可以获得当前对象中，所有的Symbol属性名。
>
> 返回一个数组

```JS
const obj = {};
let a = Symbol('a');
let b = Symbol('b');

obj[a] = 'Hello';
obj[b] = 'World';

const objectSymbols = Object.getOwnPropertySymbols(obj);

objectSymbols
// [Symbol(a), Symbol(b)]
```

### Reflect.ownKeys()

> 返回所有的属性名，包括常规的属性名和Symbol属性名。
>
> 返回一个数组

```js
let obj = {
  [Symbol('my_key')]: 1,
  enum: 2,
  nonEnum: 3
};

Reflect.ownKeys(obj)
//  ["enum", "nonEnum", Symbol(my_key)]
```

## Symbol.for() 、Symbol.keyFor()

```js
/*
Symbol.for() : 接收一个字符串作为参数，会检查当前参数的Symbol值是否存在，存在就返回，不存在就创建一个新的
Symbol() : 每次都会新建一个 Symbol 值。

如果调用30次 Symbol.for() , 每次都会返回同一个 Symbol 值。
如果调用30次 Symbol() , 会返回30个不同的 Symbol 值。
*/


Symbol.for("bar") === Symbol.for("bar")
// true

Symbol("bar") === Symbol("bar")
// false
```

> Symbol.for() 为 Symbol 登记的名字是全局的。
>
> Symbol() 不存在登记机制

```js
// Symbol.keyFor() 返回一个已经登记的 Symbol 的 key

let s1 = Symbol.for("foo");
Symbol.keyFor(s1) // "foo"

let s2 = Symbol("foo");
Symbol.keyFor(s2) // undefined
```



`Symbol.for()`的这个全局登记特性，可以用在不同的 iframe 或 service worker 中取到同一个值。

```js
iframe = document.createElement('iframe');
iframe.src = String(window.location);
document.body.appendChild(iframe);

iframe.contentWindow.Symbol.for('foo') === Symbol.for('foo')
// true

//上面代码中，iframe 窗口生成的 Symbol 值，可以在主页面得到。
```



## Symbol 的内置属性

### Symbol.hasInstance

```js

```



