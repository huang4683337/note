## Class基础

### Class 基本用法

**Es5写法**

```js
function Point(x,y){
    this.x = x;
    this.y = y;
}

Point.prototype = {
    say(){
        console.log(this.x,this.y)
    }
}

var p = new Point(1,3);
p.say();
```

**使用class后**

```js
class Point {
    constructor(x, y) {
        this.x = x;
        this.y = y;
    }
    say() {
        console.log(this.x, this.y)
    }
}

var p = new Point(1,2);
p.say();
```

```js
// 类的数据类型就是函数，类本身就指向构造函数
console.log( typeof Point );    // function
console.log( Point===Point.prototype.constructor );     // true
```

```js
// 类里面所有的方法都定义在类的 prototype 属性上面

class Point {
  constructor(){}
  say(){}
  num(){}
}

// 等效于

Point.prototype = {
  constructor(){},
  say(){},
  num(){}
}
```

**一次性向类中添加多个方法**

```js
class Point {
  constructor(){
    // ...
  }
}

Object.assign(Point.prototype, {
  toString(){},
  toValue(){}
});
```

### constructor 方法

class 定义的类，想要执行必须new。

`constructor`方法是类的默认方法，在new时会自动执行。

一个类必须有`constructor`方法，如果没有显式定义，一个空的`constructor`方法会被默认添加。



### 取值函数（getter）和存值函数（setter）

```js

```



### Generator 方法

一般情况下函数只要开始执行，除非 return 或者执行完毕，除此之外是不会停止的。

Generator 函数是ES6提供的一种异步编程解决方案，可以打断函数的执行

+ 如果某个方法之前加上星号（`*`），就表示该方法是一个 Generator 函数
+ 函数体内使用 yield 表达式，定义不同的内部状态 （可以有多个yield）
+ 直接调用 Generator函数并不会执行，也不会返回运行结果，而是返回一个遍历器对象（Iterator Object）
+ 次调用遍历器对象的next方法，遍历 Generator函数内部的每一个状态

```js
function * generator(){
  yield 'status one'         // yield 表达式是暂停执行的标记  
  return 'hello world'
}

let g = generator();

console.log('直接执行 Generator 函数，得到一个遍历器',g) // Object [Generator] {}

console.log(g.next())	// status one
console.log(g.next()) // hello world
```



## 静态方法 

类相当于实例的原型，在类中定义的方法，都会被实例继承。

使用 `static` 修饰符修饰的方法称为静态方法，它们不需要实例化，而是直接通过类来调用。

```js
class Point{
    constructor(){

    }
    static say(){
        console.log('sayFunction');
    }
}

Point.say(); // sayFunction , 类直接调用static修饰的方法

var  p = new Point();
p.say();  // p.say is not a function , static修饰的方法不会被实例继承
```

> 父类的静态方可以被子类继承

```js
class Point {
    constructor() {
    }
    static say(){
        console.log('parent_static');
    }
}


class Child extends Point{

}

Child.say();
```



**静态方法中的 this 指向类**   、**静态方法和非静态的方法可以重名**

```js
class Point{
    constructor(){
        this.x = '1'
    }
    static say(){
        console.log(this);
    }
    say(){
        console.log(this)
    }
}

Point.say();	// [Function: Point]  => this指向 类定义的Point整个方法

var  p = new Point();
p.say(); // Point { x: '1' }  => this指向实例对象
```



## 静态属性

Class 本身的属性，不是定义在实例对象`(this)`上的属性

```js
class Point{

}

Point.prop = 1;
console.log(Point);	// { [Function: Point] prop: 1 }

// Es6 中可以在属性前加上 static 声明是一个静态属性
class Point{
    static prop = 1;
}
console.log(Point);	// 1
```



## 实例属性的新写法

```js
class Point {
    constructor() {
        this.x = '1'
    }
}

// 实例属性也可以写在 constructor 上面

class Point {
    bar = 'hello';
    baz = 'world';
    
    constructor() {
    }
}
var p = new Point()

p.bar;  // 'hello'
p.baz;  // 'world'
```



## 私有方法和私有属性

目前有一个提案，就是给在属性名 或者 方法 前加上 `#` 证明是私有的

```js
class Point{
    constructor(){
        this.#prop = 'private';
    }

    #say(){
        console.log('privateFn');
    }
}

// 没进行测试
```





## new.target 属性

> 在函数内部使用。判断一个函数是否被实例化。
>
> 如果函数不是通过`new`命令或`Reflect.construct()` ，new.target 会返回一个 undefinde

```js
function Person(name) {
  if (new.target !== undefined) {
    this.name = name;
  } else {
    throw new Error('必须使用 new 命令生成实例');
  }
}

// 另一种写法
function Person(name) {
  if (new.target === Person) {
    this.name = name;
  } else {
    throw new Error('必须使用 new 命令生成实例');
  }
}

var person = new Person('张三'); // 正确
var notAPerson = Person.call(person, '张三');  // 报错
```



> 在继承时 new.target 会指向子类。
>
> 利用这个特点可以实现，不能独立使用，必须通过继承后才能使用的类

```js
class Parent{
    constructor(){
        console.log(new.target);
    }
}

class Child extends Parent{
    constructor(){
        super();
    }
}

var child = new Child();  // [Function: Child]
```



**实现只能继承使用的类**

```js
class Parent {
    constructor() {
        if (new.target === Parent) {
            throw new Error('本类不能实例化');
        }
    }
}

class Child extends Parent {
    constructor() {
        super();
    }
}

var p = new Parent(); // Error: 本类不能实例化
```

