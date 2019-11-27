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
var ul = document.querySelector(".ul");
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

