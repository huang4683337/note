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





### Proxy 中 this 问题





## Reflect

