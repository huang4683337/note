## Object 类型

**什么是对象**

```js
对象其实就是一组数据和功能的集合。
```



**创建对象**

```js
let o = new Object; // 合法，但不推荐

let o = new Object();
```





## 理解 Object

ECMAScript 中的 Object 也是派生其他对象的基类。Object 类型的所有属性和方法在派生的对象上同样存在。

**每个 Object 实例都有如下的属性和方法**

+ *constructor:* 用于创建当前对象的函数。

  在前面的例子中，这个属性的值就是 Object() 函数。

+ *hasOwnProperty(propertyName):* 用于判断当前对象实例(不是原型)上是否存在给定的属

  性。要检查的属性名必须是字符串(如 o.hasOwnProperty("name"))或符号。

+ *isPrototypeOf(object):* 用于判断当前对象是否为另一个对象的原型。

  (第 8 章将详细介绍原型。)

+ *propertyIsEnumerable(propertyName):* 用于判断给定的属性是否可以使用(本章稍后讨

  论的)for-in 语句枚举。与 *hasOwnProperty()* 一样，属性名必须是字符串。

+ *toLocaleString():* 返回对象的字符串表示，该字符串反映对象所在的本地化执行环境。

+ *toString():* 返回对象的字符串表示。

+ *valueOf():* 返回对象对应的字符串、数值或布尔值表示。通常与 toString()的返回值相同。 因为在 ECMAScript 中 Object 是所有对象的基类，所以任何对象都有这些属性和方法



> 严格来讲，ECMA-262 中对象的行为不一定适合 JavaScript 中的其他对象。比如浏 览器环境中的 BOM 和 DOM 对象，都是由宿主环境定义和提供的宿主对象。而宿主对象 不受 ECMA-262 约束，所以它们可能会也可能不会继承 Object。

