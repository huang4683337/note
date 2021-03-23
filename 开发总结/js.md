# JS将网址转换成可点击的超链接

```js
<script>
window.onload=function(){
  var div = document.getElementById("container");
  var s=div.innerHTML;
var re = /(http:\/\/[\w.\/]+)(?![^<]+>)/gi;
  div.innerHTML=s.replace(re,"<a href='$1' target='_blank'>$1</a>");
}
</script>
```

https://blog.csdn.net/changzhen11/article/details/84106465