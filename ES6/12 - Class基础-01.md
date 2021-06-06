## 生成实例的方法

+ 传统方法

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

  

+ ES6 写法

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



**类的数据类型就是函数，类本身就指向构造函数**

```js
// 类的数据类型就是函数，类本身就指向构造函数
console.log( typeof Point );    // function
console.log( Point===Point.prototype.constructor );     // true
```



**类里面所有的方法都定义在类的 prototype 属性上面，除非通过 this.xx 显式定义在本身**

在类上调用某个方法，实际上就是调用原型上的方法

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



因为类的所有方法都定义在 prototype 上，所以新的方法可以添加在 prototype 上。

`Object.assign()`方法可以一次性的向类添加多个方法

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





## constructor 方法

+ class 定义的类，想要执行必须new。

+ `constructor`方法是类的默认方法，在new时会自动执行。

+ 一个类必须有`constructor`方法，如果没有显式定义，一个空的`constructor`方法会被默认添加。



**constructor默认返回对象实例（this），也可以给他指定一个新的对象**

```js
class Foo {
  constructor() {
    return Object.create(null);
  }
}

// 实例对象不是类 Foo 的实例
new Foo instanceof Foo
```

```js
const obj = {
  name: '李四',
  age: '19'
}

class Foo {
  constructor() {
    this.name = '张三';
    return obj;
  }
}

const foo = new Foo();
console.log(foo.age);		// 19
console.log(foo.name);	// 李四
```



## 类实例



**与ES5一样，类的所有实例共享一个原型对象**

```js
var p1 = new Point(2,3);
var p2 = new Point(3,2);

p1.__proto__ === p2.__proto__
//true
```

所以可以通过 `__proto__`  给`类`添加方法，类的所有实例都会享用这个方法，但是有些 JS 引擎中可能没有提供这个私有属性。

```js
p1.__proto__.say = function(){ return 'say-say' };
console.log(foo.say());		// say-say
console.log(foo1.say());	// say-say
```



**建议通过 `Object.getPrototypeOf(p1)` 来获取实例的原型，然后为原型添加方法属性**

```js
const proto = Object.getPrototypeof(p1);

proto.sayHell = function(){ return 'sayHell' }

console.log(foo.sayHell());
```



## 取值函数（getter）和存值函数（setter）

与 `ES5`  一样在 `类` 内部可以使用 `get`、`set`  关键字对某些属性添加存值函数和取值函数，用来拦截该属性的存取行为

```js

```





## 属性表达式

类的属性名，可以采用表达式。

```js
let methodName = 'getArea';

class Square {
  constructor(length) {
    // ...
  }

  [methodName]() {
    // ...
  }
}
```



## Class  表达式

```js
const MyClass = class Me {
  getClassName() {
    return Me.name;
  }
};
```

Me 为 class 的类名，只能在 Class 的内部使用。MyClass 代表这个类

```js
console.log(Me);	// Me is not defined
new Me();	// Me is not defined

new MyClass().getClassName();		// Me
MyClass.name;		// Me
```



class 表达式可以写出立即执行的 class

```js
let person = new class {
  constructor(name) {
    this.name = name;
  }

  sayName() {
    console.log(this.name);
  }
}('张三');

person.sayName(); // "张三"
```



## 注意项

+ 类和模块的内部，默认就是严格模式，所以不需要使用`use strict`指定运行模式。

+ 类不存在变量提升

  ```js
  new Foo(); // ReferenceError
  class Foo {}
  ```

+  name 属性

  ```js
  class Point {}
  Point.name // "Point"
  ```

+ Generator 方法

  ```js
  // 在类中的某个方法前添加 *
  // 这个方法会返回一个遍历器
  // for...of... 会自动调用这个遍历器
  ```

+ this 指向

  ```js
  // 类方法内部的 this 默认指向实例
  ```

  