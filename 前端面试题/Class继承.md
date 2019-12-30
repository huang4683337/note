# Class 继承

Es5继承的本质是：先创造子类的实例对象 this，然后将父类的方法添加到this上面`(Parent.call(this))`。

Es6继承到本质是：先将父类实例对象的属性和方法，添加到this上`在子类的constructor中调用super`，然后再用子类的构造函数修改this。

## extends

通过`extends`关键字，实现继承父类。

```js
class Parent extends Child{
  
}
```



## Object.getPrototypeOf()

从子类上获取父类，用来判断子类继承了哪个父类

```js
class Parent {
    constructor() {
    }
}

class Child extends Parent {
    constructor() {
        super();
    }
}

console.log( Object.getPrototypeOf(Child) === Parent );  // true
```



## super

> 可以当作函数使用：代表的是父类的构造函数。只能存在`子类的构造函数最顶层`
>
> 可以当作对象使用：在普通方法中，指向父类的原型对象。在静态方法中指向父类。

**super当作函数**

```js
class A {}

class B extends A {
  constructor() {
    super();
  }
}

// super 不在构造函数中时 报错
class B extends A {
  say() {
    super();
  }
}
// 'super' keyword unexpected here
```

`super`代表了父类A的构造函数，返回的是子类B的实例。`super`内部的this指向B的实例。

`super()`相当于在B函数中`A.prototype.constructor.call(this)`。



**super当作对象**

