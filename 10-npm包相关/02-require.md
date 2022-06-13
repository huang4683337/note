## require 注意

运行时加载 ---- 加载某个模块时模块会执行一次。

```js
require 某个文件，因为会执行一次，之后直接调用缓存，这就相当于直接将 require 的内容放入到当前的文件中。

结果会导致: 编译之后会将 require 中的内容编译到当前文件中
```

```js
// a.json
{url:'xxx.xxx.com'}

// index.js
require('a.json')
// 编译结果是 a.json 也会编译到 index.js 中
// 在 a.json 中修改 url 配置，index 当中的 url 不会改变


// 解决方案
// 1 - ajax.js 中 XMLHttpRequest 在线上环境请求到 a.json，
// 2 - export const a = 请求到的 a.json 中的内容
// 3 - import { a } from 'ajax.js'
```

