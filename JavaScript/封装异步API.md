# 封装异步API

如下：想要得到延时器里面 `data`  的值，有什么好的办法呢？ 

```javascript
function fn(){
    setTimeout(()=>{
        var data = 'hello';
    }, 1000)
}

console.log(fn());  // undefined --> 想要输出结果为 setTimeout 中 data 的值
```



可以通过回调函数来实现我们想要的结果

```javascript
// 回调函数 fn ，将一个方法当作参数传过去
fn(function (data) {
    console.log(data)
})

// 上面的函数调用，可以理解为下面的这种情况
function fn() {
  
    var callback = function (data) {
        console.log(data)
    }
}

// 在 延时器中调用 callback 方法
setTimeout(() => {
  
    var data = 'hello';
    callback(data);

})
```



最终结果

```javascript
function fn(callback) {

    // var callback = function (data) {
    //     console.log(data)
    // }

    setTimeout(() => {
        var data = 'hello';
        callback(data);
    })

}


fn(function (data) {
    console.log(data)
})

```

