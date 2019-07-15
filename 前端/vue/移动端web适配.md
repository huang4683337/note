

[移动端 web 页面调试工具](https://www.qianduan.net/vconsole-open-source/)

[参考文章](https://juejin.im/post/5b6503dee51d45191e0d30d2#heading-24)

## rem.js

```javascript
const baseSize = 32
// 设置 rem 函数
function setRem () {
  // 当前页面宽度相对于 750 宽的缩放比例，可根据自己需要修改。
  const scale = document.documentElement.clientWidth / 750
  // 设置页面根节点字体大小
  document.documentElement.style.fontSize = (baseSize * Math.min(scale, 2)) + 'px'
}
// 初始化
setRem()
// 改变窗口大小时重新设置 rem
window.onresize = function () {
  setRem()
}
```



### index.html

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width,initial-scale=1.0">
    <!-- 屏幕大小适应手机端-->
    <meta name="viewport" content="width=device-width, initial-scale=1, user-scalable=no">
    <!-- 删除苹果默认的工具栏和菜单栏 -->
    <meta name="apple-mobile-web-app-capable" content="yes">
    <!-- 设置苹果工具栏颜色，可自定义 -->
    <meta name="apple-mobile-web-app-status-bar-style" content="black">
    <!-- 忽略页面中的数字识别为电话，忽略email识别 -->
    <meta name="format-detection" content="telphone=no, email=no">
    <!-- uc强制竖屏 -->
    <meta name="screen-orientation" content="portrait">
    <!-- QQ强制竖屏 -->
    <meta name="x5-orientation" content="portrait">
    <!-- UC强制全屏 -->
    <meta name="full-screen" content="yes">
    <!-- QQ强制全屏 -->
    <meta name="x5-fullscreen" content="true">
    <!-- UC应用模式 -->
    <meta name="browsermode" content="application">
    <!-- QQ应用模式 -->
    <meta name="x5-page-mode" content="app">
    <!-- windows phone 点击无高光 -->
    <meta name="msapplication-tap-highlight" content="no">
    <!-- 禁用夜间模式，enable为可用 -->
    <meta name="nightmode" content="disable">
    <!-- 开启强制开启图片加载，即禁止部分浏览器的省流量模式（最好不要开启。。。） -->
    <meta name="imagemode" content="force">
    <link rel="shortcut icon" href="./static/images/logo11.png">
    <title>易懂</title>
  </head>
  <body>
    <div id="app"></div>
    <!-- built files will be auto injected -->
  </body>
</html>

```

