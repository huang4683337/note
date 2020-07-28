## this 指向


> 注意: 
>
> 1、this 的指向在函数定义的时候是确定不了的, 只有在函数执行的时候才能确定 this 指向谁;
>
> 2、实际上 this 指向的是最终调用他的那个对象 ==> 谁调用 让方法 去执行, this 就指向谁
>
> 3、构造函数中, this 指向 new 的实例
>
> 4、构造函数中,  如果 return 的是一个复杂的数据类型, this 指向的就是返回的那个复杂的数据类型, 
>
> 若否: this 还是指向实例
>
> 5、在严格模式中的默认的 this 不再是 window ，而是 undefined
>
> 6、箭头函数本身是没有 `this` , 箭头函数的 `this` 指向定义此函数的哪个作用域



```javascript
function a(){

  var user = "黄";

  console.log( this.user ); //undefined

  console.log( this ); //Window

}

a();

//原因是, 调用方法a是在全局进行调用的,它相当于是window对象在调用方法 ==> window是调用者

window.a();
```



```javascript
var o = {

  user: "黄",

  fn: function() {

    console.log(this.user);  //黄

  }

}

o.fn();

//调用者是对象o, 所以this指向对象o;

window.o.fn(); //this指向最终的调用者o
```



```javascript
var o = {

  a:10,

  b:{

    a:12,

    fn:function(){

      console.log(this.a); //12

    }

  }

}

o.b.fn();  //this指向最终的调用者b
```



```javascript
var o = {

  a:10,

  b:{

    a:12,

    fn:function(){

      console.log(this.a); //undefined

      console.log(this); //window

    }

  }

}

var j = o.b.fn;

j();  //window对象调用, 执行的这个方法
```



```javascript
function Fn() {

  this.user = "黄";

}

var a = new Fn();

console.log(a.user);  //构造函数中, this指向new的实例
```



```javascript
function fn(){

  this.user = "黄";

  // return {user:"对象"};

  // return [];

  // return function(){};  // 在构造函数中, 如果return 的是一个对象, this.指向的就是返回的那个对象

  return 1;  //return 的不是一个对象, this仍旧指向实例

}

var b = new fn;

console.log(b.user)
```



### 箭头函数


```js
// 定义变量 、函数
var a = 10;
var obj = {
  a:'obj.a',
  fn: function(callback){
    callback();
	}
}
```

```js
obj.fn(()=>{
  console.log(this); // window
})

// 等价于
let ff = () => { console.log(this) }	// this 指向定义 ff 函数的作用域 window
obj.fn(ff);
```

```js
obj.fn(function(){
	console.log(this);	// window; 回调函数 callback 是 window 在调用执行, 所以 this 指向 window
})

obj.fn(function(){ console.log(this)}.bind(obj) );	// 'obj.a'; 通过 bind 改变 this 指向
```



```js
// 箭头函数无法改变 this, 因为箭头函数没有自己的 this

// 所以以下都会报错
var a = () => {}.bind(this);
var a = () => {}.call(this);
var a = () => {}.apply(this);
```







## 改变 this 指向

+ 将 a 的方法和属性给 b 使用

  ```js
  a.call(b, x, y, ...); || a.call(this, x, y, ...);
  a.apply(b, []); || a.apply(this, []);
  a.bind(b, xx); || a.bind(this, xx);
  ```

+ 如果参数 b 为 `null` 时，指向 `window`。 严格模式下指向 `undefined`

+ `apply` 和 `call` 会立马执行对应的函数；`bind` 会返回一个新的函数，想什么时候调都可以。



### apply、call

```js
// 使用其他对象的方法
function Animal(){
    this.name = 'Animal';
    this.sayName = function(){
        console.log(this.name);
    }
}

function Cat(){
    this.name = 'Cat';
}

let animal = new Animal();
let cat = new Cat();

/*
	cat 对象使用 animal 对象的 sayName 方法
*/
animal.sayName.call(cat);		// Cat
animal.sayName.apply(cat);	// Cat
```

```js
// 用于继承，调用父的构造函数
function Animal(name){
    this.name = name;
    this.sayName = function(){
        console.log(this.name);
    }
}

function Cat(name){
    // Animal.call(this, name);
    Animal.apply(this, [name]);
}

let cat = new Cat('is Cat');

cat.sayName();	// is Cat
```





### apply 、call 的应用

