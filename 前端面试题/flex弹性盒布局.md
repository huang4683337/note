# Flex 弹性盒布局

## 指定为 Flex 布局

```css
.box{
  display: flex; /* 指定为 Flex 布局 */
  display: inline-flex; /* 行内元素指定为 Flex 布局 */
  display: -webkit-flex; /* Safari；Webkit 内核的浏览器兼容写法*/
}
```

## 容器的六个属性

```css
/* 默认主轴为横向的x轴 默认交叉轴为纵向的y轴 */
flex-direction  /* 决定主轴是什么方向 */
flex-wrap	/* 是否换行 */
flex-flow /* flex-direction 和 flex-wrap 的简写 */
justify-content /* 在主轴上的对其方式 */
align-items /* 在交叉轴上的对其方式 */
align-content /* 定义存在多根轴线时的对其方式（也就是子元素存在换行） */
```

### flex-direction

```js
row（默认值）：主轴为水平方向，起点在左端。
row-reverse：主轴为水平方向，起点在右端。
column：主轴为垂直方向，起点在上沿。
column-reverse：主轴为垂直方向，起点在下沿。
```

### flex-wrap

```js
nowrap(默认值): 不换行。
wrap: 换行，从上到下依次排或者从左到右。
warp-reverse: 换行，从下到上依次排或者从右到左。
```

### flex-flow

```js
.box {
  flex-flow: <flex-direction> || <flex-wrap>;
}
```

### justify-content

```css
flex-start : 从主轴的起点开始
flex-end : 从主轴的终点开始
center : 在主轴居中排列
space-between : 两边紧挨着主轴的头尾 中间等份排列
space-around : 等份排列
```

### align-items

```css
flex-start：从交叉轴的起点开始。
flex-end：从交叉轴的终点开始。
center：在交叉轴剧中排列。
baseline: 从子元素第一行的文字底部对其。
stretch（默认值）：如果子元素没有设置交叉轴方向上占据的空间（宽或者高），子元素会在交叉轴铺满。
```

### align-content

```css
flex-start : 与交叉轴的起点对齐。
flex-end : 与交叉轴的终点对齐。
center : 与交叉轴的中点对齐。
space-between : 与交叉轴两端对齐，轴线之间的间隔平均分布。
space-around : 每根轴线两侧的间隔都相等。所以，轴线之间的间隔比轴线与边框的间隔大一倍。
stretch（默认值）: 轴线占满整个交叉轴。
```



## 子元素的六个属性

```css
order : 以数值定义子元素排列顺序，数值越小越靠前。默认为0
flex-grow : 定义子元素的放大比例，若有剩余空间就会按比例放大。默认为0，不放大。
flex-shrink : 定义子元素缩小比例，若剩余空间为负就会按比例缩小。默认为1，不缩小。
flex-basis : 定义子元素在主轴所占空间。浏览器在计算剩余空间时会减去所定义的再去分配。
flex : flex-grow、flex-shrink、flex-basis的缩写。默认为 0 1 auto，后两个属性可选。
align-self : 
	允许子元素设置与其他元素不一样的对其方式，会覆盖align-items的样式。
	默认为auto表示继承align-items的样式。
```

### order

### flex-grow

### flex-shrink

### flex-basis

### flex

### align-self

```css
auto : 继承 align-items 的样式
flex-start : 从交叉轴的起点开始。
flex-end : 从交叉轴的终点开始。
center : 在交叉轴剧中排列。
baseline: 从子元素第一行的文字底部对其。
stretch（默认值）: 如果项目未设置高度或设为auto，将占满整个容器的高度。
```



