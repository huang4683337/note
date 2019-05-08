##api文件夹

###axiosConfig.js

```javascript
require('es6-promise').polyfill();
import axios from 'axios';
import Qs from 'qs';

// axios 配置
axios.defaults.timeout = 50000;
axios.defaults.paramsSerializer = function (params) {
  return Qs.stringify(params, { arrayFormat: 'indices',allowDots: true })
};
axios.defaults.withCredentials=true;
axios.defaults.headers.post['Content-Type'] = 'application/json';
axios.defaults.headers.put['Content-Type'] = 'application/json';
axios.defaults.headers.delete['Content-Type'] = 'application/json';
axios.defaults.headers.patch['Content-Type'] = 'application/json';
axios.defaults.transformRequest= [function (data) {
  // Do whatever you want to transform the data
  return JSON.stringify(data);
}];
// http request 拦截器
axios.interceptors.request.use(
  config => {
    //在发送请求之前做某事，比如说 设置loading
    replacePathVars(config);
    return config;
  },
  err => {
    //请求错误做哪些事情
    return Promise.reject(err);
  });

// http response 拦截器
axios.interceptors.response.use(
  response => {
    //对响应数据做些事，比如说把loading动画关掉
    return response;
  },
  error => {
    return Promise.reject(error.response)
  });

function replacePathVars(config) {
  let params = config.params||{};
  if (config.data != undefined) {
    params = config.data;
  }

  config.url = config.url.replace(/{([^{}]+)}/g, function (a, b, index, input) {
    let val = params[b];
    if (val != undefined && val != null) {
      delete params[b];
      return val;
    }
    return a;
  });
}
export default axios;

```



### index.js

```javascript
export default {
  shareAPI: require('./shareAPI').default,
}
```



### share.js

```javascript
import axios from './axiosConfig';

export default {

  getRecCourse: function (params) {
    var url = '/app/lesson/getRecCourse';
    return axios.get(url, params);
  },
  getCourseInfo: function (params) {
    var url = '/app/getCourseInfo';
    return axios.get(url, params);
  },
  checkAuthentication: function (params) {
    var url = '/app/video/checkAuthentication';
    return axios.get(url, params);
  },

};
```

