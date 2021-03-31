## axios 的 token 过期

[window.top](http://222.173.43.133:8025/jsref/prop_win_top.asp.htm)

```js
// token 过期时
// 重定向至登录页 || 原地址返回走鉴权至登录页
this.http.httpService.interceptors.response.use(res => {
    return res
}, error => {
    if (error.response.status === 401) {
        let url: string = top.location.href;
        top.location.replace(url)
    }
    console.log(error);
})
```

