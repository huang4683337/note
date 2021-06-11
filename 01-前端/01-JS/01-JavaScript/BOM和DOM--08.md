

## Window 对象（窗口对象）

```text
包括：window对象的属性（window对象的内置对象）

screen（屏幕对象）
navigator（浏览器对象）-- BOM
localtion（地址对象）
history（历史对象）
document（文档对象）
event（事件对象）
```



## screen 对象

```test
获取屏幕的宽高
screen.with / availWidth   // 后者获取的高度不包括电脑工具栏的高度
screen.height / availHeight 
```



## scroll 对象

```text
scrollLeft  获取滚动条 水平 方向滚走的距离

scrollTop   获取滚动条 垂直 方向滚走的距离

兼容：document.body.scrollTop = document.documentElement.scrollTop = 值
```



## DOM - document 对象

### 获取元素

```js
getElementById();    					
// 通过 id 查找元素，结果是唯一的

getElementsByTagName()[0];		
// 通过 标签 查找元素，结果是一个集合 

getElementByName();						
// 通过 name 查找元素，用于表单

getElementsByClassName()[0];	
// 通过 class 查找元素，结果是一个集合。 ie6不能用

class = 'class'
querySelector('class');				
// 通过 选择器 查找元素，结果唯一

querySelectorAll('class');		
// 通过 选择器 查找元素，结果是一个集合
```



### 修改元素属性

```js
// 元素就是标签 比如：<div>

div.style.width  							
// 获取当前元素的宽

div.innerHTML									
// 得到标签内部所有的内容

div.outerHTML 								
// 得到元素内部以及该元素本身所有的内容

div.innerText || div.textContent 
// 得到纯文本数据
```



**测试**

```html
<div id="outerHtml">
  <div id="innerHtml">
    <span>innerText</span>
	</div>
</div>
```

```js
var dom = document.querySelector('#innerHtml');

console.log(dom.outerHTML);
// <div id="innerHtml"> <span>innerText</span> </div>

console.log(dom.innerHTML);
//  <span>innerText</span> 和换行

console.log(dom.innerText);
// 文本内容：innerText

console.log(dom.textContent);
// 文本内容：innerText 还有换行
```





### 节点种类

```text
html 是 dom 的根节点， 页面上的一切都是节点。

元素节点： 所有的 html 元素
属性节点： 所有的 html 元素的属性
文本节点： 所有的 html 元素的内容
```



### 节点关系

```text
 - 父节点：parentNode (加样式会影响到子节点，比如给ul添加颜色 li也会改变)

- 下一个兄弟节点：nextElementSibling(高版本浏览器)  nextSibling(低版本浏览器 IE678)

- 前一个兄弟节点：previousElementSibling
- 第一个孩子节点：firstElementChild
- 最后一个孩子节点：lastElementChild
- 所有的孩子节点：childNodes children
	- children： 得到所有的子节点
	- childNodes：得到元素节点和文本节点的集合 (回车会被当作文本节点)

使用方式： div.parentNode
```



### 确定节点类型

```text
通过 nodeType 的值确定节点类型：
	值为1 是元素节点
	值为2 是属性节点
	值为3 是文本节点
```



### 节点的动态操作

```
1、动态创建节点
document.createElement('div');			// 创建一个元素节点 div
document.createDocumentFragment()    //创建一个DOM片段
document.createTextNode()   //创建一个文本节点

2、添加元素节点
父元素.appendChild('子元素节点');			
// 将动态创建的 子元素 添加到 父元素的 最后

3、插入节点
父元素.insertBefore('要添加的子元素'，'参照节点');
// 掺入到参照节点前面

4、替换节点
父元素.replaceChild();

5、删除节点
父元素.removeChild('子元素节点');

6、节点克隆
需要克隆的节点.cloneNode(true);
true(深克隆)：既克隆对象本身，又克隆对象内容。
false(浅克隆)：只克隆对象本身
```



### 节点属性的动态操作

```js
获取属性：div.getAttribute('属性名 比如name')		
// 不能操作属性值为布尔的值  比如：checked

设置属性：div.setAttribute('属性名','属性值')

删除属性：div.removeAttribute('属性名')

div.attributes：获取元素身上所有属性构成的集合
```



## 什么是DOM回流（reflow）

　　页面渲染时，我们对HTML结构简单的增删查改时，浏览器会对所有的dom进行重新排序，这就i是DOM回流，严重影响浏览器性能



## 什么是DOM重绘

当render tree中的一些元素需要更新属性，而这些属性只是影响元素的外观，风格，而不会影响布局的，比如background-color。则就叫称为重绘。