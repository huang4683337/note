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