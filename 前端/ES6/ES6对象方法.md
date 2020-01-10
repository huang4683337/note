

# ES6 对象方法

## Object.is()

> 比较两个值是否严格相等 === 。
>
> 解决了es5 NaN===NaN 为false的问题

```js
Object.is('foo', 'foo')		// true
Object.is(+0, -0) 	// false 一个数的正负是不相等的
Object.is(NaN, NaN) // true


// 两个空数组或者对象是不相等的
Object.is({}, {})		// false 
Object.is([], [])		// false
```

```js
// 兼容
Object.defineProperty(Object, 'is', {
  value: function(x, y) {
    if (x === y) {
      // 针对+0 不等于 -0的情况
      return x !== 0 || 1 / x === 1 / y;
    }
    // 针对NaN的情况
    return x !== x && y !== y;
  },
  configurable: true,
  enumerable: false,
  writable: true
});
```

## Object.assign() 浅拷贝

> 主要用于对象的合并。 将源对象（source）的所有`可枚举的属性`，复制到目标对象（target）。

> 只拷贝源对象的自身属性（不拷贝继承属性），也不拷贝不可枚举的属性（`enumerable: false`）

> Object.assign( target, source, source )

```js
const target = { a: 1 };

const source1 = { b: 2 };
const source2 = { c: 3 };

Object.assign(target, source1, source2);
console.log(target)	 // {a:1, b:2, c:3}
```

```js
// 若目标对象和源对象存在相同的属性名，后面的会 覆盖 前面的。
const target = { a: 1 };
const source = { a: 2, b:4 };
Object.assign( target, source );
console.log(target); // {a: 2, b: 4}
```

```js
/*
第一个 参数不是对象 则会自动先转成对象
第二个 参数不是对象 不会自动转
*/ 

typeof Object.assign(2) // "object"  

// null nudefined 无法转成对象 会报错
Object.assign(undefined) // 报错
Object.assign(null) // 报错
```

```js
// 字符串拷贝结果。 因为字符串可枚举
Object.assign({},'hell'); //{0: "h", 1: "e", 2: "l", 3: "l"}
```

```js
// 对于数组的处理 , 把数组的索引当作对象的key
Object.assign([1, 2, 3], [4, 5])	// [4, 5, 3]
//等效于
Object.assign({0:1,1:2,2:3},{0:4,1:5});
```

```js
// 处理取值函数 => 将函数的结果进行复制
const source = {
  get foo() { return 1 }
};
const target = {};

Object.assign(target, source)
// { foo: 1 }

/*
上面代码中，source对象的foo属性是一个取值函数，Object.assign不会复制这个取值函数，只会拿到值以后，将这个值复制过去。
*/
```

### 为对象增加属性

```js
class Point {
  constructor(x, y) {
    Object.assign(this, {x, y});
  }
}
/*
上面方法通过Object.assign方法，将x属性和y属性添加到Point类的对象实例。
*/
```

### 为对象增加方法

```js
Object.assign(SomeClass.prototype, {
  someMethod(arg1, arg2) {
    ···
  },
  anotherMethod() {
    ···
  }
});

// 等同于下面的写法
SomeClass.prototype.someMethod = function (arg1, arg2) {
  ···
};
SomeClass.prototype.anotherMethod = function () {
  ···
};
```

### 克隆对象及其继承的值

```js
function clone(origin) {
  let originProto = Object.getPrototypeOf(origin);
  return Object.assign(Object.create(originProto), origin);
}
```

### 合并多个对象

```js
const merge =
  (target, ...sources) => Object.assign(target, ...sources);

//合并后返回一个新对象
const merge =
  (...sources) => Object.assign({}, ...sources);
```



## Object.getOwnPropertyDescriptors()

> 返回指定对象所有自身属性（非继承属性）的描述对象。
>
> 主要目的是为了解决 Object.assign 无法正确拷贝get属性和set属性的问题

