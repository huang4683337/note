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

使用`Symbol.asyncIterator` 作为属性时，该属性表示一个方法，对应的方法会返回对象默认的 ` AsyncIterator`，由 **for-await-of** 语句使用。换句话说，这个符号表示实现异步迭代器 API 的函数



## 6、Symbol.hasInstance

+ 使用`Symbol.hasInstance` 作为属性时，该属性表示一个方法
+ 方法决定一个构造器对象是否认可一个对象是它的实例。由 instanceof 使用

```js
function Foo() {}
let f = new Foo();
```

```js
// ES5 中, instanceof 判断实例的原型链上是否存在某个原型
conosle.log( f instanceof Foo ); // true

// ES6 中 instanceof 会使用 Symbol.hasInstance 确定关系
console.log(Foo[Symbol.hasInstance](f));	// true
```



## 7、Symbol.isConcatSpreadable

用`Symbol.isConcatSpreadable` 作为属性时，该属性表示一个布尔值，如果是 true，则对象应该使用 `Array.prototype.concat()` 根据接收到的对象类型选择如何将一个类数组对象拼接成一个数组实例。



```js
let initial = ['foo'];
let array = ['bar'];
let similarArr = { length: 1, 0: 'baz' };
let otherObject = new Set().add('qux');
```



**默认情况下，数组对象、类数组对象、其他对象**

```js
// 普通数组，默认打平到已有数组	 - 打平
console.log(array[Symbol.isConcatSpreadable]);  // undefined
console.log(initial.concat(array)); // [ 'foo', 'bar' ]

// 类数组对象，默认将整个对象添加到末尾 - 整个对象
console.log(similarArr[Symbol.isConcatSpreadable]); // undefined
console.log(initial.concat(similarArr));    // [ 'foo', { '0': 'baz', length: 1 } ]

// 其他对象，默认将整个对象添加到末尾 - 整个对象
console.log(array[Symbol.otherObject]); // undefined
console.log(initial.concat(otherObject));   // [ 'foo', Set(1) { 'qux' } ]
```



**值为 false 或者 假值时，数组对象、类数组对象、其他对象**

```js
// 普通数组，将整个对象添加到数组末尾 - 整个对象
array[Symbol.isConcatSpreadable] = false;
console.log(initial.concat(array)); 
// ['foo',Array(1)] ==> [ 'foo', [ 'bar', [Symbol(Symbol.isConcatSpreadable)]: false ] ]


// 类数组对象，整个对象添加到数组末尾 - 整个对象
similarArr[Symbol.isConcatSpreadable] = false;
console.log(initial.concat(similarArr)); 
// ['foo', {...}] ==> ['foo', { '0': 'baz', length: 1, [Symbol(Symbol.isConcatSpreadable)]: false }]

// 其他对象，整个对象添加到数组末尾 - 整个对象
otherObject[Symbol.isConcatSpreadable] = false;
console.log(initial.concat(otherObject));
// [ 'foo', Set(1) ] ==> [ 'foo', Set(1) { 'qux', [Symbol(Symbol.isConcatSpreadable)]: false } ]
```



**值为 true 或者 真值时，数组对象、类数组对象、其他对象**

```js
// 普通数组，将整个对象打平添加到数组末尾 - 打平
array[Symbol.isConcatSpreadable] = true;
console.log(initial.concat(array)); // ['foo', 'bar']

// 类数组对象，将整个对象打平添加到数组末尾 - 打平
similarArr[Symbol.isConcatSpreadable] = true;
console.log(initial.concat(similarArr)); // [ 'foo', 'baz' ]

// 其他对象，会被忽略掉
otherObject[Symbol.isConcatSpreadable] = true;
console.log(initial.concat(otherObject));   // ['foo']
```



## 8、Symbol.iterator

+ 使用`Symbol.iterator` 作为属性时，该属性表示一个方法
+ 方法返回一个默认的迭代器，由 *for-of* 使用。

> 详情参考第七章迭代器



## 9、Symbol.match

+ 使用`Symbol.match` 作为属性时，该属性表示一个正则表达式方法，
+ 方法会使用正则表达式去匹配字符串，由 `String.prototype.match()` 方法使用。

`String.prototype.match()`方法会使 用以 `Symbol.match` 为键的函数来对正则表达式求值。

