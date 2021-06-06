## Proxy

Proxy 用于修改某些操作的默认行为，等同于在语言层面做出修改，所以属于一种“元编程”（meta programming），即对编程语言进行编程。

Proxy 可以理解成，在目标对象之前架设一层“拦截”，外界对该对象的访问，都必须先通过这层拦截，因此提供了一种机制，可以对外界的访问进行过滤和改写。Proxy 这个词的原意是代理，用在这里表示由它来“代理”某些操作，可以译为“代理器”。

[参考地址](http://caibaojian.com/es6/proxy.html)

### 语法

```js
// 语法
var proxy = new Proxy(target, handler);

// 参数
/**
 * 
 * @param {*} target		需要拦截的目标对象
 * @param {*} handler 	是一个对象，用来定制拦截行为
 */
```



### 示例

```js
// 对空对象设置一个拦截
var obj = new Proxy({}, {
    get: function (target, key, receiver) {
        console.log(`getting ${key}!`);
        return Reflect.get(target, key, receiver);
    },
    set: function (target, key, value, receiver) {
        console.log(`setting ${key}!`);
        return Reflect.set(target, key, value, receiver);
    }
});

obj.a = 1;	// setting count!

++obj.a;		// getting count!  setting count!
```

上面代码说明，Proxy 实际上重载（overload）了点运算符，即用自己的定义覆盖了语言的原始定义



==注意：==使用 Proxy 时，只能对 Proxy 实例进行操作，并不能对目标对象进行操作，上例就是对实例 obj 进行操作，而不是对布标对象 {} 进行操作。



```js
// 如果参数 handler 中没有定制任何拦截行为, 那么访问结果就是目标对象。
var obj = new Proxy({}, {});

obj.a = 1;

++obj.a;
```



### Proxy 支持的拦截操作

| 属性                                      | 功能                                                         |
| ----------------------------------------- | ------------------------------------------------------------ |
| get( target, propKey, receiver )          | 拦截对象属性的读取，比如`proxy.foo`和`proxy['foo']`。<br />`receiver`  是一个对象 |
| set( target, propKey, value, receiver )   | 拦截对象属性的设置，比如`proxy.foo = v`或`proxy['foo'] = v`，返回一个布尔值。 |
| has(target, propKey)                      | 拦截`propKey in proxy`的操作，以及对象的`hasOwnProperty`方法，返回一个布尔值。 |
| deleteProperty(target, propKey)           | 拦截`delete proxy[propKey]`的操作，返回一个布尔值。          |
| ownKeys(target)                           | 拦截`Object.getOwnPropertyNames(proxy)`<br />`Object.getOwnPropertySymbols(proxy)`、<br />`Object.keys(proxy)`，返回一个数组。<br />该方法返回对象所有自身的属性，而`Object.keys()`仅返回对象可遍历的属性。 |
| getOwnPropertyDescriptor(target, propKey) | 拦截`Object.getOwnPropertyDescriptor(proxy, propKey)`，返回属性的描述对象。 |
| defineProperty(target, propKey, propDesc) | 拦截`Object.defineProperty(proxy, propKey, propDesc）`、`Object.defineProperties(proxy, propDescs)`，返回一个布尔值。 |
| preventExtensions(target)                 | 拦截`Object.preventExtensions(proxy)`，返回一个布尔值。      |
| getPrototypeOf(target)                    | 拦截`Object.getPrototypeOf(proxy)`，返回一个对象。           |
| isExtensible(target)                      | 拦截`Object.isExtensible(proxy)`，返回一个布尔值。           |
| setPrototypeOf(target, proto)             | 拦截`Object.setPrototypeOf(proxy, proto)`，返回一个布尔值。  |
| apply(target, object, args)               | 拦截 Proxy 实例作为函数调用的操作，比如`proxy(...args)`、`proxy.call(object, ...args)`、`proxy.apply(...)`。 |
| construct(target, args)                   | 拦截 Proxy 实例作为构造函数调用的操作，比如`new proxy(...args)`。 |



### Proxy.revocable() 

Proxy.revocable方法返回一个可取消的Proxy实例。

```js
let target = {};
let handler = {};

let {proxy, revoke} = Proxy.revocable(target, handler);

proxy.foo = 123;
proxy.foo // 123

revoke();
proxy.foo // TypeError: Revoked
```

`Proxy.revocable`方法返回一个对象，该对象的`proxy`属性是`Proxy`实例，`revoke`属性是一个函数，可以取消`Proxy`实例。上面代码中，当执行`revoke`函数之后，再访问`Proxy`实例，就会抛出一个错误。





### Proxy 中 this 问题

在 Proxy 代理的情况下，目标对象内部的`this`关键字会指向 Proxy 代理

```js
const target = {
  m: function () {
    console.log(this === proxy);
  }
};
const handler = {};

const proxy = new Proxy(target, handler);

target.m() // false
proxy.m()  // true
```



解决上述问题

```js
const handler = {
    get(target, prop) {
        if (prop === 'm') {
            return target.m.bind(target);
        }
        return Reflect.get(target, prop);
    }
};
```





## Reflect

`Reflect`对象与`Proxy`对象一样，也是ES6为了操作对象而提供的新API。`Reflect`对象的设计目的有这样几个。

（1） 将`Object`对象的一些明显属于语言内部的方法（比如`Object.defineProperty`），放到`Reflect`对象上。现阶段，某些方法同时在`Object`和`Reflect`对象上部署，未来的新方法将只部署在`Reflect`对象上。

（2） 修改某些Object方法的返回结果，让其变得更合理。比如，`Object.defineProperty(obj, name, desc)`在无法定义属性时，会抛出一个错误，而`Reflect.defineProperty(obj, name, desc)`则会返回`false`。

```js
// 老写法
try {
  Object.defineProperty(target, property, attributes);
  // success
} catch (e) {
  // failure
}

// 新写法
if (Reflect.defineProperty(target, property, attributes)) {
  // success
} else {
  // failure
}
```

（3） 让`Object`操作都变成函数行为。某些`Object`操作是命令式，比如`name in obj`和`delete obj[name]`，而`Reflect.has(obj, name)`和`Reflect.deleteProperty(obj, name)`让它们变成了函数行为。

```js
// 老写法
'assign' in Object // true

// 新写法
Reflect.has(Object, 'assign') // true
```

（4）`Reflect`对象的方法与`Proxy`对象的方法一一对应，只要是`Proxy`对象的方法，就能在`Reflect`对象上找到对应的方法。这就让`Proxy`对象可以方便地调用对应的`Reflect`方法，完成默认行为，作为修改行为的基础。也就是说，不管`Proxy`怎么修改默认行为，你总可以在`Reflect`上获取默认行为。

```js
Proxy(target, {
  set: function(target, name, value, receiver) {
    var success = Reflect.set(target,name, value, receiver);
    if (success) {
      log('property ' + name + ' on ' + target + ' set to ' + value);
    }
    return success;
  }
});
```

上面代码中，`Proxy`方法拦截`target`对象的属性赋值行为。它采用`Reflect.set`方法将值赋值给对象的属性，然后再部署额外的功能。

下面是另一个例子。

```js
var loggedObj = new Proxy(obj, {
  get(target, name) {
    console.log('get', target, name);
    return Reflect.get(target, name);
  },
  deleteProperty(target, name) {
    console.log('delete' + name);
    return Reflect.deleteProperty(target, name);
  },
  has(target, name) {
    console.log('has' + name);
    return Reflect.has(target, name);
  }
});
```

上面代码中，每一个`Proxy`对象的拦截操作（`get`、`delete`、`has`），内部都调用对应的Reflect方法，保证原生行为能够正常执行。添加的工作，就是将每一个操作输出一行日志。

有了`Reflect`对象以后，很多操作会更易读。

```js
// 老写法
Function.prototype.apply.call(Math.floor, undefined, [1.75]) // 1

// 新写法
Reflect.apply(Math.floor, undefined, [1.75]) // 1
```

