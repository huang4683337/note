## Generator 概念

`Generator` 函数是ES6提供的一种异步编程解决方案，语法行为与传统函数完全不同

+ Generator函数是一个状态机，封装了多个内部状态。

+ Generator函数是一个遍历器对象生成函数

  执行Generator函数会返回一个遍历器对象，也就是说，Generator函数除了状态机，还是一个遍历器对象生成函数。返回的遍历器对象，可以依次遍历Generator函数内部的每一个状态。



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

