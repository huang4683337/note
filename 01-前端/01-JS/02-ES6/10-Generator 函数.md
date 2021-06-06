## Generator 概念

`Generator` 函数是ES6提供的一种异步编程解决方案，语法行为与传统函数完全不同

+ Generator函数是一个状态机，封装了多个内部状态。

+ Generator函数是一个遍历器对象生成函数，执行Generator函数会返回一个遍历器对象，

  可以依次遍历Generator函数内部的每一个状态。



## Generator 特征

**特征：**

+ `function` 关键字 与 函数名 之间有一个 `星号`
+ 函数体内部使用`yield`语句，定义不同的内部状态

```js
function* helloWorldGenerator() {
  yield 'hello';
  yield 'world';
  return 'ending';
}

const hw = helloWorldGenerator();

// 定义一个 Generator 函数 helloWorldGenerator
// 函数内部有两个 yield 语句：hello 、world
// 函数有三个状态：hello，world和return语句（结束执行）
```



**函数分析：**

+ 调用Generator函数后，该函数并不执行。

+ 返回的也不是函数运行结果，而是一个指向内部状态的指针对象。

+ 这个对象就是遍历器对象（Iterator Object）。



```js
console.log(hw.next()); // { value: 'hello', done: false }
console.log(hw.next()); // { value: 'world', done: false }
console.log(hw.next()); // { value: 'ending', done: true }
console.log(hw.next()); // { value: undefined, done: true }

// 使用 next() 访问遍历器
// 从函数头部开始，遇到一个 yield 或者 return 就停止
// yield || return 语句是暂停执行的标记，而next方法可以恢复执行
```





## yield语句

+ 由于Generator函数返回的遍历器对象，
+ 只有调用`next`方法才会遍历下一个内部状态，所以其实提供了一种可以暂停执行的函数。
+ `yield`语句就是暂停标志。



遍历器对象的`next`方法的运行逻辑如下

+ 遇到`yield`语句，就暂停执行后面的操作，

+ 并将紧跟在`yield`后面的那个表达式的值，作为返回的对象的`value`属性值。

  ```js
  function* gen() {
      yield 123 + 456;
  }
  
  // yield后面的表达式123 + 456，不会立即求值
  // 只有 next 方法将指针移到这一个 yield 时，表达式才会求值
  
  const generator = gen();
  
  console.log( generator.next() );    
  // { value: 579, done: true }
  // 值为 579，遍历结束
  ```

+ 如果没有新的 `yield`  语句，函数就会一直执行到完毕，或者遇到 `return`  为止。

+ 并将 `return`  后面到表达式的值，作为返回对象的 `value` 属性的值

  ```js
  function* gen() {
      yield 123 + 456;
      return '函数执行完毕'
  }
  
  const generator = gen();
  
  console.log(generator.next());  // { value: 579, done: false }
  console.log(generator.next());  // { value: '函数执行完毕', done: true }
  ```

+ 如果没有 `return ` 语句，返回对象的 `value` 属性的值为 `undefined`

  ```js
  function* gen() {
      yield 123 + 456;
  }
  
  const generator = gen();
  
  console.log(generator.next());  // { value: 579, done: false }
  console.log(generator.next());  // { value: undefined, done: true }
  
  ```

  



## Generator 与 Iterator 接口的关系

+ 任意对象的 `Symbol.iterator` 属性对应的方法，是该对象的遍历器生成函数。会返回一个该对象的遍历器对象
+ Generator函数是一个遍历器对象生成函数
+ 所以可以将 `Genator函数` 赋值给对象的  `Symbol.iterator`  属性

```js
const myIterable = {};
myIterable[Symbol.iterator] = function* () {
    yield 1;
    yield 2;
    yield 3;
};

[...myIterable] // [1, 2, 3]

// Generator 函数赋值给 myIterable 对象的 Symbol.iterator 属性
// myIterable 对象就具有了 Iterator 接口
// myIterable 对象就可以被 ... 操作符遍历了
```



Generator 函数执行后，返回一个遍历器对象。该对象本身也具有`Symbol.iterator`属性，执行后返回自身。

```js
function* gen(){
  // some code
}

var g = gen();

g[Symbol.iterator]() === g
```



## next 方法的参数

+ `yield` 本身没有返回值，或者说 `yield` 总是返回 `undefined`

```js
function* fn() {
    const y = yield 1;
    console.log(y);
    return 1
}

let f = fn();
f.next();
f.next();	// undefined
```

+ `next` 方法可以带一个参数，这个参数，可以作为上一个 `yield` 的返回值

```js
function* fn() {
    const a = yield (1 + 1);
    const b = (yield (a + 2)) / 2
    return a + b
}

let f = fn();
console.log(f.next()); 		// { value: 2, done: false }
console.log(f.next(4));		// { value: 6, done: false }
console.log(f.next(8));		// { value: 8, done: true }
```

`f.next()`  访问的是	`yield (1 + 1)` ，这个 `yield` 语句后面的表达式`(1+1)` 的运算结果



