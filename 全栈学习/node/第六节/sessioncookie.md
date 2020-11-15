+ 什么是cookie

  浏览器的一种约定。

  cookie 不能用于鉴权， 因为他很容易被篡改

  浏览器只会显示本域cookie，无法看到其他域的cookie

  + 能看件、能修改
  + 容量大小限制
  + 不能存实例对象

+ session-cookie

  浏览器每次访问会将设置的cookie带到服务端，

  cookie中会有一个key，

  通过这个 key 在  session 中读取信息

  + key :  用来读取服务端存储的信息，存在 cookie 中
  + value ： 服务端存储的信息，存在 session 中

  httpOnly：true ==》不允许在浏览器中通过 js 获取cookie

+ hash？

  + 雪崩效应：密文和明文之间的差距越大越好，不易推测出规律

+ redis 相当于全局 session

  为什么要全局 session？

  + 实际开发中可能会存在多台服务，用户第一次访问的是服务1，信息存储在服务1上,

    第二次进入时，如何保证还能访问服务1呢？这种不号实现。

    那么就可以存一个全局的 session，不管访问那个服务器，都会通过全局session，

    都会得到对应的消息

  

## session-cookie 鉴权

思路 :

+ 第一步：

  请求的是登录接口，就让他过去，实现访问。==> 匹配账号密码 ==> 用户信息存入 session

  其他接口访问，是否登录 ，登录是否过期

+ 第二步：

  退出 或者登录过期，清除 session



## token

session 不足

+ 因为使用了 cookie ，使它无法脱离浏览器



使用 axios 中的拦截器，将 token 添加到请求头带给服务器



toke组成：三部分

+ 令牌头：是一个base64编码的 json
+ payload（载荷）：明文形式存储 用户信息，时间戳组成的json经过base64 编码
+ 哈希，签名



## sso 单点登录



