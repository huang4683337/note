## 利用事件委托获取 li 索引

```html
<ul class="ul">
    <li class="ele" draggable="true">1</li>
    <li class="ele" draggable="true">2</li>
    <li class="ele" draggable="true">3</li>
    <li class="ele" draggable="true">4</li>
</ul>
```

```js
/*
Array.prototype.slice.call 将伪数组lis转化为数组
利用数组的 indexOf 得到对应的索引
*/
/**
1- 获取到所有的li伪数组，伪数组转化为数组
2- 给 ul 绑定监听事件，点击某个li时，会得到点击的目标对象，使用	  indexOf 在li数组中进行匹配，就可以得到索引  
*/
var ul = document.ev(".ul");
var lis = document.querySelectorAll("li");
var li = Array.prototype.slice.call(lis);
ul.addEventListener('click',callback,false);
function callback(e){
    var event = event || window.event;
    var target = event.target || event.srcElement;
    console.log(li.indexOf(target))
}
```

```js
/* 
利用闭包实现
*/
var lis = document.getElementsByTagName('li');
for (let i = 0; i < lis.length; i++) {
    lis[i].addEventListener("click", function (e) {
        console.log(i)
    }, false);
}
```