`f.next(4)` 访问的是	`yield (a + 2)` ，这个 `yield` 语句后面的表达式 `(a+2)` 的运算结果
将 `yield (1 + 1)` 这个 yield 表达式的值设为 4，
则 a = 4, 然后执行 a+2 ， 
返回对象的 value 为 4+2=6



`f.next(8) `访问的是	`return a + b` 的结果
将 `yield (a+2)` 这个 yield 表达式的值设为 8，
则 b = 8/2，b= 4, 然后执行 a + b
返回对象的 value 为 4+4=8





## for ... of

+ `for ... of` 会自动遍历  Generator 函数生成的 Iterator 对象

+ `for ... of` 只会访问 done 属性为 false 的值

  所以 retrun 的值不会被访问

  ```js
  function* fn() {
      yield 1;	// { value: 1, done: false }
      yield 2;	// { value: 2, done: false }
      yield 3;	// { value: 3, done: false }
      return 4;	// { value: 4, done: true }
  }
  
  let f = fn();
  
  for (let k of f) {
      console.log(k);	// 1 2 3
  }
  ```

  

  



## Generator.prototype.throw()

Generator函数返回的遍历器对象，都有一个`throw`方法

+ 在 Generator 函数体外抛出错误

+ 在 Generator 函数体内捕获，函数体外部抛出的错误

  ```js
  var g = function* () {
      try {
          yield 1;
      } catch (e) {
          console.log('内部捕获', e);
      }
  };
  
  var i = g();
  
  console.log(i.next());		// { value: 1, done: false }
  
  
  try {
      i.throw('a');		// 第一个错误被 generator 函数的 catch 捕获
      i.throw('b');		// 第二个错误因为 generator 函数的 catch 已经执行过了，所以会在外部捕获
  } catch (e) {
      console.log('外部捕获', e);
  }
  
  
  // 内部捕获 a
  // 外部捕获 b
  ```

  



+ Generator 函数中，throw 方法捕获错误后，会附带执行下一个 yield 语句

+ throw 方法配合 try...catch 不会影响后续 yield 执行；不配合 try...catch 后续 yield 不再执行

  ```js
  // try...catch 
  var gen = function* gen(){
    try {
      yield console.log('a');
    } catch (e) {
      // ...
    }
    yield console.log('b');
    yield console.log('c');
  }
  
  var g = gen();
  g.next() // a
  g.throw() // b
  g.next() // c
  ```

  ```js
  // 没有 try...catch 
  var gen = function* gen() {
      yield console.log('a');
      yield console.log('b');
      yield console.log('c');
  }
  
  var g = gen();
  g.next() 	// a
  g.throw() // Uncaught undefined
  g.next() 	// 不再执行
  ```

  



## Generator.prototype.return()

Generator函数返回的遍历器对象，有一个`return`方法

+ 对 --  Generator函数返回的对象 -- 设定一个指定的值

+ 并且终止这个  Generator函数

  ```js
  function* gen() {
    yield 1;
    yield 2;
    yield 3;
  }
  
  var g = gen();
  
  g.next()        // { value: 1, done: false }
  g.return('foo') // { value: "foo", done: true }
  g.next()        // { value: undefined, done: true }
  ```

  

+ 如果Generator函数内部有`try...finally`代码块

  `return`方法会推迟到`finally`代码块执行完再执行

  ```js
  function* numbers () {
    yield 1;
    try {
      yield 2;
      yield 3;
    } finally {
      yield 4;
      yield 5;
    }
    yield 6;
  }
  var g = numbers()
  g.next() // { done: false, value: 1 }			--- 执行 yield 1
  g.next() // { done: false, value: 2 }			--- 执行 yield 2
  g.return(7) // { done: false, value: 4 }  
  // --- 不执行后续代码，返回结果为 7，因为存在 finally，需要将 finally 中的执行完毕，再停止。;
  // --- 执行 yield 4
  
  
  g.next() // { done: false, value: 5 }			--- 执行 yield 5
  g.next() // { done: true, value: 7 }
  ```





## yield*语句

+ 在 Generator 函数中调用另一个  Generator 函数是没有效果的

  ```js
  function* a() {
      yield 'a';
  }
  
  function* b() {
      yield 'b';
      a()
      yield 'c';
  }
  
  for (let v of b()) {
      console.log(v);	// b c
  }
  ```

+ yield* : 在 Generator 函数中调用另一个  Generator 函数

  ```js
  function* a() {
      yield 'a';
  }
  
  function* b() {
      yield 'b';
      yield* a()
      yield 'c';
  }
  
  // 等同于
  function* b() {
      yield 'b';
      yield 'a';
      yield 'c';
  }
  
  // 等同于
  function* b() {
      yield 'b';
      for (let v of a()) {
          yield v;
      }
      yield 'c';
  }
  
  for (let v of b()) {
      console.log(v);	//  b a c
  }
  ```

  

## Generator函数的this

## Generator与状态机

## Generator与协程