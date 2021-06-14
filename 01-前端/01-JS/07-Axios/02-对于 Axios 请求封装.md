## 引入拦截封装、element

```js
import instance from './intercept.js'
import { Message, Loading } from 'element-ui';
```



## 请求参数处理

```
对于 loading 的处理
- Loading.service();
	- 以为服务的方式调用 loading
	- 生成的 loading 是单例的。
	- 在一个 loading 调用一个新的 loading 并不会返回新的实例，而是返回现有的。
	- 通过 close() 关闭loading
	- 利用 finally{ ... } 在所有接口请求完毕后关闭 loading
```

```
- 此方法返回一个新的 Promises 实例
- instance.then 中接收 axios 拦截返回的 resovle()
- instance.catch 中接收 axios 拦截返回的 reject()
	- 4xx 错误
	- 5xx 错误
	- 跨域、请求超时
```



```js
/**
 * 核心函数，可通过它处理一切请求数据，并做横向扩展
 * @param {url} 请求地址
 * @param {params} 请求参数
 * @param {options} 请求配置，针对当前本次请求；
 * @param loading 是否显示loading
 * @param mock 本次是否请求mock而非线上
 * @param error 本次是否显示错误
 */
function request(url, params, options = { loading: true, mock: false, error: true }, method) {
    let loadingInstance;
    // 请求前loading
    if (options.loading) loadingInstance = Loading.service();
    return new Promise((resolve, reject) => {
        let data = {}
        // get请求使用params字段
        if (method == 'get') data = { params }
        // post请求使用data字段
        if (method == 'post') data = { data: params }
        // 通过mock平台可对局部接口进行mock设置
        if (options.mock) url = 'http://www.mock.com/mock/xxxx/api';
        instance({
            url,
            method,
            ...data
        }).then((res) => {
            if (res.code === 200) {
                resolve(res);
            } else {
                // 通过配置可关闭错误提示
                Message.closeAll();
                if (options.error) Message.error(res.msg || '响应异常');
                reject(res);
            }

        }).catch((err) => {
            // 跨域、请求超时等
            Message.closeAll();
            if (options.error) Message.error(err.msg || '响应异常');
        }).finally(() => {
            loadingInstance.close();
        })
    })
}
```



## 对于 get、post 等方式的封装

```js
// 封装GET请求
function get(url, params, options) {
    return request(url, params, options, 'get')
}
// 封装POST请求
function post(url, params, options) {
    return request(url, params, options, 'post')
}

export default {
    get, post
}
```

