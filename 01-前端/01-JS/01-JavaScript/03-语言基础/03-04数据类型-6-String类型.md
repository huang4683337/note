String(字符串)数据类型表示零或多个 16 位 Unicode 字符序列。



## 字符串的特点

ECMAScript 中的字符串是不可变的(immutable)，意思是一旦创建，它们的值就不能变了。

要修改 某个变量中的字符串值，必须先销毁原始的字符串，然后将包含新值的另一个字符串保存到该变量。

```js
let lang = "Java";
lang = lang + "Script";
```

```
1 - lang 变量存储 Java
2 - 开辟 10 个字符的空间，将 Java 、Script 放入，合成 JavaScript
3 - 删除 Java 、Script 这两个字符串
```



## xx.toString() 转换为字符串

+ 数据类型转字符串，默认返回**十进制**字符串表示，可以通过是设置**参数**来控制转化成**几进制**的字符串表示

+ `null`、`undefined` 没有 `toString()`

```js
// 数据类型转字符串，默认转化 10 进制字符串表示

let params = 1;
console.log(params.toString()); // '1'

let params = true;
console.log(params.toString()); // 'true'

let params = '123';
console.log(params.toString()); // '123'  返回字符串副本
```



```js
// 转化成 几进制 字符串表示
let num = 10;
console.log(num.toString());    // "10" 默认十进制表示
console.log(num.toString(2));   // "1010" 2进制表示
console.log(num.toString(8));   // "12" 8进制表示
console.log(num.toString(10));  // "10" 十进制表示
console.log(num.toString(16));  // "a"  16进制表示
```



## String(xx) 转换为字符串

+ 如果该数据类型存在 `toStirng()`，就调用 `toString()`

+ `undefined`、`null`  调用 `String()`

```js
console.log(String(null));	// "null"
console.log(String(undefined)); // "undefined"
```





## 模板字面量

+ 模板字面量不是字符串，是一种特殊的 JavaScript 表达式，求值后得到一个字符串。

+ 模板字面量会保留换行符，可以**跨行定义字符串**，可以用来定义 html 模板

```js
let pageHTML = ` 
<div>
  <a href="#">
    <span>Jake</span>
  </a> 
</div>`;
```

模板字面量会保持反引号内部的空格, 所以书写时建议根据实际需要格式书写，格式从边量定义的第二格开始。

```
console.log(pageHTML);

<div>
  <a href="#">
    <span>Jake</span>
  </a> 
</div>
```





## 字符串插值

**模板字面量**最常用的一个特性是**支持字符串插值**，也就是可以**在一个连续定义中插入一个或多个值**

模板字面量不是字符串，是一种特殊的 JavaScript 表达式，求值后得到一个字符串。

模板字面量在定义时立即求值并转换为字符串实例，任何插入的变量也会从它们最接近的作用域中取值。



**字符串插值通过在${}中使用一个 JavaScript 表达式实现**

```js
let name = "Tom";

let where = 'eat';
let str = `${name} 在 ${where} 饭饭`;
console.log(str);   // Tom 在 eat 饭饭
```



**所有插入的值都会使用 toString 强制转型为字符串。**

```js
let foo = { toString: () => 'World' };
console.log(`Hello, ${foo}!`); // Hello, World!
```

```js
let a = foo.toString();
console.log(a);	// World
```



**在插值表达式中可以调用函数和方法**

```js
function strFn(params) {
    return params;
}

console.log(`${strFn('插值中调用函数')}`);

// 插值中调用函数
```



**模板中也可以插入自己之前的值**

```js
let value = '';
function append() {
    value = `${value}abc`
    console.log(value);
}
append();  // abc
append();  // abcabc
append();  // abcabcabc
```



## 模板字面量标签函数

函数接收两种参数：

+ 第一个参数：原始字符串，除字符串插值以外的字符

  每个插值之间默认原始字符串`''`

+ 后续参数：每个参数对应一个字符串插值



```js
// 插值和 `` 之间默认 通过空的字符串（''） 进行填充
let a = 6;
function simpleTag(strings, params1) {
    console.log(strings);   // [ '', '' ]
    console.log(params1);   // 6
}

let taggedResult = simpleTag`${a}`;
```

```js
// 原始字符串有对应信息时，（插值两边有字符）
let a = 6;
function simpleTag(strings, params1) {
    console.log(strings);   // [ 'QQ:', '' ]
    console.log(params1);   // 6
}

let taggedResult = simpleTag`QQ:${a}`;

// or

let a = 6;
function simpleTag(strings, params1) {
    console.log(strings);   // [ 'QQ:', 'WX' ]
    console.log(params1);   // 6
}

let taggedResult = simpleTag`QQ:${a}WX`;

```



**使用扩展运算符收集所有参数**

```js
let a = 6;
let b = 9;
function simpleTag(strings, ...params) {
    console.log(strings);   // [ '', '+', '=', '' ]
    console.log(params);   // [ 6, 9, 15 ]
}

let taggedResult = simpleTag`${6}+${9}=${6 + 9}`;
```

```js
let a = 6;
let b = 9;
function simpleTag(strings, ...params) {
    return res = strings[0] + params.map((el, i) =>
        `${el} ${strings[i + 1]} `
    ).join('');
}

let taggedResult = simpleTag`${6}+${9}=${6 + 9}`;

console.log(taggedResult);	// 6+9=15
```





## 原始字符串

通过**标签函数** `String.raw` 可以获取原始字符串的内容，而不是转换后的内容

```js
// 转化后的
console.log(`\u00A9`);  // ©

// 为转化的
console.log(String.raw`\u00A9`);    // \u00A9
```

