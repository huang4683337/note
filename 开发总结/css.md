# css面试题

## css3的选择器有哪几种

[掘金选择器优先级](https://juejin.im/post/5be3d07be51d457d4932b043)

```js
// 基本选择器
通用选择器，匹配任何元素		// *
标签选择器				// p div...
class选择器			// .class
id选择器		// #id

伪类选择器  
// 不会改变DOM内容，只是插入一些修饰类的元素。:link :visited :hover :active

伪元素选择器	
// 需要添加一个实际的元素才可以 ::before ::after ::first-letter(第一个字) ::first-line(针对第一行)

属性选择器  // input[value='1']

// 优先级
!important > 内联 > ID选择器 > 类选择器 > 标签选择器
```



## 样式表的优先级

```js
内联最大， 内部和外部样式不分大小，谁在后边谁生效。
```



## 盒模型和怪异和模型(IE)

```css
盒模型组成 : 内容 + border + padding + margin

怪异盒模型 : border、padding 会计算在内容的宽高中 

利用 box-sizing: border-box 解决盒模型差距
W3C中 box-sizing: content-box
IE中 box-sizing: border-box


*, *:before, *:after {
  -moz-box-sizing: border-box;
  -webkit-box-sizing: border-box;
  box-sizing: border-box;
}
```

## margin-top 不生效的解决方法

```js
给父元素添加 边框
给父元素添加 overflow:hidden;
给父元素添加 pading
给父元素添加 float
给自身添加 float
```



## 清除浮动

```css
给父元素添加 overflow: hidden
给父元素添加 overflow: auto
给父元素添加 高

使用clear: both
.clearfix:after {
    content: '.';
    height: 0;
    display: block;
  	overflow:hidden;
  	visibility: hidden;
    clear: both;
}
```



## 定位

```css
static /* 默认的 */
relative /* 相对定位：相对于自身产生位移 */
absolute /* 绝对定位：相对于离自己最近的已定位的祖先元素 */
fixed /* 固定在某个区域 */

/* 定位后可通过 z-index 来调整层叠顺序 */
```



## 元素类型

```js
// 块元素
div,dl,dt,dd,ol,ul,(h1-h6),p,form,hr,table,tr,td
默认情况下块元素独占一行，自上而下排列。
块元素都可以定义自己的宽高

// 内联元素（行内元素）
a,span,i,em,strong,b，img，input
在行内逐个显示，子左向右。
没有自己的宽高，给padding会变大 且互相遮住

// 可变元素
通过 display 来设置，一共有18个属性。
display: block; // 块元素：独占一行、可设宽高
display: inline; // 行内元素：逐个显示、没有宽高
display: inline-block; // 行内块元素：逐个显示、可设宽高
display: list-item; // 列表元素
```

### 不定款导航居中

```css
Ul{text-align:center}

Li{display:inline-block}
```

### 单行居中多行靠左

```css
.all{text-align:center;}

.all div{display:inline-block;text-align:left;}
```

### 图片垂直居中

```css
Img{vertical-align:middle;}

Span{hieght:100%;width:0; vertical-align:middle;display:inline-block;}
```



## display 和 visibility

```css
display: hidden; /* 后代元素跟着隐藏；不占页面空间；引起DOM重新渲染和回流 */
visibility: hidden; /* 隐藏自身；继续占页面空间； 不会引起DOM渲染和回流 */
```



## 浏览器内核

```js
Trident: IE	   => 
Gecko: Firefox 	  => -moz-
Webkit: safari chrome		  => -webkit-
Presto: opera		  =>  -o-
Blink: Google		  => 
```



## css Bug、css Hack、Filter

```js
css Bug: css 在不同浏览器中的解析不一致
css Hack: 兼容 css 在不通浏览器显示的方法
Filter: 对特定的浏览器使用特定的方式解决兼容问题
## css 实现可配置换行

```html
<div class="div1">
    <span>131313113</span>
    <span>131313113</span>
    <span>131313113</span>
</div>

<!-- 通过p标签实现可配置换行 -->
<div class="div2">
    <span>131313113</span>
    <p></p>
    <span>131313113</span>
    <p></p>
    <span>131313113</span>
</div>
```

```css
* {
    margin: 0;
    padding: 0;
}

.div1 {
    background-color: green;
}

.div2 {
    margin-top: 40px;
    background-color: yellow;
}

span {
    margin: 10px 0;
    background-color: red;
}

p{
    height: 10px;
}
```