```js
const obj = {
  foo: 123,
  get bar() { return 'abc' }
};

Object.getOwnPropertyDescriptors(obj);
// { foo:
//    { value: 123,
//      writable: true,
//      enumerable: true,
//      configurable: true },
//   bar:
//    { get: [Function: get bar],
//      set: undefined,
//      enumerable: true,
//      configurable: true } }
```

```js
// 实现 Object.getOwnPropertyDescriptors()
function getOwnPropertyDescriptors(obj) {
  const result = {};
  for (let key of Reflect.ownKeys(obj)) {
    result[key] = Object.getOwnPropertyDescriptor(obj, key);
  }
  return result;
}
```

### 配合 Object.create 实现浅拷贝

```js
const clone = Object.create(Object.getPrototypeOf(obj),
  Object.getOwnPropertyDescriptors(obj));

// 或者

const shallowClone = (obj) => Object.create(
  Object.getPrototypeOf(obj),
  Object.getOwnPropertyDescriptors(obj)
);
```

### 一个继承另一个对象

```js
const obj = Object.create(
  prot,
  Object.getOwnPropertyDescriptors({
    foo: 123,
  })
);
```

## \__prop__

> 用来读取或设置当前对象的`prototype`对象

```js
// es5 的写法
const obj = {
  method: function() { ... }
};
obj.__proto__ = someOtherObj;

// es6 的写法
var obj = Object.create(someOtherObj);
obj.method = function() { ... };
```

## Object.setPrototypeOf()

> 用来设置一个对象的`prototype`对象，返回参数对象本身

```js
// 格式
Object.setPrototypeOf(object, prototype)

// 用法
const o = Object.setPrototypeOf({}, null);

// 等同于
function setPrototypeOf(obj, proto) {
  obj.__proto__ = proto;
  return obj;
}
```

```js
let proto = {};
let obj = { x: 10 };
Object.setPrototypeOf(obj, proto);

proto.y = 20;
proto.z = 40;

obj.x // 10
obj.y // 20
obj.z // 40
```

## Object.getPrototypeOf() 

> 该方法与`Object.setPrototypeOf`方法配套，用于读取一个对象的原型对象



## Object.keys()，Object.values()，Object.entries()

> 配合 for...of 使用  返回一个数组

```js
// Object.keys(Object);
var obj = { foo: 'bar', baz: 42 };
Object.keys(obj)
// ["foo", "baz"]
```

```js
// Object.values(Object)
const obj = { foo: 'bar', baz: 42 };
Object.values(obj)

// 如果属性名为数值的话。返回的结果会按照数值排列
const obj = { 100: 'a', 2: 'b', 7: 'c' };
Object.values(obj)	// ["b", "c", "a"]


// 会过滤属性名为 Symbol 值的属性。
Object.values({ [Symbol()]: 123, foo: 'abc' });	// ['abc']

// 值为字符串
Object.values('foo')	// ['f', 'o', 'o']
```

```js
// Object.entries(Object)
const obj = { foo: 'bar', baz: 42 };
Object.entries(obj)	// [ ["foo", "bar"], ["baz", 42] ]

// 会过滤属性名为 Symbol 值的属性。
Object.entries({ [Symbol()]: 123, foo: 'abc' });	// [ [ 'foo', 'abc' ] ]
```

```js
// 自己实现Object.entries

// Generator函数的版本
function* entries(obj) {
  for (let key of Object.keys(obj)) {
    yield [key, obj[key]];
  }
}

// 非Generator函数的版本
function entries(obj) {
  let arr = [];
  for (let key of Object.keys(obj)) {
    arr.push([key, obj[key]]);
  }
  return arr;
}
```

## Object.fromEntries()

> Object.entries 的逆向操作。将建值对的数组转成对象。 主要用于处理map对象

```js
Object.fromEntries([
  ['foo', 'bar'],
  ['baz', 42]
])
// { foo: "bar", baz: 42 }
```

```js
/*
配合 URLSearchParams 对象，将查询字符串转为对象
*/
Object.fromEntries(new URLSearchParams('foo=bar&baz=qux')) // { foo: "bar", baz: "qux" }
```