```js
console.log(RegExp.prototype[Symbol.match]);
// ƒ [Symbol.match]() { [native code] } 5

console.log('foobar'.match(/bar/));
// ["bar", index: 3, input: "foobar", groups: undefined]
```



`String.prototype.match()`  会将非正则表达式转化成正则表达式，如果不想要这种行为，可以通过 `Symbol.match` 重写方法。

```js
class FooMatcher {
    static [Symbol.match](target) {
        return target.includes('foo');
    }
}

console.log('foobar'.match(FooMatcher)); // true 
console.log('barbaz'.match(FooMatcher)); // false
```



## 10、Symbol.replace

+ 使用`Symbol.replace` 作为属性时，该属性表示一个正则表达式方法，
+ 方法会替换掉字符串中匹配的子串，由 `String.prototype.replace()` 方法使用。

`String.prototype.replace()` 方法会使用以 `Symbol.replace`为键的函数对正则表达式求值。

```js
console.log(RegExp.prototype[Symbol.replace]);
// ƒ [Symbol.replace]() { [native code] } 

console.log('foobarbaz'.replace(/bar/, 'qux'));
// 'fooquxbaz'
```



`String.prototype.replace()` 方法会将非正则表达式的参数转化为 RegExp 对象，想要改变这种行为就重新定义 `Symbol.replace ` 函数

```js
class StringReplacer {
    constructor(str) {
        this.str = str;
    }
    [Symbol.replace](target, replacement) {
        return target.split(this.str).join(replacement);
    }
}

console.log('barfoobaz'.replace(new StringReplacer('foo'), 'qux')); // "barquxbaz"
```

```js
// 'barfoobaz'.replace('foo','qux');  将 barfoobaz 中的 foo 替换为 qux
/**
 * 1. barfoobaz 通过 foo 进行拆分; 'barfoobaz'.split('foo'); ==> [ 'bar', 'baz' ]
 * 2. 拆分后的数组转成字符串，通过 qux 进行拼接
 *    [ 'bar', 'baz' ].join('qux'); ==> barquxbaz
 */
```



## 11、Symbol.search

+ 使用`Symbol.search` 作为属性时，该属性表示一个正则表达式方法，
+ 方法会返回字符串中匹配正则表达式的索引，由 `String.prototype.search()` 方法使用。

`String.prototype.search() `方法会使用以 `Symbol.search `为键的函数来对正则表达式求值。

```js
console.log(RegExp.prototype[Symbol.search]); 
// ƒ [Symbol.search]() { [native code] }

console.log('foobar'.search(/bar/)); // 3
```

`String.prototype.search()` 方法会将非正则表达式的参数转化为 RegExp 对象，想要改变这种行为就重新定义 `Symbol.search ` 函数

```js
class StringSearcher {
    constructor(str) {
        this.str = str;
    }
    [Symbol.search](target) {
        return target.indexOf(this.str);
    }
}
console.log('foobar'.search(new StringSearcher('foo'))); // 0 
console.log('barfoo'.search(new StringSearcher('foo'))); // 3 
console.log('barbaz'.search(new StringSearcher('qux'))); // -1
```





## 12、Symbol.species

+ 使用`Symbol.species` 作为属性时，该属性表示一个函数值
+ 这个函数作为派生对象（子类等）的构造函数。

常用于内置类型，用于对内置类型（ Array ）实例化方法的返回值（ [1,2] ）暴露实例化派生对象的方法。

```js
// 通过内置类型 Array 实例化，返回一个 arr
let arr = new Array();

// 通过访问 Symbol.species 可以知道是谁实例化得到了派生类（arr）
console.log( arr instanceof Array ); // true
```



```js
class Bar extends Array { };

let bar = new Bar(); 
console.log(bar instanceof Array);  // true
console.log(bar instanceof Bar);  // true


bar = bar.concat('bar'); 
console.log(bar instanceof Array);  // true
console.log(bar instanceof Bar);  // true
```



用 `Symbol.species` 定义静态的获取器(getter)方法，可以覆盖新创建实例的原型定义。

```js
class Baz extends Array {
    static get [Symbol.species]() {
        return Array;
    }
}

let baz = new Baz();
console.log(baz instanceof Array);  // true
console.log(baz instanceof Baz);  // true


// 通过内置类型 Array 下的 concat 方法生成一个新的实例
// 访问新实例的构造函数会触发当前实例 baz 的 Symbol.species
// 这样将新实例 baz1 的构造函数就重写为 Array 了
baz1 = baz.concat('baz');
console.log(baz1 instanceof Array);  // true
console.log(baz1 instanceof Baz);  // false
```





