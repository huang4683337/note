## 组件复合

意思就是组件的复用。tong

[组合 vs 继承](https://zh-hans.reactjs.org/docs/composition-vs-inheritance.html)



## APP.js

```react
import React from "react";

function App() {
  return (
    <div className="App">
      
    </div>
  );
}
```





## 头部组件

**TopBar.js**

```react
import React, { Component } from 'react'

export default class TopBar extends Component {
    render() {
        return (
            <div>
                TopBar
            </div>
        )
    }
}

```



## 底部组件

**BottomBar.js**

```react
import React, { Component } from 'react'

export default class BottomBar extends Component {
    render() {
        return (
            <div>
                BottomBar
            </div>
        )
    }
}

```



## 展示页面

**Layout.js**

```react
import React, { Component } from "react";
import TopBar from "./TopBar";
import BottomBar from "./BottomBar";

export default class Layout extends Component {
  
  render(){
    return(
      <div>
        <TopBar/>
        isLayout
        <BottomBar/>
      </div>
    )
  } 
}
```



```react
// App.js 中添加
import Layout from "./pages/componentMerge/Layout";

<div className="App">
  <Layout/>
</div>
```



## 使用插槽

### 匿名插槽

**HomePage.js**

```react
import React, { Component } from "react";
import Layout from "./Layout";

export default class HomePage extends Component {
  render() {
    return (
      // 相当于在 Layout 组件中插入 <div>HomePage</div>
      <Layout showTopBar={false} showBottomBar={true} title="HomePage">
        <div>HomePage</div>
      </Layout>
    );
  }
}

```



**Layout.js**

```react
import React, { Component } from "react";
import TopBar from "./TopBar";
import BottomBar from "./BottomBar";

export default class Layout extends Component {
  render() {
    // HomePage.js 中给 Layout 传的值
    // children 就是想要插入的内容
    const { children, showTopBar, showBottomBar } = this.props;
    return (
      <div>
        {showTopBar && <TopBar />}

        {children}

        {showBottomBar && <BottomBar />}
      </div>
    );
  }
}
```



### 具名插槽

**SlotName.js**

```react
import React, { Component } from "react";
import Layout from "./Layout";
// 类似于 vue 具名插槽
export default class SlotName extends Component {
  render() {
    return (
      <Layout showTopBar={true} showBottomBar={true} title="SlotName">
         {/* 第一个 {} 是 jsx 语法，放入变量; 第二个 {} 是一个对象，存入的是需要插入的信息  */}
        {{
          content: <div>SlotName</div>,
          txt: "这是一个文本",
          btnClick: () => {
            console.log("btnClick");
          },
        }}
      </Layout>
    );
  }
}

```



**Layout.js**

```react
import React, { Component } from "react";
import TopBar from "./TopBar";
import BottomBar from "./BottomBar";

export default class Layout extends Component {
  componentDidMount() {
    const { title = "title" } = this.props;
    document.title = title;
  }
  render() {
    const { children, showTopBar, showBottomBar } = this.props;
    return (
      <div>
        {showTopBar && <TopBar />}
        {children.content}
        {children.txt}
        <button onClick={children.btnClick}>button</button>
        {showBottomBar && <BottomBar />}
      </div>
    );
  }
}

```

