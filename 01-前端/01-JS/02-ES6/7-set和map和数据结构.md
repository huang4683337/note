

## set 数据结构

### 概念

> 类似于数组结构 
>
> 值唯一：在set结构中不会出现相同的值

```js
const s = new Set();

[2, 3, 5, 4, 5, 2, 2].forEach(x => s.add(x));

for (let i of s) {
  console.log(i);
}
// 2 3 5 4


const set = new Set([1, 2, 3, 4, 4]);
console.log([...set])	// [1, 2, 3, 4]
set.size	// 4
```



### set 操作方法

```js
const set = new Set();

// Set.prototype.add(value)：添加某个值，返回 Set 结构本身
set.add(1).add(2); // set(2) {1,3}

// Set.prototype.delete(value)：删除某个值，返回一个布尔值，表示删除是否成功
set.delete(1);	// true

// Set.prototype.has(value)：返回一个布尔值，表示该值是否为Set的成员
set.has(1);	// true

// Set.prototype.clear()：清除所有成员，没有返回值。
set.clear();
```



### set 使用 

```js
// 数组去重
[...new set([1,2,3,3,3,3])]; // 使用展开操作符将set结构放入一个新的数组中
Array.from(new Set(array)); // 利用set值的唯一性，然后利用 Array.from转成数组


// 字符串去重
[...new Set('ababbc')].join('')
```

> 向 set 中插入值的时候不会发生数据类型的转换
>
> set 中判断两个值是否相等类似于 `===`   但是在set 中 NaN===NaN





### set 实例属性

```js
// Set.prototype.constructor：构造函数，默认就是Set函数。
// Set.prototype.size：返回Set实例的成员总数
```





### set 遍历方法,配合 for...of

```js
const set = new Set(['a','b','c','d']);

// Set.prototype.keys()：返回键名的遍历器
for(let key of set.keys()){
  console.log(key);	// a b c d
}

// Set.prototype.values()：返回键值的遍历器
for (let value of set.values()) {
  console.log(value);	// a b c d
}
for (let value of set) {
  console.log(value);	// a b c d
}

// Set.prototype.entries()：返回键值对的遍历器
for (let ent of set.entries()) {
  console.log(ent);	// ['a','a'] ['b','b']...
}

// Set.prototype.forEach()：使用回调函数遍历每个成员
set.forEach( (value, key)=>{
  console.log(`${key}:${value}`);	// a:a. b:b ...
} )
```

```js
let a = new Set([1, 2, 3]);
let b = new Set([4, 3, 2]);

// 并集
let union = new Set([...a, ...b]);
// Set {1, 2, 3, 4}

// 交集
let intersect = new Set([...a].filter(x => b.has(x)));
// set {2, 3}

// 差集
let difference = new Set([...a].filter(x => !b.has(x)));
// Set {1}
```





## WeakSet

> WeakSet 结构和 set 类似。两者的区别在于 WeakSet 中的成员只能是对象

> WeakSet 中的对象都是弱引用，垃圾回收机制不会考虑WeakSet对于对象的引用。
>
> WeakSet 适合临时存放一组对象、或者是跟对象绑定的信息。一般用来存储DOM节点。
>
> 因为垃圾回收机制不考虑WeakSet，所以规定WeakSet不可遍历

```js
// WeakSet 是一个构造函数 所以可以使用 new 操作符
const ws = new WeakSet();
```

```js
// WeakSet 中的成员只能是对象 或者是具有 interator 接口的
new WeakSet(1); // number 1 is not iterable 

new WeakSet([1,2]); 
// Invalid value used in weak set(…)  数组成员中的 1 2 不是对象

new WeakSet([[1,3],[1,3]]); // WeakSet {[1, 3], [1, 3]}
```

### WeakSet 实例方法

```js
const ws = new WeakSet();
const obj = {a:'name'}

// WeakSet.prototype.add(value)：向 WeakSet 实例添加一个新成员。
ws.add(obj);

// WeakSet.prototype.delete(value)：清除 WeakSet 实例的指定成员
ws.delete(obj); 

// WeakSet.prototype.has(value)：返回一个布尔值，表示某个值是否在 WeakSet 实例之中。
ws.has(obj);
```



## Map 数据结构

> 传统的字符串只能用字符串当作键。在Map结构中任何数据类型都可以当作键来使用。



### Map 操作方法

+ 创建一个 Map   ==> `cosnt map = new Map([ [key,value] ])`
+ 给 Map 添加值   ==> `map.set(key,value)`
+ 获取值  ==> `map.get(key)`



### Map 使用

**Map 接收一个数组作为参数,数组的成员是一个表示键值对的数组 => [ [key,value], [key,value] ]**

```js
// Map 接收一个数组作为参数,数组的成员是一个表示键值对的数组 => [ [key,value], [key,value] ]

let arr = [ ['name','张三'], ['age', 18] ];
const map = new Map( arr );	// Map(2) {'name'=>'张三', 'age'=>18}


// 实现原理
const items = [
  ['name', '张三'],
  ['title', 'Author']
];

const map = new Map();

items.forEach(
  ([key, value]) => map.set(key, value);  // [key,value]使用了解构赋值
);
```



### 用一个变量去当作键

```js
const m = new Map();
const o = {p: 'Hello World'};

m.set(o, 'content')
m.get(o) // "content"
```

**使用复杂类型(引用类型)作为键时 ，应注意 Map 的键是跟内存的地址进行绑定的**

```js
const map = new Map();

// 因为两个 ['a'] 相当于两个数组实例，所以他们在内存中的地址不一样
map.set(['a'], 555);
map.get(['a']) // undefined

// 可以修改为
let a = ['a'];
map.set(a,555);
map.get(a); // 555

/********************************************************/

//  k1、k2 是在内存中的地址是两个
const k1 = ['a'];
const k2 = ['a'];
map.set(k1, 111).set(k2, 222);

map.get(k1) // 111
map.get(k2) // 222
```

**对于简单数据类型来说只要严格相等的两个值，Map会将其视为一个键，所有的NaN也会视为一个键**

```js
let map = new Map();

map.set(-0, 123);
map.get(+0) // 123

map.set(true, 1);
map.set('true', 2);
map.get(true) // 1

map.set(undefined, 3);
map.set(null, 4);
map.get(undefined) // 3

map.set(NaN, 123);
map.get(NaN) // 123
```





### Map 实例的属性

**size属性** ： 返回Map属性的成员数

```js
const map = new Map();
map.set('foo', true);
map.set('bar', false);

map.size // 2
```

**Map.prototype.set(key, value)**

返回为新的Map结构，如果key有值，则key对应的值更新。否则生成新的键值

```js
const m = new Map();

m.set('edition', 6)        // 键是字符串
m.set(262, 'standard')     // 键是数值
m.set(undefined, 'nah')    // 键是 undefined
```

**可以采用链式写法**

```js
let map = new Map()
  .set(1, 'a')
  .set(2, 'b')
  .set(3, 'c');
```

