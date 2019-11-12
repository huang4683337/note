# Event

## ie 和 dom 事件流的区别

```shell
IE(低版本)采用 冒泡型 事件

Netscape 采用 捕获型事件

DOM 采用 先捕获后冒泡型事件  类似于字母 V 这种情况
```



## Event 对象

```js
事件对象：触发点击事件（onclick）、键盘事件（onKeyup）等这些事件时会产生一个事件对象 Event

不同的浏览器产生事件对象所对应的属性不同，所以需要兼容
谷歌浏览器：window.event  事件参数为evt
火狐或者 IE：window.event

兼容：
evt || window.event  先兼容高版本浏览器 evt为事件参数

function fn(evt){
  var e = evt || event; // 事件兼容
  if(evt){
     return e.事件  // 高版本浏览器
  }else if(window.event){
    return e.事件   // 低版本浏览器
  }
  
}
```



## 鼠标事件的对象参数

```js
clientX / clientY ： 距离 window 窗口（网页可视区）的坐标
pageX / pageY ：距离文档窗口的坐标（不算滚动条）
offsetX / offsetY ：相对于父级的坐标
screenX / screenY ：距离显示器屏幕的坐标
```



## 阻止浏览器默认事件兼容

```js
e.preventDefault ? e.preventDefault() : e.returnValue = false;
```



## 事件流

```js
事件执行的顺序问题，当事件发生时，从子元素向父元素触发 或者 从父元素向子元素触发的过程，就交租事件流。

事件流的两种模式：
	事件冒泡：从子元素向父元素触发的过程
  事件捕获：从父元素向子元素触发的过程
  
当一个元素既有冒泡又有捕获时： 先捕获后冒泡
```



## 事件捕获（true）和 事件监听

```js
addEventListener(事件,事件处理程序,true);  // true 代表事件捕获

// 通过 事件监听 的方法解决事件捕获
可以为 相同的元素绑定多个相同事件并且都会从上到下执行。(正常情况下 同样的事件只会执行最后一个，但事件监听不是)

// 事件监听兼容
高版本浏览器： addEventListener(事件不加on,事件处理程序,true);
低版本浏览器： attachEvent(事件必须加on,事件处理程序)
```



## 事件冒泡（false）和 事件委托

```js
当一个事件发生时，同样的事件会在父级元素触发，这个过程就是事件冒泡。

并不时所有的事件都会有冒泡： onload onfocus onblur 就不会

阻止事件冒泡兼容：
	e.stopPropagation ? e.stopPropagation() : e.cancelBubble = true;
					传播																	取消	泡沫

// 事件委托 => 利用了事件冒泡的原理
多个相同的元素都需要执行某个事件，将事件添加在元素的父级上，委托父级元素来执行这个事件。          
          
// 事件委托获取目标元素兼容
e.target || e.srcElement;
```



## 拖拽

```js
1、 onmousedowm (按下鼠标左键)
2、 onmousemove (鼠标移动)
3、 document.onmousemove = null (取消鼠标移动事件)

// 获取窗口的宽 高
window.innerWidth / window.innerHeight (浏览器窗口内部的宽高)
window.outerWidth / window.outerHeight (包括浏览器工具栏的宽高)


//取消拖拽时的文字选中状态
window.getSelection ? window.getSelection().removeAllRanges() : document.selection.empty();
														高版本浏览器																	低版本浏览器
```



## offset家族属性

```js
offsetWidth / offsetHeight ： 内容+padding+border
clientWisth / clientHeight ： 内容+padding
offsetLeft ： 默认相当于body的偏移。 如果父级又定位，则相对于父级偏移。
offsetTop ： 同 offsetLeft

offset 值为 Number 不需要加 px // 元素.offsetWidth = 100; 能取任意样式; 可读不可写
style.width 值为 String 需要加 px //元素.style.width = 100+'px'; 只能取行内元素样式; 可读写
```



