#animation动画

## 什么是 animation

> animation则是通过关键帧@keyframes来实现更为复杂的动画效果



## 属性

> [注意]IE9-不支持；safari4-8、IOS3.2-8.4、android2.1-4.4.4需要添加-webkit-前缀

|           属性            |               介绍               |                 例子                 |
| :-----------------------: | :------------------------------: | :----------------------------------: |
|      animation-name       | 动画名称(默认值为none)，可自定义 |        animation-name: test;         |
|    animation-duration     | 持续时间(默认值为0)，不能为负数  |       animation-duration: 3s;        |
| animation-timing-function |      时间函数(默认值为ease)      |   animation-timing-function: ease;   |
|      animation-delay      |       延迟时间(默认值为0)        |         animation-delay: 0s;         |
| animation-iteration-count |       循环次数(默认值为1)        | animation-iteration-count: infinite; |
|    animation-direction    |     动画方向(默认值为normal)     |     animation-direction: normal;     |
|   animation-play-state    | 播放状态(默认值为播放：running)  |    animation-play-state: paused;     |
|    animation-fill-mode    |      填充模式(默认值为none)      |    animation-fill-mode: forwards;    |

### `animation-name`

```css
/*动画名称*/

div{
  animation-name: test; /*在div这个dom中 调用动画 test*/
}


@keyframes test{} /*test就是自定义的动画名称*/
```

###`animation-timing-function`

```css
/*时间函数*/

/*时间函数可以应用于整个动画中，也可以应用于关键帧的某两个百分比之间*/
div{
    animation-timing-function: ease;
}

@keyframes test{
    0%{left: 0px;animation-timing-function: ease;}
}

/*
liner：匀速；ease：逐渐慢；ease-in：加速；ease-out：减速；ease-in-out:先加速后减速
*/
```

### `animation-iteration-count`

```css
/*循环次数*/

/*可以是整数、小数，不能是0和负数；infinite代表无限次*/
```

###`animation-delay`

```css
/*延迟时间*/

/*
1 - 该延迟时间是指整个动画的延迟时间，而不是每个循环的延迟时间，只在动画开始时进行 一次 时间延迟;
2 - 如果该值是负值，则表示动画的起始时间从0s变为延迟时间的 绝对值;
*/
```

### `animation-fill-mode`

```css
/*填充模式 - 定义动画开始帧之前和结束帧之后的动作*/

/*
none: 动画结束后，元素移动到初始状态
    [注意]初始状态并不是指0%的元素状态，而是元素本身属性值

forwards: 元素停在动画结束时的位置
    [注意]动画结束时的位置并不一定是100%定义的位置，因为动画有可能反向运动，也有可能动画的次数是小数

backwards:在animation-delay的时间内，元素立刻移动到动画开始时的位置。若元素无animation-delay时，与none的效果相同
    [注意]动画开始时的位置也不一定是0%定义的位置，因为动画有可能反向运动。

both: 同时具有forwards和backwards的效果
*/

/* 
1 - android2.1-3不支持animation-fill-mode;
2 - 当持续时间animation-duration为0s时，animation-fill-mode依然适用，
		当animation-fill-mode的值为backwards时，动画填充在任何animation-delay的阶段。
		当animation-fill-mode的值为forwards时，动画将保留在100%的关键帧上;

*/
```





## 示例

```html
<div></div>
```

```css
div{
    width: 300px;
    height: 100px;
    background-color: pink;
    animation-name: test; /* 动画名字 */
    animation-duration: 3s; /* 持续时间 */
    animation-timing-function: ease; /* 时间函数 */
    animation-delay: 0s; /* 延迟时间 - 多少秒后开始动画 */
    animation-iteration-count: infinite; /* 循环次数 */
    animation-direction: normal; /* 动画方向 */
    animation-play-state: running; /* 播放状态 */
    animation-fill-mode: none; /* 填充模式 */
}
/* 关于keyframes关键帧的内容稍后介绍     */
@keyframes test{
    0%{background-color: lightblue;}
    30%{background-color: lightgreen;}
    60%{background-color: lightgray;}
    100%{background-color: black;}
}
```



## animation动画实现

> animation制作动画效果需要两步，首先用关键帧声明动画，再用animation调用动画



### 关键帧声明动画 - @keyframes

> 关键帧的语法是以@keyframes开头，后面紧跟着动画名称animation-name。from等同于0%，to等同于100%。百分比跟随的花括号里面的代码，代表此时对应的样式



```css
@keyframes animation-name{
    from | 0%{}
    n%{}
    to | 100%{}
}

/* 
1 - 百分比顺序不一定非要从 0% 到 100% 排列，最终浏览器会自动按照 0%-100% 的顺序进行解析; 0% 不可以省略百分号;
2 - 如果存在负百分数或高于 100% 的百分数，则该关键帧将被忽略;
3 - 如果 0% 或 100% 不指定关键帧，将使用该元素默认的属性值;
4 - 若存在多个 @keyframes，浏览器只识别最后一个 @keyframes 里面的值; 空的keyframes规则是有效的;
*/
```

### 关键帧使用动画 - animation-name

```css
div{
  animation-name: test; /*在div这个dom中 调用动画 test*/
}
```



# API

>animation涉及到的事件有animationstart、animationend、animationiteration三个。这三个事件的bubbles都是yes，cancelable都是no
>
>[注意]对于safari浏览器，animation的事件为webkitAnimationStart、webkitAnimationEnd、webkitAnimationIteration
>
>[注意]动画事件只支持DOM2级事件处理程序的写法



##animationstart

```javascript
// 发生在动画开始时
/*
如果存在delay，且delay为正值，则元素等待延迟完毕后，再触发该事件

如果delay为负值，则元素先将初始值变为delay的绝对值时，再触发该事件
*/

oSb.addEventListener('animationstart', function () {
    this.innerHTML = '动画开始';
    this.style.background = 'lightgreen';
}, false);
```



## animationend

```javascript
// 发生在动画结束时

test.addEventListener('animationend',function(){
    this.style.background="lightgreen";
    this.innerHTML = '动画结束';
},false);
```



## animationiteration

```javascript
// 发生在动画的一次循环结束时，只有当iteration-count循环次数大于1时，触发该事件

var i = 0;
oSb.addEventListener('animationiteration',function(){
    i++;
    this.innerHTML = i;
},false);
```



```shell
1 - 只有改变animation-name时，才会使animation动画效果重新触发
2 - 这三个事件的事件对象，都有animationName和elapsedTime属性这两个私有属性
		animationName属性:返回产生过渡效果的CSS属性名
		elapsedTime属性:动画已经运行的秒数
3 - 对于animationstart事件，elapsedTime属性等于0，除非animation-delay属性等于负值
```

