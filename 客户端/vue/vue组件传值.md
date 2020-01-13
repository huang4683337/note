# vue组件传值



## 父传子

```js
/* 父组件通过属性传值 子组件通过 props来接收参数 */

父组件：
	<bbb :item="item"></bbb>
  data(){
    item:'111',
  }
  
子组件：
	<div> {{item}} </div>
	props:['item']  
	// 或者用对象接收来规定接收的类型和未接收到传值时的默认值 porps:{ item:{type:String,default:null} }
```



## 子传父

```js
1 -- 传事件
/* 父组件给子组件传递一个事件，子组件通过 this.$emit 来触发这个事件 */

父组件：
	<bbb @parent='parent'></bbb>
	parents(val){
  	console.log(val);
  }

子组件：
	<div @click='bbb'>3131</div>
  bbb(){
      this.$emit('parents',{bb:'我是子组件'}); //this.$emit('父组件传来的事件',需要给父的值)
  }


2 -- this.$parent
/* 直接在子组件中通过 this.$parent 调用父组件方法 */
父组件：
	parents(val){
  	console.log(val);
  }
子组件：
	<div @click='bbb'>3131</div>
  bbb(){
      this.$parent.parents('我是子组件');
  }


3 -- 传方法  props 接收方法
/* 父组件将自己的某个方法当作属性传给子组件， 子组件去调用 */
父组件：
	<bbb :aaa="aaa"></bbb>
	parents(val){
  	console.log(val);
  }
子组件：
	<div @click='bbb'>3131</div>
	props:['aaa']
  bbb(){
    if(this.aaa){
     	this.aaa('我是子组件'); 
    }
  }
```



## 父组件调用子组件方法

```js
/* 通过 ref 来实现   this.$refs来调用 */
父组件：
	<button @click="aaa">点击</button>
	<bbb ref="child1"></bbb>
	aaa(val){
    this.$refs.child1.child();
	}
子组件：
  child() {
  	console.log('我是子组件');
  }
```





## 无关系组件传值

### 创建 Bus.js 文件

```js
//Bus.js文件一般都是放在assets文件下面
import Vue from 'vue';
export default new Vue;
```

### 组件1中

```js
//点击按钮时，中央事件总线通过 $emit 发送一个send事件
<button @click='sendMsg'>传送信息给组件2</button>

import Bus from '@/assets/js/bus';

export default {
    methods:{
        sendMsg(){
            Bus.$emit('send','我是组件1');
        }
    }
}
```

### 组件2中

```js
//中央事件总线通过 $on 监听组件1中发送的send事件, 用一个回调函数接受传过来的参数

import Bus from '@/assets/js/bus';

export default {
    created(){
        Bus.$on('send',(val)=>{
            console.log(val);
        })
    }
}
```

