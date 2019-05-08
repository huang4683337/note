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
> ​	若否: this 还是指向实例
>
> 5、在严格模式中的默认的 this 不再是 window ，而是 undefined



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

