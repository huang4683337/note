## Iterator（遍历器）的概念

遍历器（Iterator）是一种接口，为各种不同的数据结构提供统一的访问机制 。



## Iterator（遍历器）的作用

+ 为各种数据结构，提供一个统一的、简便的访问接口
+ 使得数据结构的成员能够按某种次序排列（依次处理该数据结构的所有成员）
+ ES6创造了一种新的遍历命令`for...of`循环，Iterator接口主要供`for...of`消费



## Iterator（遍历器）的简单实现

+ 创建一个指针对象 ，指向当前数据结构的起始位置
+ 通过不断调用 `next` 方法，将指针指向后续的数据结构成员，直至数据结构的结束位置

```js

/**
 * value: 当前成员的值
 * done: 遍历是否结束
 */

const arr = [1, 2]

function creatIterator(arr) {
    var nextIndex = 0;
    return {
        next: function () {
            return nextIndex < arr.length ? { value: arr[nextIndex++], done: false } : { value: undefined, done: true };
        }
    };
}

const it = creatIterator(arr);

console.log(it.next());
console.log(it.next());

// 每次调用 next 就会访问数组 arr 的下一个索引
```



## ES6 原生具备Iterator接口的数据结构

+ 数组
+ 某些类似数组的对象
+ Set和Map结构。



### 数组

```js
let arr = ['a', 'b'];
let iter = arr[Symbol.iterator]();

console.log(iter.next());
console.log(iter.next());
console.log(iter.next());

// arr 是一个数组，原生具备遍历器接口，部署在 arr 的 Symbol.iterator 属性上
// 执行 arr 的 Symbol.iterator 方法，就可以获得一个遍历器对象 
```

```js
// 具有 Iterator 接口的数据结构，可以使用 for...of 遍历
for(let i of arr){
    console.log(i); // a b
}
```



### 某些类似数组的对象

```js
// 字符串
let str = "hello";

for (let s of str) {
  console.log(s); // h e l l o
}

// DOM NodeList对象
let paras = document.querySelectorAll("p");

for (let p of paras) {
  p.classList.add("test");
}

// arguments对象
function printArgs() {
  for (let x of arguments) {
    console.log(x);
  }
}
printArgs('a', 'b');
// 'a'
// 'b'
```



### Set和Map结构

```js
// Set 结构
const setArr = new Set([1,2,2,3,4])

for (let e of setArr) {
  console.log(e);	// 1 2 3 4
}
```

```js
// Map 结构
const mapData = new Map();
mapData.set('a','a1');
mapData.set('b','b1');

for (let [name, value] of es6) {
  console.log(name + ": " + value);	// a:a1 b:b1
}
```



## 对象没有默认部署 Iterator 接口

是因为对象的哪个属性先遍历，哪个属性后遍历是不确定的，需要开发者手动指定。

本质上，遍历器是一种线性处理，对于任何非线性的数据结构，部署遍历器接口，就等于部署一种线性转换。

ES6 中对象部署遍历器接口并不是很必要，对象实际上被当作 Map 结构使用，我们完全可以使用 Map 结构。





### 添加 Iterator 接口

+ 对于类似数组的对象（存在数值键名和length属性），部署Iterator接口，有一个简便方法，就是`Symbol.iterator`方法直接引用数组的Iterator接口。

  ```js
  NodeList.prototype[Symbol.iterator] = Array.prototype[Symbol.iterator];
  // 或者
  NodeList.prototype[Symbol.iterator] = [][Symbol.iterator];
  
  [...document.querySelectorAll('div')] // 可以执行了
  ```

  ```js
  // 举例
  let iterable = {
    0: 'a',
    1: 'b',
    2: 'c',
    length: 3,
    [Symbol.iterator]: Array.prototype[Symbol.iterator]
  };
  for (let item of iterable) {
    console.log(item); // 'a', 'b', 'c'
  }
  ```

