## 前言

浏览器是一个进程，js是浏览器的一个线程（单线程）。也就是说JS同一时间只能做一件事。

为了协调事件、用户交互、脚本、UI 渲染和网络处理等行为，防止主线程的不阻塞，Event Loop 的方案应用而生。



js执行主要经历以下四个：

+ JS引擎
+ 浏览器管理模块（浏览器API）
+ 回调队列 / 异步队列 / 任务队列
+ 事件循环（Event-Loop）





## Event-Loop

`Event-Loop` 包含两类

+ [Browsing Context](https://link.zhihu.com/?target=https%3A//html.spec.whatwg.org/multipage/browsers.html%23browsing-context) 执行上下文
+ [Worker](https://link.zhihu.com/?target=https%3A//www.w3.org/TR/workers/%23worker)  实现了JS的多线程

二者的运行是独立的，也就是说，每一个 JavaScript 运行的**线程环境**都有一个独立的 Event Loop，每一个 Web Worker 也有一个独立的 Event Loop。





## 任务队列

+ **任务队列机制**：事件循环是通过任务队列的机制来进行协调的。

+ **一对多**：一个 Event Loop 中，可以有一个或者多个任务队列(task queue)，一个任务队列便是一系列有序任务(task)的集合。

+ **任务源**：每个任务都有一个任务源(task source)，源自同一个任务源的 task 必须放到同一个任务队列，从不同源来的则被添加到不同队列。

+ setTimeout/Promise 等API便是任务源，而进入任务队列的是他们指定的具体执行任务。



**在事件循环中，每进行一次循环操作称为 tick。每次 tick的处理大概如下：**

+ 从任务队列中一个取出最先入队的任务**(宏任务)**。如果有责执行一次。
+ 检查当前任务**(宏任务)** ，执行过程冲是否产生`微任务(Microtasks)`，如果有则执行，直至清空`Microtasks Queue(微任务队列)`
+ 渲染 render
+ 执行下一个 tick



**在上述tick的基础上需了解**：

+ JS分为同步任务和异步任务
+ 同步任务都在主线程上执行，形成一个执行栈
+ 主线程之外，**事件触发线程**管理着一个任务队列，只要异步任务有了运行结果，就在任务队列之中放置一个事件。
+ 一旦执行栈中的所有同步任务执行完毕（此时JS引擎空闲），系统就会读取任务队列，将可运行的异步任务添加到可执行栈中，开始执行。



## 宏任务

`(macro)task`，可以理解是**每次执行栈执行的代码**就是一个宏任务（包括每次**从事件队列中获取一个事件回调**并放到执行栈中执行）。

浏览器为了能够使得JS内部`(macro)task(宏任务)`与`DOM`任务能够有序的执行，会在一个`(macro)task(宏任务)`执行结束后，在下一个`(macro)task (宏任务)`执行开始前，对页面进行重新渲染，流程如下：

```js
(macro)task->渲染->(macro)task->...
```



**宏任务包含**

```text
script(整体代码)
setTimeout
setInterval
I/O
UI交互事件
postMessage
MessageChannel
setImmediate(Node.js 环境)
```





## 微任务

`microtask(微任务)`,可以理解是在当前 `task(任务)` 执行结束后立即执行的任务。也就是说，在当前`task(任务)`后，下一个`task(任务)`之前，在渲染之前。

所以**微任务的响应速度比宏任务更快**，因为无需等待渲染。也就是说，在某一个`macrotask(宏任务)`执行完后，就会将在它执行期间产生的所有`microtask(微任务)`都执行完毕（在渲染前）。



**微任务包含：**

```text
Promise.then
Object.observe
MutaionObserver
process.nextTick(Node.js 环境)
```





## 运行机制

- 执行一个宏任务（栈中没有就从事件队列中获取）
- 执行过程中如果遇到微任务，就将它添加到微任务的任务队列中
- 宏任务执行完毕后，立即执行当前微任务队列中的所有微任务（依次执行）
- 当前宏任务执行完毕，开始检查渲染，然后GUI线程接管渲染
- 渲染完毕后，JS线程继续接管，开始下一个宏任务（从事件队列中获取）