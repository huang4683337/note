## 初始化

```shell
# 新建一个文件夹，名字为：create_npm_package

# 初始化
$ npm init

# 因为 npm 包默认入口文件为 index.js
# create_npm_package 下新建文件 index.js
# 代码在下方

# 将包发布到 npm
$ npm publish
# 如果提示没有登陆，就进行登陆

# 登陆 npm
$ npm login
```



**create_npm_package/index.js **

```js
exports.showMsg = function () {
    console.log('Tis is test a npm');
}
```



## 添加本地软链

**将本地的包生成一个软链接，然后就可以在本地的其他项目中调用这个包**

```shell
# 进入 create_npm_package 文件夹

# 添加软链
$ npm link
or
$ yarn link

# 获取软链地址（本地包的地址）
$ pwd	# 得到地址为 /xx/yy/xxx
```



## 使用软链

```shell
# 新建文件夹 npm_test

# npm 初始化
$ npm init -y

# 使用软链
$ npm link /xx/yy/xxx

# npm_test 新建 index.js 文件

# 执行 index.js
$ node index.js

# 调用了 npm 包导出的方法，输出了：Tis is test a npm
```

```js
// npm_test/index.js

const npm_package = require('create_npm_package');
npm_package.showMsg();
```

