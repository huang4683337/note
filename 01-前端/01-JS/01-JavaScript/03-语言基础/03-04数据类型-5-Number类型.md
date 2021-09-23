## 浮点值

```js
// 浮点值的定义
let floatNum1 = 1.1;
let floatNum2 = 1.0;
let floatNum3 = .1;	// 有效但不推荐
```

```js
// 小数点后无值

// 因为存储浮点值使用的内存空间是存储整数值的两倍，所以 ECMAScript 总是想方设法把值转换为整数
let floatNum1 = 1.;	// 1
let floatNum2 = 10.0;	// 10
```

```js
// e 表示法

// 对于非常大或非常小的数值，浮点值可以用科学记数法来表示
// 10 的 n 次幂 表示为 e的n
let floatNum = 3.125e7; // 等于31250000
```

```js
// 浮点值精度问题
// 浮点值精度最高到 17 位，但还是会有误差

console.log(0.1 + 0.2 == 0.3); // false

console.log(0.1 + 0.2); // let floatNum = 3.125e7; // 等于31250000
```



## 值的范围

```js
// js 可以表示的最小值保存在 Number.MIN_VALUE 中
// js 可以表示的最大值保存在 Number.MAX_VALUE 中

// 超出 js 表示范围: 正无穷大 Infinity
// 超出 js 表示范围: 负无穷大 -Infinity
```



**isFinite() 判断一个值是不是介于 js 可以表示的最大值和最小值之间**

```js
console.log(isFinite(1));	// true
console.log(isFinite(Infinity));	// false
```



> 使用 Number.NEGATIVE_INFINITY 和 Number.POSITIVE_INFINITY 也可以获取正、负 Infinity。没错，这两个属性包含的值分别就是 -Infinity 和 Infinity。



## NaN

不是数值的数字类型。

+ 任何涉及到 NaN 的操作，返回结果都是 NaN

+ NaN 不等于任何值（包括 NaN 自身）

  ```js
  console.log(NaN == NaN); // false
  ```

+ isNaN() 判断一个数据类型是不是 NaN，会尝试把这个数据类型转化为数值，转化失败就是 NaN，返回 true

  ```js
  console.log(isNaN(NaN)); // true 是NaN
  console.log(isNaN('10')); // false 转化成 10
  console.log(isNaN(true));	// false 转化成 1
  console.log(isNaN([]));	// false 转化成 0
  console.log(isNaN('blue'));	// true 转化失败
  console.log(isNaN({}));	// true 转化失败
  ```

  



## Number() 

将**任何类型**强制转换成  **十进制**  数字类型

| 数据类型  | 转化方式                                                     | 转化结果            |
| --------- | ------------------------------------------------------------ | ------------------- |
| Undefined | Number( undefined )                                          | NaN                 |
| Null      | Number( null )                                               | 0                   |
| Boolean   | Number( true \| fales )                                      | 1\|0                |
| Number    | Number( 1 )                                                  | 直接返回            |
| String    | Number( ' ' \| '12' \| '12s' \| '0xf' )                      | 0 \| 12 \| NaN \|15 |
| Object    | 先试用  *valueOf()*  按照前面 5 类数据处理。如果返回 *NaN* 则再次调用 *toString()* 按照字符串规则转换 `变量.valueOf()|变量.valueOf().toString()` |                     |



## parseInt(xx, 进制)

字符串转数值，默认按照 **十进制** 转数值。通过设置**第二个参数** 规定第一个参数是 **什么进制** ，然后解析成 **十进制数**。

+ 从第一个非空字符开始解析

+ 遇到非数值字符停止解析

+ 第一个字符不是数值字符，直接返回 NaN

  `+数字`、`-数字` 的情况除外

```js
// 从第一个非空字符开始解析
console.log(parseInt('     1'));	// 1
```

```js
// 遇到非数值字符停止解析
console.log(parseInt('12+s'));  // 12

// 利用遇到非数值字符停止解析的特性，可以对数字进行取整操作
console.log(parseInt(12.11111)); // 12
```

```js
// 第一个字符不是数值字符，直接返回 NaN
console.log(parseInt('+')); // NaN
console.log(parseInt(''));	// Nan

// 第一位为 +/- 随后紧跟数值字符
console.log(parseInt('+1')); // 1
```



**规定通过什么进制去解析**

```js
let num1 = parseInt("10", 2); // 2，按二进制解析 
let num2 = parseInt("10", 8); // 8，按八进制解析 
let num3 = parseInt("10", 10); // 10，按十进制解析 
let num4 = parseInt("10", 16); // 16，按十六进制解析
```



## parseFloat(xx)

**只能**将字符串按照**十进制**转化。

+ 从第一个非空字符开始解析
+ 遇到除了 第一个点 以外的 非数值 字符停止解析

+ 第一个字符不是数值字符，直接返回 NaN

  `+数字`、`-数字` 的情况除外

```js
// 从第一个非空字符开始解析
console.log(parseFloat('    11.1')); // 11.1
```

```js
// 遇到除了 第一个点 以外的 非数值 字符停止解析
console.log(parseFloat('11.1.2.3.4')); // 11.1
console.log(parseFloat('11ss1.1.1')); // 11
```

```js
console.log(parseFloat('3.125e7'));	// 31250000
```

