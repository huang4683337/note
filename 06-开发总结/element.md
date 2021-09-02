## element-message 提醒框只显示一个

```js
this.$message.closeAll();
```



## element-form 删除表单提示语

```js
this.$refs.ruleForm['clearValidate']('name');

// <el-form ref="ruleForm">

// <el-form-item ref='name'>
```



## element-table 多选禁用

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





## Element-table 行、列合并

https://juejin.cn/post/6844904017806491662





## element-dialog 嵌套

```html
// 第二个 dialog 添加 append-to-body 属性
<el-dialog append-to-body ></el-dialog>
```





## Element-form 在接口返回信息后自动触发表单校验

**场景**

```
- 表单中包含 ucid、数量，两个输入框
- ucid、数量，不能为空
- 点击提交
	- ucid、数量，不为空时，校验 ucid 是否存在
	- 接口返回 ucid 不存在时，想要再次触发表单的校验提示。
	
- 通过 input 失焦事件再次触发表单的校验功能
```



**HTML**

```html
<!-- 表单元素 -->
<el-form ref="form">
	<el-form-item label="ucid" prop="ucid" ref="ucid_form_item">
    <el-input
        v-model.trim="form.ucid"
        placeholder="请输入ucid"
    ></el-input>
	</el-form-item>
</el-form>

<!--提交按钮-->
<el-button  type="danger" @click="submitFn" >  确定 </el-button>
```



**js**

```js
data(){
  	return {
      	form: {
            ucid: "",
        },
      	// 表单校验规则
        rules: {
        	ucid: [{ validator: VALIDATE_UCID, trigger: "blur" }],
    		},
      	// 接口校验
      	ucid_err: "",
  	}
  
  	// input 失去焦点时触发
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
```



```js
methods: {
    submitFn() {
        this.$refs['form'].validate((valid) => {
            if (valid) {
              	// 表单校验通过后，发起异步请求
                setTimeout(()=>{
                  
                  	// 保存响应异常信息
                    this.ucid_err = 'ucid不存在，请检查';
                  
                  	// 通过失焦事件触 ucid 的校验，给出相应的提示
                  	this.$refs['ucid_form_item'].onFieldBlur();
                })
            }
        });
    }
},
```



## el-cascader 同时获取 label、value 的值

### 1、元素配置

```html
<el-cascader
    ref="city"
    v-model="selected"
    :options="dataList"
    @change="handleChange"
    :props="{
        label: 'dictName',
        value: 'all',
        multiple: true,
        checkStrictly: false
    }"
>
```



### 2、处理 options

**初始 options 数据结构**

```js
[
    {
        dictId: "1",
        dictName: "北京",
        isEnable: true,
        itemSource: 0,
        children: [
            {
                children: null,
                dictId: "378",
                dictName: "东城",
                isEnable: true,
                itemSource: 0
            }
        ]
    }
];
```



**处理 options，在 options 中添加 all 字段**

```js
handleoptions(options) {
    return options.map((item) => {
        if (item.children) {
            this.handleoptions(item.children);
        }
        item.all = {
            label: item.dictName,
            value: item.dictId
        };
        return item;
    });
}
```



**处理结果为下**

```js
[
    {
        "dictId": "1",
        "dictName": "北京",
        "isEnable": true,
        "itemSource": 0,
        "children": [
            {
                "children": null,
                "dictId": "378",
                "dictName": "东城",
                "isEnable": true,
                "itemSource": 0,
                "all": {
                    "label": "东城",
                    "value": "378"
                }
            }
        ],
        "all": {
            "label": "北京",
            "value": "1"
        }
    }
]
```



### 3、el-cascader 的 props 中添加配置

```html
:props="{
    label: 'dictName',
    value: 'all',
}
```



### 4、点击获取结果

```js
console.log( this.selected );

// [[{"label":"北京","value":"1"},{"label":"东城","value":"378"}]]
```



## el-cascader 中获取 label 的值

```html
<el-cascader
    ref="city"
    v-model="invoceInfoData.selected"
    :options="dataList"
    @change="handleChange"
>
```

```js
handleChange() {
    const e = this.$refs.city.getCheckedNodes()[0];
    const label = e ? e.pathLabels : null;
  	console.log(label);
},
```

