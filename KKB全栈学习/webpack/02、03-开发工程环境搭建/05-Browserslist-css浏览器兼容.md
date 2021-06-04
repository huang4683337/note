## Browserslist

browserslist 实际上就是声明了一段浏览器的集和，我们的工具可以根据这段集和描述，针对性的输出兼容性的代码。

[Browserslist]()就是帮助我们来设置目标浏览器的工具。Browserslist 被广泛应用到 Babel、postcss-preset-env、autoprefixer 等开发工具上。



## 配置

Browserslist 的配置可以放在 `package.json` 中，也可以单独放在配置文件  `.browserslistrc` 中。所有的工具都会主动查找 browserslist 的配置文件，根据 browserslist 配置找出对应的目标浏览器集和。



Browserslist 数据都是来自 CanIUse ：https://browserl.ist/

可惜网站关闭了：现在需要手动检测

```shell
# 使用 browserslist 代替网站

$ npx browserslist "last 1 versions, >1%"
# or
$ npx browserslist
```



在 `package.json` 中的配置是增加一个 `browserslist` 数组属性：

```js
{
  "browserslist":["last 2 version", "> 1%", "maintained node versions", "not ie < 11"]
}
```

或者在项目根目录创建一个 `.browserslistrc` 文件：

```shell
# 注释是这样写的，以#号开头
# 每行一个浏览器集和描述
last 2 version
> 1%
maintained node version
not ie < 11
```



## 常见集和范围说明

| 范围                        | 说明                                                         |
| --------------------------- | ------------------------------------------------------------ |
| `last 2 versions`           | https://caniuse.com 网站跟踪的最新两个版本，假如 IOS 12 是最新版本，那么向后兼容两个版本就是 IOS 11 和 IOS 12 |
| `> 1%`                      | 全球超过 1% 人使用的浏览器，<br />类似 `> 5% in US` 则代指美国 5% 以上用户 |
| `cover 99.5%`               | 覆盖 99.5% 主流浏览器                                        |
| `chrome >50` `ie 6-8`       | 指定某个浏览器范围                                           |
| `unreleased versions`       | 说有浏览器的 beta 版本                                       |
| `not ie < 11`               | 排除 ie11 以下版本不兼容                                     |
| `since 2013` `last 2 years` | 某时间范围发布的所有浏览器版本                               |
| `maintained node versions`  | 所有被 node 基金会维护的 node 版本                           |
| `current node`              | 当前环境的 node 版本                                         |
| `dead`                      | 通过`last 2 versions` 筛选浏览器中，全球使用率低于`0.5%` 且官方声明不在维护或者事实上已经两年没有再更新的版本 |
| `defaults`                  | 默认配置，`> 0.5%` `last 2 versions` `Firefox ESR` `not dead` |



## 浏览器名称列表（大小写不敏感）

+ `Android` ： 安卓 webview 浏览器
+ `Baidu` : 百度浏览器
+ `BlackBerry` / `bb`：黑莓浏览器
+ `Chrome` ：chrome 浏览器
+ `ChromeAndroid` / `and_chr` ：chrome 安卓移动浏览器
+ `Edge` ：微软 Edge 浏览器
+ `Electron` : Electron
+ `Explorer` / `ie` ：ie 浏览器
+ `ExplorerMobile` / `ie_mob` ：ie 移动浏览器
+ `Firefox` / `ff` ：火狐浏览器
+ `FirefoxAndroid` / `and_ff` ：火狐安卓浏览器
+ `ios` / `ios_saf` ：ios Safari 浏览器
+ `Node` ：nodejs
+ `Opera` ：Opera 浏览器
+ `OperaMini`/ `op_mini` ：OperaMini 浏览器
+ `OpreaMobile` / `op_mob` ：opera 移动浏览器
+ `QQAndroid` / `and_qq` ：QQ 安卓浏览器
+ `Samsung` ：三星浏览器
+ `Safari` ：桌面版本 Safari
+ `UCAndroid` / `and_uc` ：UC 安卓浏览器

