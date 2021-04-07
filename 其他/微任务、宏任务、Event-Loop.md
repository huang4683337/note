## 前言

浏览器是一个进程，js是浏览器的一个线程（单线程）。也就是说JS同一时间只能做一件事。

为了协调事件、用户交互、脚本、UI 渲染和网络处理等行为，防止主线程的不阻塞，Event Loop 的方案应用而生。



js执行主要经历以下四个：

+ JS引擎
+ 浏览器管理模块
+ 回调队列 / 异步队列
+ 事件循环（Event-Loop）





## Event-Loop

`Event-Loop` 包含两类

+ [Browsing Context](https://link.zhihu.com/?target=https%3A//html.spec.whatwg.org/multipage/browsers.html%23browsing-context) 执行上下文
+ [Worker](https://link.zhihu.com/?target=https%3A//www.w3.org/TR/workers/%23worker)  实现了JS的多线程







## 微任务



## 宏任务



