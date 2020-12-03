## Iterator（遍历器）的概念

遍历器（Iterator）是一种接口，为各种不同的数据结构提供统一的访问机制  `for...of`。

任何数据结构只要部署Iterator接口，就可以完成遍历操作（即依次处理该数据结构的所有成员）

+ 为各种数据结构，提供一个统一的、简便的访问接口
+ 使得数据结构的成员能够按某种次序排列
+ ES6创造了一种新的遍历命令`for...of`循环，Iterator接口主要供`for...of`消费



## Iterator的简单实现

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



## TS写法

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





## 数据结构的默认 Iterator 接口

>ES6 规定，默认的 Iterator 接口部署在数据结构的 `Symbol.iterator` 属性，也就是说一个数据结构只要具有`Symbol.iterator`属性，就可以认为是“可遍历的”（iterable）。
>
>`Symbol.iterator` 属性本身是一个函数，就是当前数据结构默认的遍历器生成函数。执行这个函数，就会返回一个遍历器。
>
>属性名 `Symbol.iterator`，它是一个表达式，返回 `Symbol` 对象的 `iterator` 属性，这是一个预定义好的、类型为 Symbol 的特殊值，所以要放在方括号内。

总结如下：

+  `Symbol.iterator`  是数据结构的一个属性，例如 `obj[Symbol.iterator]`
+  `Symbol.iterator`  本身是一个函数，执行函数会返回当前数据结构的遍历器

+ 遍历器就是一个可以通过 next 方法访问数据结构的下一个数据



```javascript
const obj = {
    [Symbol.iterator]: function () {
        return {
            next: function () {
                return {
                    value: 1,
                    done: true
                };
            }
        };
    }
};
const it = obj[Symbol.iterator]()
console.log(it.next());		// { value: 1, done: true }

// 1 - obj 拥有一个 `Symbol.iterator` 属性
// 2 - 这个 `Symbol.iterator` 属性是一个函数
// 3 - 执行 obj[Symbol.iterator] 会返回一个遍历器 it
// 4 - 通过遍历器 it 可以访问 next
```



### ES6 中原生俱备Iterator接口的数据结构

```js
// ES6 中原生俱备Iterator接口的数据结构 数组、某些类似数组的对象、Set和Map结构。

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



### 对象没有默认部署 Iterator 接口

是因为对象的哪个属性先遍历，哪个属性后遍历是不确定的，需要开发者手动指定。

本质上，遍历器是一种线性处理，对于任何非线性的数据结构，部署遍历器接口，就等于部署一种线性转换。

ES6 中对象部署遍历器接口并不是很必要，对象实际上被当作 Map 结构使用，我们完全可以使用 Map 结构。







## 调用Iterator接口的场合



## 字符串的Iterator接口



## Iterator接口与Generator函数



## 遍历器对象的return()，throw()



## for...of循环

