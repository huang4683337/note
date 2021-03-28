## css 实现可配置换行

```html
<div class="div1">
    <span>131313113</span>
    <span>131313113</span>
    <span>131313113</span>
</div>

<!-- 通过p标签实现可配置换行 -->
<div class="div2">
    <span>131313113</span>
    <p></p>
    <span>131313113</span>
    <p></p>
    <span>131313113</span>
</div>
```

```css
* {
    margin: 0;
    padding: 0;
}

.div1 {
    background-color: green;
}

.div2 {
    margin-top: 40px;
    background-color: yellow;
}

span {
    margin: 10px 0;
    background-color: red;
}

p{
    height: 10px;
}
```

