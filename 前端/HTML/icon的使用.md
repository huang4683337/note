## symbol

**优势：**

- 支持多色图标了，不再受单色限制。
- 支持像字体那样通过font-size,color来调整样式。
- 支持 ie9+
- 可利用CSS实现动画。
- 减少HTTP请求。
- 矢量，缩放不失真
- 可以很精细的控制SVG图标的每一部分

### 使用方法

```shell
# 引入 iconfont.js

# 加入通用css代码（引入一次就行）：
<style type="text/css">
    .icon {
       width: 1em; height: 1em;
       vertical-align: -0.15em;
       fill: currentColor;
       overflow: hidden;
    }
</style>

# 使用
<svg class="icon" aria-hidden="true">
    <use xlink:href="#icon-xxx"></use>
</svg>
```

### 在vue中使用

+ 在 src/assets下新建 font文件夹
+ 在阿里矢量图标库中下载 symbol ==> 点击下载到本地
+ 将下载好的文件放入 font 文件夹中
+ 在 main.js 中

```javascript
 import './assets/font/iconfont';
```

+ 在 app.vue 中

```css
<style type="text/css">
    .icon {
       width: 1em; height: 1em;
       vertical-align: -0.15em;
       fill: currentColor;
       overflow: hidden;
    }
</style>
```

+ 直接使用

```css
<svg class="icon" aria-hidden="true">
    <use xlink:href="#icon-xxx"></use>
</svg>
```



## symbol在vue中组件的封装





## font-class

```html
<!-- 在阿里矢量库拷贝项目下面生成的fontclass代码：
在前面加上 http:  可在浏览器观看
http://at.alicdn.com/t/font_405597_5zg7jxvurr3.css
然后将页面内容粘贴到 app.vue
-->

<!-- 在app.vue中 @import './assets/font/iconfont.css'; -->

<!-- 使用 -->
<i class="iconfont icon-xxx"></i>
```



## unicode

==vue使用时放入 aap.vue中即可==

**引入自定义字体 `font-face**

```css
 @font-face {
   font-family: "iconfont";
   src: url('iconfont.eot'); /* IE9*/
   src: url('iconfont.eot#iefix') format('embedded-opentype'), /* IE6-IE8 */
   url('iconfont.woff') format('woff'), /* chrome, firefox */
   url('iconfont.ttf') format('truetype'), /* chrome, firefox, opera, Safari, Android, iOS 4.2+*/
   url('iconfont.svg#iconfont') format('svg'); /* iOS 4.1- */
 }
```

**定义使用iconfont的样式**

```css
.iconfont {
  font-family:"iconfont" !important;
  font-size:16px;
  font-style:normal;
  -webkit-font-smoothing: antialiased;
  -webkit-text-stroke-width: 0.2px;
  -moz-osx-font-smoothing: grayscale;
}
```

**使用**

```html
<i class="iconfont">&#xe604;</i>
```

