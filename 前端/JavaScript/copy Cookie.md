**copy Cookie**

```javascript
//用别人的接口做购物车时 我们没有cookie 找到他的拷贝下来



function copyCookie(str){

		var arr = str.split("; ");
		
  	arr.forEach(function(element) {
      
				document.cookie =  element
		
    });

}



//copyCookie(别人的cookie)
```

