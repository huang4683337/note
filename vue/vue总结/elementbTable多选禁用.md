```html
 <el-table-column
              type="selection"
              v-model="multipleSelection"
              :selectable='selectDisableRoom' 
            >
```

```js
// selectable 对应一个函数

   selectDisableRoom (row, index) {
      if(row.id !== 1){
        return true
      }
    },
```





## Element 行、列合并

https://juejin.cn/post/6844904017806491662

