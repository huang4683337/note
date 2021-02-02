

## 什么是 Promise

**是异步编程的一种解决方案  主要是为了解决回调函数产生的回调地狱**



## Promise 对象的特点

+ 存在三种状态 `padding`（进行中）、`fulfilled｜Resolved`（已成功）、`rejected`（已失败）

  这三种状态是由异步操作的结果来决定的，其他的任何操作都不能改变这个状态。

+ 一旦状态发生改变，状态再也不会改变，在任何时候都可以到这个结果。

  改变方式只有两种可能，从`padding`=>`fulfilled｜Resolved` 或者从 `padding`=>`rejected`

  一旦以上两种可能发生，这时我们就称为`resolved`（已定型），并且再也不会改变。

  如果改变已经发生，我们对`Promise`对象添加回调函数，就是立即得到结果。

+ 提供统一的接口



## Promise 对象的缺点

+ 无法取消 `Promise`，一旦新建它就会立即执行，无法中途取消
+ 如果不设置回调函数，`Promise`内部抛出的错误，不会反应到外部
+ 当处于`Pending`状态时，无法得知目前进展到哪一个阶段（刚刚开始还是即将完成）





## 基本用法

+ `Promise` 是一个构造函数
+ `Promise`  构造函数，接收一个函数作为参数
+ 这个函数拥有 `resolve` 和`reject` 函数作为参数

```js
new promise( (resolve, reject)=>{
  if(true){
    resolve('成功');
  }else{
    reject('失败');
  }
} )
```



### `resolve` 函数作用

+ 异步操作成功时调用

+ `Promise` 状态从未完成（Pending ）变为成功（ Resolved ）
+ 将异步操作的结果，作为参数传递出去



### `reject`函数作用

+ 异步操作失败时调用
+ `Promise` 状态从未完成（Pending ）变为成功（ Rejected ）
+ 将异步操作报出的错误，作为参数传递出去



### `then` 方法

**`Promise`实例生成后，可以通过 `then`方法分别指定 `resolve` 和 `reject` 状态的回调函数**

```js
promise.then(
	res=>{ console.log('状态为 Resolved 时会调用这个函数，res 就是异步操作成功返回的结果') },
  err=>{ console.log('状态为 Rejected 时会调用这个函数，err 就是异步操作失败返回的错误信息') }
)
```



## then 方法

+ `Promise.prototype.then()` 定义在原型对象 Promise.prototype上

+ 为 `Promise`  实例添加，状态发生改变时的回调函数

  第一个参数是：`resolve（ 成功 ）` 时的回调函数

  第二个参数是：`reject（ 失败 ）`  时的回调函数

+ `then`方法返回的是一个 新的 Promise实例，因此可以实现链式调用

```js
getJSON("/post/1.json")
    .then((post) => {
        return getJSON(post.commentURL);
    })
    .then((comments) => { console.log("Resolved: ", comments); }, (err) => { console.log("Rejected: ", err); });
```

1 - `getJSON("/post/1.json")` 调用成功返回  `resolve`，在 `then` 方法中第一个参数中获得返回结果，

2 - 在 `then` 中 retrun 一个新的 promise，`getJSON("/post/1.json").then()` 对应的就是一个新的 primise,

3 - `getJSON("/post/1.json").then().then()` 就可以获取到新的 promise 返回的状态。实现了链式调用 





## catch 方法

+ `Promise.prototype.catch`方法是`.then(null, rejection)`的别名，用于指定发生错误时的回调函数。

+ 只有在 `rejected` 状态下才能触发 `catch` 函数
+ `catch`  返回一个 `Promise` 对象，所以`catch` 后面还可以 `.then()`
+ `Promise` 对象的错误具有 “冒泡” 性质，会一直向后传递，直到被捕获为止。

```js
getJSON("/post/1.json")
    .then((post) => {
        return getJSON(post.commentURL);
    }).then((comments) => {
        // some code
    }).catch((error) => {
        // 处理前面三个Promise产生的错误
    });


// 在链式调用中，将 catch 放在最后，可以捕获前面所有的错误
```





## Promise.all() - 同时调用多个异步函数

+ 将多个 `Promise` 实例包装成一个新的实例

  ```js
  var p = Promise.all([p1, p2, p3]);
  ```

+ 等所有的异步函数都返回结果了，才会调用  `.then()`。相当于同时调用多个异步函数

+ 成功：调用成功返回的结果是数组 [ p1, p2, p3 ] 

  返回的结果数组是按照调用顺序，而不是谁先调用完

+ 失败：返回的结果是最先被 `rejeced`失败状态的值





## Promise.race() - 同时调用多个异步函数

+ 将多个 `Promise` 实例包装成一个新的实例

  ```js
  var p = Promise.race([p1, p2, p3]);
  ```

+ 哪个异步函数先调用完，就先返回哪个异步函数的结果





## Promise.resolve()

+ 将现有对象转为 `Promise` 对象

```js
var jsPromise = Promise.resolve($.ajax('/whatever.json'));
```

```js
Promise.resolve('foo')
// 等价于
new Promise(resolve => resolve('foo'))
```



### （1）resolve 参数是一个 Promise 实例

如果参数是Promise实例，那么`Promise.resolve`将不做任何修改、原封不动地返回这个实例。



### （2）参数是一个 `thenable `对象

`thenable `对象指的是具有 `then` 方法的对象

```js
let thenable = {
  then: function(resolve, reject) {
    resolve(42);
  }
};
```

`Promise.resolve `方法会将这个对象转为 Promise 对象，然后就立即执行 `thenable` 对象的 `then` 方法。

```js
let thenable = {
  then: function(resolve, reject) {
    resolve(42);
  }
};

let p1 = Promise.resolve(thenable);
p1.then(function(value) {
  console.log(value);  // 42
});
```



### （3）参数不是具有`then`方法的对象，或根本就不是对象

如果参数是一个原始值，或者是一个不具有`then`方法的对象，

则`Promise.resolve`方法返回一个新的Promise对象，状态为`Resolved`。

```js
var p = Promise.resolve('Hello');

p.then(function (s){
  console.log(s);		// Hello
});
```



### （4）不带有任何参数

`Promise.resolve`方法允许调用时不带参数，直接返回一个`Resolved`状态的Promise对象。

所以，如果希望得到一个Promise对象，比较方便的方法就是直接调用`Promise.resolve`方法。

```js
var p = Promise.resolve();

p.then(function () {
  // ...
});
```

需要注意的是，立即`resolve`的Promise对象，是在本轮“事件循环”（event loop）的结束时，而不是在下一轮“事件循环”的开始时。

```js
setTimeout(function () {
  console.log('three');
}, 0);

Promise.resolve().then(function () {
  console.log('two');
});

console.log('one');

// one
// two
// three
```





## Promise.reject()

+ `Promise.reject(reason)`方法也会返回一个新的Promise实例
+ 该实例的状态为`rejected`
+ 参数用法与 `Promise.resolve `方法完全一致。



## done()



## finally()



