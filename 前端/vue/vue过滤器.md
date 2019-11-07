# vue过滤器

## 编写过滤文件(全局过滤器)

```javascript
// 在src 文件夹中新建filters文件夹，文件夹里面新建index.js文件，用来声明过滤器的实现方法并导出
/**
 * 去除首尾空格
 * @param  {[string]} string [字符串]
 * @return {[string]}        [返回处理后数据]
 */
const trim = (string) => {
  return string.toString().replace();
}

const subString = (value, length = 10) => {
  if (value && value != null && value.length > length) {
    return value.substring(0, length)+'...'
  }
  return value;
};
//格式化时间
 const formatDate=(date, formatStr) => {
     var date = new Date(date);
     /*
      函数：填充0字符
      参数：value-需要填充的字符串, length-总长度
      返回：填充后的字符串
      */
     var zeroize = function (value, length) {
     if (!length) {
     length = 2;
   }
value = new String(value);
for (var i = 0, zeros = ''; i < (length - value.length); i++) {
  zeros += '0';
}
return zeros + value;
};

if (!formatStr) {
  formatStr = 'yyyy-MM-dd';
}

return formatStr.replace(/"[^"]*"|'[^']*'|\b(?:d{1,4}|M{1,4}|yy(?:yy)?|([hHmstT])\1?|[lLZ])\b/g, function ($0) {
  switch ($0) {
    case 'd':
      return date.getDate();
    case 'dd':
      return zeroize(date.getDate());
    case 'ddd':
      return ['周日', '周一', '周二', '周三', '周四', '周五', '周六'][date.getDay()];
    case 'dddd':
      return ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday'][date.getDay()];
    case 'M':
      return date.getMonth() + 1;
    case 'MM':
      return zeroize(date.getMonth() + 1);
    case 'MMM':
      return ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'][date.getMonth()];
    case 'MMMM':
      return ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December'][date.getMonth()];
    case 'yy':
      return new String(date.getFullYear()).substr(2);
    case 'yyyy':
      return date.getFullYear();
    case 'h':
      return date.getHours() % 12 || 12;
    case 'hh':
      return zeroize(date.getHours() % 12 || 12);
    case 'H':
      return date.getHours();
    case 'HH':
      return zeroize(date.getHours());
    case 'm':
      return date.getMinutes();
    case 'mm':
      return zeroize(date.getMinutes());
    case 's':
      return date.getSeconds();
    case 'ss':
      return zeroize(date.getSeconds());
    case 'l':
      return date.getMilliseconds();
    case 'll':
      return zeroize(date.getMilliseconds());
    case 'tt':
      return date.getHours() < 12 ? 'am' : 'pm';
    case 'TT':
      return date.getHours() < 12 ? 'AM' : 'PM';
  }
});
};
 const transformJsonData=(data,json)=>{
   for(let key in json){
     if(key==data){
       return json[key]
     }
   }
 };

 // 转换面议
 const price=(val)=> {
   if(val=="-1"){
     return "面议"
   }else{
     return val
   }
 };


function padLeftZero (str) {
    return ('00' + str).substr(str.length);
};

export default {
  trim, subString,formatDate,transformJsonData,price
}

```



## 使用

```javascript
// 在main.js中将此文件导入，并定义过滤器

Object.keys(filters).forEach(key => {
  Vue.filter(key, filters[key]);
});

/*
过滤器只能使用在v-bind和双花括号里，以管道符 “|”隔开，参数类似于函数的形式 如：{{msg | filter(param)}}
*/ 
```



## 局部过滤器

```javascript
 filters: {
    capitalize: function (value) {
      /* 业务逻辑 */
    }
  }
```

