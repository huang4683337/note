# template

**定义一个组件 a.wxml**

```html
<!-- 定义时需要一个 name 属性作为唯一标识别 -->

<template name="postItem">
  <text>{{cont}}</text>
  <block wx:for="{{list}}" wx:key="key{{index}}">
    <text>索引是：{{index}}</text>
    <text>内容是：{{item.sum}}</text>
  </block>
</template>

```

**在 b.wxml 中使用组件 a.wxml**

```html
<!-- 通过 import 标签引入组件 a.weml -->
<import src="a.wxml" />

<!-- 通过 template 标签使用组件，is属性表明使用的是哪个组件， data属性是将值传给子组件 -->
<view>
  <template is="postItem" data="{{cont, list}}"></template>
</view>
```

```css
/* 通过 @import 引入组件对应的样式 */
@import "a.wxss"
```

