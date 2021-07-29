## typeof 操作符

| 类型        | typeof  操作                                   | typeof 输出             |
| ----------- | ---------------------------------------------- | ----------------------- |
| `Undefined` | `typeof undefined`                             | `"undefined"`           |
| `Null`      | `typeof null`                                  | `"object"`              |
| `Boolean`   | `typeof true|false`                            | `"boolean"`             |
| `Number`    | `typeof 1`                                     | `"number"`              |
| `String`    | `typeof '12312'`                               | `string`                |
| `Symbol`    | `typeof Symbol()`                              | `symbol`                |
| `Object`    | `typeof []|{} `  \|\|  `typeof function a(){}` | `object`  || `function` |

> typeof 在判断未声明和未定义时都是 undefined ，所以建议在定义变量时对变量进行初始化，这样 typeof 返回 undefined 时就知道是变量未定义了

```js
// 变量未来初始化
var a;
console.log(typeof a);  // undefined

// 变量未定义
console.log(typeof xx); // undefined
```