## 13、Symbol.split

+ 使用`Symbol.split` 作为属性时，该属性表示一个正则表达式方法，

+ 方法会匹配正则表达式的索引位置拆分字符串，由 `String.prototype.split()` 方法使用。

`String.prototype.split() `方法会使用以 `Symbol.split `为键的函数来对正则表达式求值。

```js
console.log(RegExp.prototype[Symbol.split]); 
// ƒ [Symbol.split]() { [native code] }

console.log('foobarbaz'.split(/bar/)); 
// ['foo', 'baz']
```

`String.prototype.split()` 方法会将非正则表达式的参数转化为 RegExp 对象，想要改变这种行为就重新定义 `Symbol.split` 函数。

`Symbol.split` 函数接收一个参数，就是调用` match()` 方法的字符串实例。

```js
class StringSplitter {
    constructor(str) {
        this.str = str;
    }
    [Symbol.split](target) {
        return target.split(this.str);
    }
}
console.log('barfoobaz'.split(new StringSplitter('foo'))); // ["bar", "baz"]
```



## 14、Symbol.toPrimitive

+ 使用`Symbol.toPrimitive` 作为属性时，该属性表示一个*方法*。

+ 该方法将对象转换为相应的原始值。由 `ToPrimitive` 抽象操作使用。

很多内置操作都会尝试强制将对象转换为原始值，包括字符串、 数值和未指定的原始类型。

```js
class Foo { }
let foo = new Foo();

console.log(3 + foo);   // 3[object Object]
console.log(3 - foo);   // NaN
console.log(String(foo));   // [object Object]
```



对于一个自定义对象实例，通过在这个实例的 `Symbol.toPrimitive`  属性上定义一个函数可以改变默认行为。

```js
// 根据提供给这个函数的参数(string、number 或 default)，可以控制返回的原始值

class Bar {
    constructor() {
        this[Symbol.toPrimitive] = function (hint) {
            switch (hint) {
                case 'number':
                    return 3;
                case 'string':
                    return 'string bar';
                case 'default':
                default:
                    return 'default bar';
            }
        }
    }
}


let bar = new Bar();
console.log(3 + bar); // "3default bar"
console.log(3 - bar); // 0 
console.log(String(bar)); // "string bar"
```



## 15、Symbol.toStringTag

+ 使用`Symbol.toStringTag` 作为属性时，该属性表示一个字符串。

+ 该字符串用于创建对象的默认字符串描述，由内置方法 `Object.prototype.toString()` 使用。

通过 ` toString()` 方法获取对象标识时，会检索由 `Symbol.toStringTag` 指定的实例标识符，默认为 `Object`。内置类型已经指定了这个值，但自定义类实例还需要明确定义。

```js
// 内置类型
let s = new Set();
console.log(s); // Set(0) {} 
console.log(s.toString()); // [object Set] 
console.log(s[Symbol.toStringTag]); // Set
```

```js
// 自定义类型 - 未明确指定时为 undefined
class Foo { }
let foo = new Foo();
console.log(foo);   // Foo {}
console.log(foo.toString());  // [object Object]
console.log(foo[Symbol.toStringTag]);   // undefined

// 自定义类型 - 明确指定类实例
class Bar {
    constructor() {
        this[Symbol.toStringTag] = 'Bar';
    }
}
let bar = new Bar();

console.log(bar);  // Bar {}
console.log(bar.toString());   // [object Bar] 
console.log(bar[Symbol.toStringTag]);  // Bar
```



## 16、Symbol.unscopables

+ 使用`Symbol.unscopables` 作为属性时，该属性表示一个对象。

+ 该对象所有的以及继承的属性， 都会从关联对象的 with 环境绑定中排除

设置这个符号并让其映射对应属性的键值为 true，就可以 10 阻止该属性出现在 with 环境绑定中

```js
let o = { foo: 'bar' };
with (o) {
    console.log(foo); // bar
}

o[Symbol.unscopables] = {
    foo: true
};
with (o) {
    console.log(foo); // ReferenceError
}
```

> 不推荐使用 with，因此也不推荐使用 Symbol.unscopables。