+ 普通对象需要在当前数据结构上添加 `Symbol.iterator`  属性和对应的遍历方法

  ```js
  let obj = {
    data: [ 'hello', 'world' ],
    [Symbol.iterator]() {
      const self = this;
      let index = 0;
      return {
        next() {
          if (index < self.data.length) {
            return {
              value: self.data[index++],
              done: false
            };
          } else {
            return { value: undefined, done: true };
          }
        }
      };
    }
  };
  ```

  ```js
  function Obj(value) {
    this.value = value;
    this.next = null;
  }
  
  Obj.prototype[Symbol.iterator] = function() {
    var iterator = {
      next: next
    };
  
    var current = this;
  
    function next() {
      if (current) {
        var value = current.value;
        current = current.next;
        return {
          done: false,
          value: value
        };
      } else {
        return {
          done: true
        };
      }
    }
    return iterator;
  }
  
  var one = new Obj(1);
  var two = new Obj(2);
  var three = new Obj(3);
  
  one.next = two;
  two.next = three;
  
  for (var i of one){
    console.log(i);
  }
  // 1
  // 2
  // 3
  
  ```

  



## 遍历器对象的return()，throw()

+ `next()` 是必须的，通过执行  `next`  来访问数据结构的下一个值
+ `return()` : 出错、`brew语句`、`continue语句` 时，通过 `return()`清理或者释放资源
+ `throw()` 



### return 方法

```js
function readLinesSync(file) {
  return {
    next() {
      if (file.isAtEndOfFile()) {
        file.close();
        return { done: true };
      }
    },
    return() {
      file.close();
      return { done: true };
    },
  };
}


for (let line of readLinesSync(fileName)) {
  console.log(line);
  break;
}
```



### throw 方法

`throw`方法主要是配合Generator函数使用，一般的遍历器对象用不到这个方法。请参阅《Generator函数》一章。





## 调用Iterator接口的场合

+ 解构赋值

  对数组和Set结构进行解构赋值时，会默认调用`Symbol.iterator`方法。

  ```js
  let set = new Set().add('a').add('b').add('c');
  
  let [x,y] = set;
  // x='a'; y='b'
  
  let [first, ...rest] = set;
  // first='a'; rest=['b','c'];
  ```

  

+ 扩展运算符

  扩展运算符（...）也会调用默认的iterator接口。

  ```js
  // 例一
  var str = 'hello';
  [...str] //  ['h','e','l','l','o']
  
  // 例二
  let arr = ['b', 'c'];
  ['a', ...arr, 'd']
  // ['a', 'b', 'c', 'd']
  ```

  只要某个数据结构部署了Iterator接口，就可以对它使用扩展运算符，将其转为数组

  ```js
  let arr = [...iterable];
  ```



+ yield*

  `yield*`后面跟的是一个可遍历的结构，它会调用该结构的遍历器接口。

  ```js
  let generator = function* () {
    yield 1;
    yield* [2,3,4];
    yield 5;
  };
  
  var iterator = generator();
  
  iterator.next() // { value: 1, done: false }
  iterator.next() // { value: 2, done: false }
  iterator.next() // { value: 3, done: false }
  iterator.next() // { value: 4, done: false }
  iterator.next() // { value: 5, done: false }
  iterator.next() // { value: undefined, done: true }
  ```



+ 其他场合

  由于数组的遍历会调用遍历器接口，所以任何接受数组作为参数的场合，其实都调用了遍历器接口。下面是一些例子。

  ```js
  // for...of
  // Array.from()
  // Map(), Set(), WeakMap(), WeakSet()（比如new Map([['a',1],['b',2]])）
  // Promise.all()
  // Promise.race()
  ```

  





## Iterable 的 TS写法

遍历器接口（Iterable）、指针对象（Iterator）和 next 方法返回值的规格可以描述如下

```javascript
interface Iterable {
  [Symbol.iterator]() : Iterator,
}

interface Iterator {
  next(value?: any) : IterationResult,
}

interface IterationResult {
  value: any,
  done: boolean,
}
```
