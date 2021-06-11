## 什么是 BFC

BFC(Block formatting context)直译为 **"块级格式化上下文"**。

它是一个独立的渲染区域，只有Block-level box参与， 它规定了内部的Block-level Box如何布局，并且与这个区域外部毫不相干。



## Box

Box 是 CSS 布局的对象和基本单位.。

+ 块元素：

  display ：block 、list-item 、table 会让一个元素成为块级元素。

  + 宽高有效
  + 自动换行
  + margin  有效

+ 行内元素：

  display ：inline、inline-block、inline-table、table-cell 会让一个元素成为行内元素。

  + 宽高无效
  + 不会自动换行
  + margin 上下无效

+ 行内块状元素

  + 宽高有效
  + 自动换行
  + margin 有效



## Formatting Context

页面中的一块渲染区域，并且有一套渲染规则，它决定了其子元素将如何定位，以及和其他元素的关系和相互作用。

+ BFC：Block fomatting context
+ IFC：Inline formatting context



## BFC 布局规则

- BFC 页面上一个独立容器，容器里面和外面的元素互不影响
- Box 垂直方向的距离由 margin 决定

**解决margin重叠**

+ 同一个 BFC 相邻的两 Box 的 margin 会发生重叠

**左右两边自适应宽度**

- BFC 的区域不会与 float Box 重叠

**清除浮动**

- 计算 BFC 的高度时，浮动元素参与计算

+ 当前盒子的 margin Box 的左边和 父元素的 border Box 的左边接触（或者右边和右边接触），即使存在浮动也是如此。

  



## 如何创建 BFC

+ 存在浮动

+ 非相对定位（relative）的定位

+ display的值是inline-block、table-cell、flex、table-caption

  或者 inline-flex

+ overflow的值不是默认属性visible



## BFC 的使用

### 避免margin重叠

```html
<p>看看我的 margin是多少</p>
<p>看看我的 margin是多少</p>
```

```css
p {
    color: #f55;
    background: yellow;
    width: 200px;
    line-height: 100px;
    text-align: center;
    margin: 30px;
}
```

因为在同一个 BFC 所以 margin 重叠

![1623309553198](C:\Users\Amd\AppData\Roaming\Typora\typora-user-images\1623309553198.png) 



**新建一个BFC**

```css
.p1 {
    /* float: left; */
    /* position: absolute; */
    display: inline-block;
}
```

解决 margin 重叠

![1623309585860](C:\Users\Amd\AppData\Roaming\Typora\typora-user-images\1623309585860.png)



### 自适应两栏布局

实现：左边固定， 右边自适应

```html
<div class="left">LEFT</div>
<div class="right">RIGHT</div>
```

```css
div {
    text-align: center;
    font-size: 20px;
}

.left {
    width: 100px;
    height: 150px;
    float: left;
    background: rgb(139, 214, 78);
    line-height: 150px;
}

.right {
    height: 300px;
    background: rgb(170, 54, 236);
    line-height: 300px;
}
```

![1623309361976](C:\Users\Amd\AppData\Roaming\Typora\typora-user-images\1623309361976.png)



**通过 overflow 形成一个BFC**

```css
.right {
    overflow: hidden;
    height: 300px;
    background: rgb(170, 54, 236);
    line-height: 300px;
}
```

![1623309434981](C:\Users\Amd\AppData\Roaming\Typora\typora-user-images\1623309434981.png)





### 清除浮动

**BFC计算高度时，浮动元素也会计算，通过 `overflow: hidden;` 解决高度塌陷**

```html
<div class="par">
    <div class="child"></div>
    <div class="child"></div>
</div>
```

```js
.par {
    border: 5px solid rgb(91, 243, 30);
    width: 300px;
}

.child {
    border: 5px solid rgb(233, 250, 84);
    width: 100px;
    height: 100px;
    float: left;
}
```

![1623310721149](C:\Users\Amd\AppData\Roaming\Typora\typora-user-images\1623310721149.png)



**通过 overflow：hidden 给外壳 .par 新建一个 BFC  **

```css
.par {
    border: 5px solid rgb(91, 243, 30);
    width: 300px;
    overflow: hidden;
}
```

![1623310793467](C:\Users\Amd\AppData\Roaming\Typora\typora-user-images\1623310793467.png)