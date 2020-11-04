[JSX](https://zh-hans.reactjs.org/docs/introducing-jsx.html)

## JSX

JSX是⼀种JavaScript的语法扩展，其格式⽐较像模版语⾔，但事实上完全
是在JavaScript内部实现的。

JSX可以很好地描述UI，能够有效提⾼开发效率，体验 [JSX](https://zh-hans.reactjs.org/)。





## 准备

注释掉 index.js 中所有代码，添加以下代码

```react
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';

ReactDOM.render(jsx, document.getElementById('root'))
```







##  基本使用

表达式用 {} 包裹

```js
const name = "react study";
const jsx = <div>hello, {name}</div>;
```



## 函数

```react
const obi = {
  first:'1',
  last:"2"
}

const jsx = (
	<div>{obj}</div>
)

{/* 直接访问 obj 报错，只能通过函数访问 */}
```



函数也是合法表达式，index.js

```react
const user = {
 fistName: "Harry",
 lastName: "Potter"
};
function formatName(name) {
 return name.fistName + " " + name.lastName;
}
const jsx = <div>{formatName(user)}</div>;
```



## JSX 对象

```react
// 定义一个 jsx 对象
const greet = <div>good</div>;

// 直接将 jsx 放入{}表达式中
const jsx = <div>{greet}</div>;
```



## 条件语句

条件语句可以基于上⾯结论实现，index.js

```react
const show = true;//false;
const greet = <div>good</div>;
const jsx = (
 <div>
 {/* 条件语句 */}
 {show ? greet : "登录"}
 {show && greet}
 </div>
);
```



## 数组

通过 map 处理

数组会被作为⼀组⼦元素对待，数组中存放⼀组jsx可⽤于显示列表数据

```react
//  diff时候，⾸先⽐较type，然后是key，所以同级同类型元素，key值必须得 唯⼀ 

const a = [0, 1, 2];
const jsx = (
  <div>
    <ul>
      {a.map((item) => (
        <li key={item}>{item}</li>
      ))}
    </ul>
  </div>
);
```



## 属性

```js
// 属性：静态值⽤双引号，动态值⽤花括号；class、for等要特殊处理。

import logo from "./logo.svg";
const jsx = (
  <div>
    <img src={logo} style={{ width: 100 }} />
  </div>
);
```



## 模块化

css模块化，创建index.module.css，index.js

```react
import style from "./index.module.css";
<img className={style.logo} />
```



或者npm install sass -D

```react
import style from "./index.module.scss";
<img className={style.logo} />
```

更多css modules规则[参考](http://www.ruanyifeng.com/blog/2016/06/css_modules.html)