```js
// 加法运算
function sum(a, b) {
    console.log('sum', a + b);
    console.log(this);
}

// 减法运算
function sub(a, b) {
    console.log('sub', a - b);
}

sub(10,1);	// 9

// 想要在 sub 方法中实现加法运算；将 sum 方法的功能继承到 sub
sum.call(sub, 1, 2);    // sum 3;  this 指向 sub
sum.apply(sub,[1,4]);   // sum 5;  this 指向 sub
```

```js
// 数组之间的追加
let arr = [1, 2, 3, 4];
let arr1 = ['a', 'b', 'c'];

Array.prototype.push.call(arr, ...arr1);
console.log( arr );	// [1, 2, 3, 4, 'a', 'b', 'c'];

Array.prototype.push.apply(arr, arr1);
console.log( arr );	// [1, 2, 3, 4, 'a', 'b', 'c'];
```

```js
// 利用 apply 求数组中的最大、最小值
/*
	原理：
		因为 apply 接收一个数组形式的参数。 xx.apply(null, []);
		利用这个特性加上 Math.max、Math.mian 来实现这个功能
*/

let arr = [22, 4, 13, 7, 44, 1, 55];

let max = Math.max.apply(null, arr);
console.log(max);	// 55

let min = Math.min.apply(null, arr);
console.log(min);	// 1
```

```js
// 验证是否为数组
// 将 Object 中的 toString 方法继承给数组去用。
let arr = [22, 4, 13, 7, 44, 1, 55];

function isArray(obj) {
    return Object.prototype.toString.call(obj) === '[object Array]';
}

console.log( isArray(arr) );	// true
```

```js
// 伪数组使用数组方法
let allDom = document.getElementsByTagName('*');

let domCall = Array.prototype.slice.call(allDom);
console.log(domNodes);

let domApply = Array.prototype.slice.apply(allDom);
console.log(domNodes);
```



### bind

**原理：** `xx.bind(module)` 时，`bind()` 方法会创建一个新的函数，这个新函数时 `xx`的拷贝，这个新的函数的 `this` 会指定为 `bind(module)`的第一个参数 `module`，之后的参数会被当作新的函数的参数。

**创建绑定函数**

```js
this.num = 9;

let OtherNum = { num: 66 };

let ObjNum = {
    num: 88,
    getNum: function () {
        return this.num;
    }
}


console.log(ObjNum.getNum());		// 88

let win = ObjNum.getNum;
console.log(win());		// 9

// 通过 bind 将 this 绑定至 ObjNum
let otherNum = ObjNum.getNum.bind(OtherNum);
console.log(otherNum());		// 66
```



**偏函数**

使一个函数拥有预设的初始参数。

需要将预设的参数当作 `bind()`  的参数，写在第二个参数起的位置。

当绑定的函数被调用时，这些参数会被插入到目标函数的参数列表的开始位置，传递给绑定函数的参数会跟在它们后面。


```js
function list() {
    /*
        将数组的 slice 方法中的 this 指向 arguments，
        也就是说将数组的 slice 方法交给 arguments 使用
        因为 call、apply 的特性，会自动执行一次 slice 方法把 arguments 拆分成数组
    */
    let arr = Array.prototype.slice.call(arguments);
    console.log(arr);
}

let defaultList = list.bind(null, 37);

defaultList();	// [37]
```

```js
function sum(a, b){
    return a + b;
}

// 通过 bind 创建一个绑定函数，默认第一个参数是 37
let defaultSum = sum.bind(null, 37);

// 因为第一个参数已经被默认绑定，所以 defaultSum 函数穿进的参数是从第二个开始的。
console.log( defaultSum(1) );	// 37 + 1 = 38;
console.log( defaultSum(3, 100) );	// 37 + 3 = 40;
```



**配合 setTimeout**

在 `setTimeout` 的回调函数中 `this` 是指向 `window`， 因为`windwo.setTimeout()`。

通过 `bind()` 改变 `setTimeout` 中的 `this` 指向。

```js
let Fn = {
    bloom: function(){
        setTimeout(function(){
            this.declare();
        }.bind(this), 1000)
    },
    declare: function(){
        console.log('declare')
    }
}

Fn.bloom();	// declare
```



**快捷调用**

```js
Array.prototype.slice.apply(arguments);

let slice = Function.prototype.apply.bind(Array.prototype.slice);
slice(arguments);
```





## 匿名函数

```js
function a(){}
// 等于
var a = function(){}
```



```js
// 这是一个函数声明
function a(){
  
}

// 这是一个表达式
(function(){
})

// js 在预编译阶段会解释（编译）函数声明，但不会解释（编译）表达式。
```