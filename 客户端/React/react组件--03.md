## 组件

组件，从概念上类似于 JavaScript 函数。它接受任意的⼊参（即 “props”），并返回⽤于描述⻚⾯展示
内容的 React 元素。



**组件有两种形式：class 组件和 function 组件。**

[组件 & Props](https://zh-hans.reactjs.org/docs/components-and-props.html)

## 注意

+ 在 react 中修改 state 中的数据，必须使用 setState。`this.setState({xx:xxx})`





## class组件

class组件通常拥有 `状态 state` 和 `⽣命周期 ，继承于Component，实现 render ⽅法`。⽤ class 组件创建⼀个 Clock

```react
import React, { Component } from "react";

export default class ClassComponent extends Component {
  constructor(props) {
    super(props);
    this.state = {
      date: new Date(),
    };
  }

  componentDidMount() {
    this.timerID = setInterval(() => {
      // 使⽤setState⽅法更新状态
      this.setState({
        date: new Date(),
      });
    }, 1000);
  }

  componentWillUnmount() {
    // 组件卸载前停⽌定时器
    clearInterval(this.timerID);
  }
  render() {
    return <div>{this.state.date.toLocaleTimeString()}</div>;
  }
}

```





## Function 组件

借用 `Hook` 来实现

+ 通过 `useState`  来实现 `state`
+ `useEffect` 

```react
import React, { useState, useEffect } from "react";

export default function FunctionComponent(props) {
  const [date, setDate] = useState(new Date());
  useEffect(() => {
    const timer = setInterval(() => {
      setDate(new Date());
    }, 1000);

    return () => clearInterval(timer);
  },[]); // [] 代表 useEffect 不依赖任何项，只会执行一次

  return (
    <div>
      <p>{date.toLocaleTimeString()}</p>
    </div>
  );
}
```



>提示: 如果你熟悉 React class 的⽣命周期函数，你可以把 useEffect Hook 看做
componentDidMount ， componentDidUpdate 和 componentWillUnmount 这三个函数的组
合。

