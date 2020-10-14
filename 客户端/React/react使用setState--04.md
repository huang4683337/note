## setState

语法： `setState(partialState, callback)`

+ `partialState` : object | function
  + object  直接写需要合并的子集
  + function 需要 return 一个需要和 state 合并的子集

+  `callback` : function

  state 更新之后调用





## 不要直接修改 State

例如以下代码不会重新渲染组件：

```react
// 在一个定时器中
this.state.date = new Date(); // 页面不会发生改变
```

```react
// 在一个定时器中 正确写法
this.setState({ date: new Date() }); // 页面 发生改变
```



## State 的更新可能是异步的

```react
import React, { Component } from "react";

export default class SetStatePage extends Component {
  state = { cont: 0 };

  render() {
    const { cont } = this.state;
    return (
      <div>
        <p>SetStatePage</p>
        {cont}
      </div>
    );
  }
}
```



+ `setState` 在 `合成事件`、`生命周期` 中是异步的（对操作的数据进行批量更新），这些优化性能的操作

  ```react
  // 合成事件
  changeValue = (v) => {
    this.setState({ cont: this.state.cont + v });
    console.log("cont", this.state.cont);
  };
  
  setCont = () => {
    this.changeValue(1);
  };
  
  <button onClick={this.setCont}>按钮</button>
  
  // 点击按钮 ：log 结果为 0，页面显示为 1
  ```
  
  ```react
  
  // 生命周期
  componentDidMount() {
    this.changeValue(1);
  }
  
  changeValue = (v) => {
    this.setState({ cont: this.state.cont + v });
    console.log("cont", this.state.cont);
  };
  
  // log 结果为 0，页面显示为 1
  ```
  
  
  
+ `setState`  在 `setTimeout` 、`原生事件`、`setState 的 callback` 中是同步的 （对操作的数据进行实时更新）

  ```react
  // setTimeout
  
  setCont = () => {
    setTimeout(() => {
      this.changeValue(1);
    }, 0);
  };
  
  
  changeValue = (v) => {
    this.setState({ cont: this.state.cont + v });
    console.log("cont", this.state.cont);
  };
  
  <button onClick={this.setCont}>按钮</button>
  
  // log 结果为 1，页面显示为 1
  ```

  ```react
  // 原生事件
  
  componentDidMount() {
    document.getElementById('test').addEventListener("click", this.setCont, false);
  }
  
  setCont = () => {
    this.changeValue(1);
  };
  
  <button id='test'>按钮</button>
  
  // log 结果为 1，页面显示为 1
  ```

  ```react
  // 在 setState 回调中取值
  changeValue = (v) => {
    this.setState({ cont: this.state.cont + v }, () => {
      console.log("cont", this.state.cont);
    });
  };
  
  setCont = () => {
    this.changeValue(1);
  };
  
  <button onClick={this.setCont}>按钮</button>
  
  // log 结果为 1，页面显示为 1
  ```

  

## State 的更新会被合并

因为 state 的更新是批量更新的，那么同时对一个数据进行多次处理就只会进行最后一次操作

```react
changeValue = (v) => {
  this.setState({ cont: this.state.cont + v });
};

setCont = () => {
  this.changeValue(1);
  this.changeValue(2);
};

<button onClick={this.setCont}>按钮</button>

// log 结果为 2， 只进行了 +2， 没有进行 +1 
```



如何实现链式更新 state

```react
 this.setState((state) => ({ cont: state.cont + v }));

// log 结果为 3 ， 先进行 +1  再进行 +2
```

