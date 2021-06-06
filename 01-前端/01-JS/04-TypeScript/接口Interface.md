TypeScript 的核心原则之一，可以对所具有的结构进行类型检查。

Interface 也被称作 `鸭式辨型法`  或 `结构性子类型化` 。



## 简单使用 Interface

**接口中未定义的属性，在使用接口的变量中是不允许出现的**

```js
interface Person {
    name: string;
}

let tom: Person = {
  name: 'Tom',
}
```

上述例子中，定义了一个接口 `Person`，定义了一个变量 `tom`，这个变量的类型是 `Person`。

这样就约束了变量 `tom`的数据格式必须和接口 `Person` 一致，当两者不一致时就会编译报错。

```js
let tom: Person = {
  name: 'Tom',
  age: 18,
}
// 变量 tom 比接口的数据结构多了一个 age ， 报错
```

```js
let tom: Person = {}
// 变量 tom 比接口的数据结构少了 name ， 报错
```



## 可选属性

在开发过程中，有些属性并不是必须的，这些属性只会在某些特定的情况下才会使用。

**可选属性通过 ？来进行甄别，也就是说 `属性名?类型`来表示这个属性可传可不传**

```js
interface Person {
    name: string;
    age?: number;
}

let tom: Person = {
    name: 'Tom'
};
```

```js
interface Person {
    name: string;
    age?: number;
}

let tom: Person = {
    name: 'Tom',
    age: 25
};

```



## 只读属性

当使用者希望在对象刚刚创建时被赋值，那么可以使用`readonly`来定义这个属性。

```js
interface Person {
    readonly id: number;
}

let tom: Person = {
    id: 123,	// 在 tom 这个对象被创建时赋值为 123
}

tom.id = 111;	// 想重新给 id 赋值时，报错。 因为只读属性只能在对象刚刚创建时对属性赋值
```
