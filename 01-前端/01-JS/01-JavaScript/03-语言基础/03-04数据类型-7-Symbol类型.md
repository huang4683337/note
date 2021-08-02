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

如果运行时的不同部分需要共享和重用符号实例，那么可以用一个**字符串作为键**，在**全局符号注册**表中创建并重用符号。



**使用 `symbol.for()`在全局注册表中创建符号**

```js
let fooGlobalSymbol = Symbol.for('foo');
console.log(typeof fooGlobalSymbol); // symbol
```



**创建与重用符号**

```js
let fooGlobalSymbol = Symbol.for('foo'); // 创建新符号 
let otherFooGlobalSymbol = Symbol.for('foo'); // 重用已有符号
console.log(fooGlobalSymbol === otherFooGlobalSymbol); // true
```



**全局注册表中定义的符号和 `Symbol()` 中定义的符号不等同**

```js
let localSymbol = Symbol('foo');
let globalSymbol = Symbol.for('foo');
console.log(localSymbol === globalSymbol); // false
```



**注册全局符号的键必须是字符串，非字符串的会被转为字符串，这个键会被当作符号描述**

```js
let emptyGlobalSymbol = Symbol.for(); 
console.log(emptyGlobalSymbol); // Symbol(undefined)

let emptyGlobalSymbol = Symbol.for(null); 
console.log(emptyGlobalSymbol); // Symbol(null)

let emptyGlobalSymbol = Symbol.for({ a: 1 });
console.log(emptyGlobalSymbol); // Symbol([object Object])
```

```js
// 使用对象 {} 作为键时创建的字符全是相等的，注意不能使用对象作为键
let emptyGlobalSymbol = Symbol.for({ a: 1 });
let emptyGlobalSymbol1 = Symbol.for({ b: 2 });
console.log(emptyGlobalSymbol); // Symbol([object Object])
console.log(emptyGlobalSymbol1); // Symbol([object Object])

console.log(emptyGlobalSymbol === emptyGlobalSymbol1); // true

// 除了使用 {} 创建的全局符号注册表有问题，其他的数据类型都可以
```



**使用 `Symbol.keyFor()` 来查询全局注册表, 查询的参数为创建的符号，查询不到则返回 `undefined`**

```js
// 创建全局符号 s
let s = Symbol.for('foo'); 
// 查询是否存在全局符号 s
console.log(Symbol.keyFor(s)); // foo


// 创建普通符号
let s2 = Symbol('bar'); 
console.log(Symbol.keyFor(s2)); // undefined
```



## 3、使用符号作为属性

凡是可以使用字符串或数值作为属性的地方，都可以使用符号。



**通过计算属性来使用符号作为属性**

```js
let s = Symbol('foo');
let o = {};
```

```js
// 不通过计算属性使用符号来作为属性
o.s = 1;
console.log(o);	// { s: 1 }
```

```js
// 通过计算属性使用符号来作为属性
o[s] = 1;
console.log(o); // { [Symbol(foo)]: 1 }
```



**因为符号是一个实例，使用符号作为属性时，实际上使用的是符号地址作为属性**

```js
// 隐式声明的符号作为属性
let o = {
    [Symbol('foo')]: 'foo val',
    [Symbol('bar')]: 'bar val'
};

// 通过隐式声名时，相当于创建一个新的符号，访问这个新的符号的值
console.log(o[Symbol('foo')]);	// undefined
```

```js
// 遍历对象所有符号，查找对应的符号
let o = {
    [Symbol('foo')]: 'foo val',
    [Symbol('bar')]: 'bar val'
};

let v = Object.getOwnPropertySymbols(o).filter(symbol => symbol.toString().match(/bar/))

console.log(v);
```



## 4、常用内置符号

> ECMAScript 6 也引入了一批常用内置符号(well-known symbol)，用于暴露语言内部行为开发者可以直接访问、重写或模拟这些行为。这些内置符号都以 Symbol 工厂函数字符串属性的形式存在。



for-of 循环会在相关对象上使用 Symbol.iterator 属性，那么就可以通过在自定义对象上重新定义 Symbol.iterator 的值，来改变 for-of 在迭代该对象时的行为。



> 所有内置符号属性都是不可写、不可枚举、不可配置的

```js
// 定义一个对象，代表 symbol 对象
let o = {};

// 对象中有一个内置属性 iterator，是一个普通的字符串属性
Object.defineProperty(o, 'iterator', {
    configurable: false,
    enumerable: false,
    writable: false
})

// 在 obj 中使用 o.iterator 声明一个工厂函数
let obj = {
    [o.iterator]: function () {
        return { a: 1 }
    }
}

// 执行 obj[o.iterator]
console.log(obj[o.iterator]());	// { a: 1 }
```



> 在提到 ECMAScript 规范时，经常会引用符号在规范中的名称，前缀为@@。比如， @@iterator 指的就是 Symbol.iterator。



## 5、Symbol.asyncIterator

使用`Symbol.asyncIterator` 作为属性时，该属性对应的方法会返回对象默认的 ` AsyncIterator`，由 **for-await-of** 语句使用。换句话说，这个符号表示实现异步迭代器 API 的函数



## 6、Symbol.hasInstance

使用`Symbol.hasInstance` 作为属性时，该属性对应的方法决定一个构造器对象是否认可一个对象是它的实例。由 instanceof 使用

```js
function Foo() {}
let f = new Foo();
```

```js
// ES5 中
conosle.log( f instanceof Foo ); // true

// ES6 中 instanceof 会使用 Symbol.hasInstance 确定关系
console.log(Foo[Symbol.hasInstance](f))
```





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



