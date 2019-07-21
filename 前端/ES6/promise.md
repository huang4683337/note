## 回调地狱

![回调地狱](/Users/mrhuang/Downloads/笔记图片/回调地狱.jpeg)

> 多个异步方法的嵌套

为了解决回调地狱嵌套，所以在ES6中新增了一个API（promise）



## promise基础

```javascript
// promise是一个构造函数

// 1- 创建一个promise容器, 容器初始状态为 Pending
/*
resolve: 容器中函数执行成功 Pending ==> resolve ==> resolve 为 .then() 中的回调函数res
reject: 容器中函数执行失败 Pending ==> reject ==> resolve 为 .then() 中的回调函数err
*/

new Promise((resolve, reject) => {
    setTimeout(() => {
      	if(/*异步操作成功*/){
           resolve('成功了');
        }else{
           reject('失败了');  
        }
    }, 2000)
})
.then(
  		res => { console.log(res) },  // 成功了
      err => { console.log(err) },  // 失败了
)
```



## promise链式调用

```javascript
new Promise((resolve, reject) => {})
.then(res => { return '下一个 then 来接我' })
.then(res => { console.log(res) })
// 下一个 then 来接我
```

如果在 .then() 中 `return new promise()`  , 那么在下一个 .then()中的第一个参数 就是他的结果

```javascript
let p = new Promise((resolve, reject) => {
    resolve('我是promise ==> p ');
})


new Promise((resolve, reject) => {
    resolve('成功');
})
.then(res => { return p })
.then(res => { console.log(res) })  //我是promise ==> p
```



## promise封装异步函数

```javascript
function pro(a){
    return new Promise((resolve, reject)=>{
        resolve(a);
    });
}

pro(1)
.then( res=>{
    console.log(res);
    return pro(19);
})
.then( res=>{
    console.log(res);
    return pro(3);
})
.then( res=>{
    console.log(res);
})

// 1 19 3 
```



## Promise 的then 与 async await 写法相互转化

```javascript
async function f() {

    let promise = new Promise((resolve, reject) => {
        setTimeout(() => resolve("done!"), 1000)
    });

    let result = await promise; // 等待至promise获得结果 (*)

    console.log(result); // "done!"
}

f();
```

