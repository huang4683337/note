## ElementUI-Message提醒框只显示一个

```js
this.$message.closeAll();
```



## ElementUI 删除表单提示语

```js
this.$refs.ruleForm['clearValidate']('name');

// <el-form ref="ruleForm">

// <el-form-item ref='name'>
```



## elementTable 多选禁用

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





## element dialog嵌套

```html
// 第二个 dialog 添加 append-to-body 属性
<el-dialog append-to-body ></el-dialog>
```





## Element-form 在 validate 中触发表单校验

```html
<el-form ref="form">
<el-form-item label="ucid" prop="ucid" ref="ucid_form_item">
    <el-input
        v-model.trim="form.ucid"
        placeholder="请输入ucid"
    ></el-input>
</el-form-item>
</el-form>
```

```js
data(){
  	return {
      	form: {
            ucid: "",
        },
        rules: {
        	ucid: [{ validator: VALIDATE_UCID, trigger: "blur" }],
    		},
      	ucid_err: "",
  	}
  
    const VALIDATE_UCID = (rule, value, callback) => {
        if (this.ucid_err) {
            callback(new Error(this.ucid_err));
            this.ucid_err = "";
        } else if (value === "") {
            callback(new Error("请输入ucid"));
        } else {
            callback();
        }
    };
}

methods:{
  fn(){
    this.$refs["form"].validate((valid) => {
      this.ucid_err = "ucid不存在，请检查";
			this.$refs["ucid_form_item"].onFieldBlur();
    })
  }
}
```

