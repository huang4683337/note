## 使用 vuter、beautify 格式化 vue

```js
vscode --> 首选项 --> 设置 --> 有上角文本形式的按钮
```



```json
{
    // 缩紧为4空格
    "vetur.format.options.tabSize": 4,
    //.vue文件template格式化支持，并使用js-beautify-html插件
    "vetur.format.defaultFormatter.html": "js-beautify-html",
    //js-beautify-html格式化配置，属性强制换行
    "vetur.format.defaultFormatterOptions": {
        "js-beautify-html": {
            // "wrap_attributes": "force-aligned",
            "wrap_attributes": "force-expand-multiline",
        },
        "prettier": {
            "semi": true, // 末尾添加分号
            "singleQuote": true, // js 单引号
        }
    },
    //根据文件后缀名定义vue文件类型
    "files.associations": {
        "*.vue": "vue"
    },
}
```

