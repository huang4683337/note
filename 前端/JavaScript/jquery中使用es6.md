在jquery中已经能使用promise了



```javascript
// 使用回调函数

$.get(
    "/greet",
    {name: 'Brad'},
    function(data) {
       $.get();
    },
    "json"

```



```javascript
// 使用promise

$.get('url-1')
.then( (res)=>{
	console.log('url-1的成功函数返回的data'，url-1);
  return $.get('url-2');
})
.then( (res)=>{
	console.log('url-2的成功函数返回的data'，url-2)
})
```



## js中promise封装ajax

```javascript
function  get(url){
  return new Promise(resolve, reject){
    var oReq = new XMLHttpRequest();
    // 请求加载成功后 调用
    oReq.onload = function(){
      resolve(oReq.responseText);
    }
    oReq.onerror = function(err){
			reject(err);
    }
    oReq.open('get',url,true);
    oReq.send();
  }
}


//使用方法

get('url')
.then( data=>{
  console.log(data)
})
```

```javascript
// 同时支持回调函数和promise

function  get(url，callback){
  return new Promise(resolve, reject){
    var oReq = new XMLHttpRequest();
    // 请求加载成功后 调用
    oReq.onload = function(){
      resolve(oReq.responseText); //promise
      callback && callback(oReq.responseText); //回调  逻辑运算符 且，当第一个条件不寻在的时候 是不会向后面执行
    }
    oReq.onerror = function(err){
			reject(err);
      callback(err);
    }
    oReq.open('get',url,true);
    oReq.send();
  }
}
```

