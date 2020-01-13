## vue 自定义滚动条

在 `Element-UI` 中存在一个标签`<el-scrollbar>`  不知什么原因 在文档中并没有对这个进行体现。



## 使用方式

```html
<div class="div">
    <el-scrollbar style="height:100%">
      <ul>
        <li></li>
      </ul>
    </el-scrollbar>
</div>
```

```css
.css{
  height:200px;
  overflow:hidden;
}
ul{
  height:400px;
}
```

## 属性介绍



```js
{
  native:Boolean, // true时不会显示模拟的滚动条，false时显示vue模拟的滚动条
  wrapStyle:{},	// 外层盒子样式
  wrapClass:{}, // 外层盒子的class
  viewClass:{},	// 内层盒子的class
  viewStyle:{},	// 内层盒子的样式
  tag: 'ul',	// 可以指定模拟出来的是什么标签
}
```

![内外盒子图](/Users/mrhuang/Downloads/笔记图片/el-scrollbar.png)