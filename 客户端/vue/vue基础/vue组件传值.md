

## 父传子

```js
/* 父组件通过属性传值 子组件通过 props来接收参数 */

// 父组件：
	<bbb :item="item"></bbb>
  data(){
    item:'111',
  }

  
// 子组件：
	<div> {{item}} </div>
	props:['item']  
	// 或者用对象接收来规定接收的类型和未接收到传值时的默认值 porps:{ item:{type:String,default:null} }
```



## 子传父

+ **传事件**

  父组件给子组件传递一个事件，子组件通过 this.$emit 来触发这个事件

  ```js
  // 父组件：
  	<bbb @inChild='parents'></bbb>
  	parents(val){
    	console.log(val);
    }
  
  
  // 子组件：
  	<div @click='bbb'>3131</div>
    bbb(){
        this.$emit('inChild',{bb:'我是子组件'}); //this.$emit('父组件传来的事件',需要给父的值)
    }
  ```

+ **this.$parent**

  直接在子组件中通过 this.$parent 调用父组件方法 

  ```js
  // 父组件：
  	parents(val){
    	console.log(val);
    }
  
  
  // 子组件：
  	<div @click='bbb'>3131</div>
    bbb(){
        this.$parent.parents('我是子组件');
    }
  ```

+ **传方法  props 接收方法**

  父组件将自己的某个方法当作属性传给子组件， 子组件去调用

  ```js
  // 父组件：
  <bbb :inChild="parents"></bbb>
  parents(val){
    console.log(val);
  }
  
  
  // 子组件：
  <div @click='handleClick'>3131</div>
  props:['inchild']
  handleClick(){
    if(this.inchild){
      this.inchild('我是子组件'); 
    }
  }
  ```

  

## 父组件调用子组件方法

通过  `ref`  来实现   `this.$refs` 来调用

```js
// 父组件
	<button @click="aaa">点击</button>
	<bbb ref="child1"></bbb>
	aaa(val){
    this.$refs.child1.child();
	}


// 子组件：
  child() {
  	console.log('我是子组件');
  }
```



## 兄弟组件传值

**1、创建 Bus.js 文件**

```js
//Bus.js文件一般都是放在assets文件下面
import Vue from 'vue';
export default new Vue;
```



**2、组件1中**

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



**3、组件2中**

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



## 解决子组件无法修改父组件传来的值

在 vue 中遵循单向数据流的方式，所以规定无法在子组件中修改父组件传来的值。

可以通过 `.sync` 实现在子组件中修改父组件中的属性。

```js
// 父组件中
{{value}}
<bbb :inChild.sync="value"></bbb>
data(){
    return{
        value: '父组件啊'
    }
}


// 子组件中
<div @click="handleClick"><>
props:['inChild'],
handleClick() {
    this.$emit('update:inChild', '我在子组件修改父组件传过来的的值')
}

```



## 父子组件使用 v-model 实现组件通信

### 原理

提到 `v-model` 就能联想到使用表单时的双向绑定。

```js
{{msg}}
<input type="text" v-model="msg">
 
data(){
  return {
    msg: '我是 input'
  }
}
```

实现原理如下:

```js
/**
核心 :valeu @input
*/

/*
input: 在值发生变化时触发事件，也就是原生 js 的 oninput 事件;
当 input 中的值发生变化时, 将 input 的值赋给 msg 就实现了双向绑定
*/

{{msg}}
<input type="text" :value="msg" @input="msg = $event.target.value">
  
data(){
  return {
    msg: '我是 input'
  }
}
```

假设把 input 当作一个组件来看, input 的值对应的就是 value 属性, 通过触发事件 @input 来实现数据的实时更新。

这种方式类似于父子组件通过事件通信的方式。

```js
// 父组件：
	{{msg}}
  <childB  :value="msg" @input="parent" ></childB>	

	data(){
    return{
      msg: '我是 input',
    }
  }
	parent(val){
  	this.msg = val;
	}


// 子组件：
	<span @click='handleClick'>dianji</span>
	props:["value"],
	handleClick(){
		this.$emit("input", this.value+1);
  }
```



### 利用 v-model 实现数据通信

**封装组件用来通信是个合适的选择**

==注意：子组件接受参数只能通过 `value` 接收。`$emit`触发的事件只能是 input==

```js
// 父组件：
	{{msg}}
  <childB  v-model="msg" ></childB>	

	data(){
    return{
      msg: '我是 input',
    }
  }


// 子组件：
	<span @click='handleClick'>dianji</span>
	props:["value"],
	handleClick(){
		this.$emit("input", this.value+1);
  }
```



**以上方式实现了通过 v-model 实现了父子组件通信，但是限制了`props`只能接收的属性名为`value`，`$emit`触发是事件只能是`input`；这样在使用表单的时候可能会引发冲突**

**为了避免上述问题，可以在子组件使用`model`属性**

```js
// 父组件：
	{{msg}}
  <childB  v-model="msg" ></childB>	

	data(){
    return{
      msg: '我是 input',
    }
  }


// 子组件：
	<span @click='handleClick'>dianji</span>

	model:{
    prop: 'info', // v-model 所绑定的属性，默认是 value
    event: 'changeModel',  // v-model 绑定的事件，默认是 input
  },

	props:["info"],	// props 接收 v-model 绑定的属性
	handleClick(){
		this.$emit("changeModel", this.info+1);
  }
```



##  非属性特性

非属性特性： 非 `prop` 特性，父组件将值传进来，但是子组件没有通过 `props` 来接收，子组件还想使用某些属性。

+ `$attrs` 爷孙之间的属性传递，并且孙子不需要通过 `props` 接收

  ```html
  <!-- 父组件 -->
  <child parent:'父组件传来的'></child>
  
  <!-- 子组件中：没有通过 proprs 接收，直接通过 $attrs 使用父组件传来的数据 -->
  <p>{{$attrs.parent}}</p>
  ```

  在封装大型组件时，会出现爷孙之间的传值`<A> <B> <C></C>  </B> </A>`

  ```html
  <!-- 
  组件 C 中需要使用组件 A 中的某些属性，
  但是组件 B 不需要
  -->
  
  <!-- 一般做法：逐层传递 -->
  <A :a="" :b=”"...> 
  	<B :a="" :b=”"...> 
  		<C></C> 
    </B> 
  </A>
  
  <!-- 
  爷爷传值，父亲转发，孙子使用
  使用 $attrs, 通过 v-bind 展开 $attrs， 类似于ES6的... 
  -->
  <A :a="" :b=”"...> 
  	<B v-bind="$attrs"> 
  		<C></C> 
    </B> 
  </A>
  ```

+ `$listeners` 爷孙之间事件传递

  ```html
  <!-- 爷爷组件 -->
  <father @hello="onHello"></father>
  
  onHello() {
    console.log('来自孙子的问候');  
  }
  ```

  ```html
  <!-- 父亲组件 -->
  <!-- v-on="$listeners" 将事件传给儿子 -->
  <Grandson v-bind="$attrs" v-on="$listeners"></Grandson>
  ```

  ```html
  <!-- 孙子组件 -->
  <!-- 直接使用、触发爷爷的方法-->
  <h3 @click="$emit('hello')">grandson</h3>
  ```

  

### provide/inject

实现祖先和后代之间的通信, `provide/inject`绑定是不可响应的，除非两者之间绑定的值是通过`vue`实现了数据绑定的，比如：`data`中的值 

[provide/inject](https://cn.vuejs.org/v2/api/#provide-inject)

+ `provide` 祖先中注入

  ```js
  export default {
    provide() {
      return {
        bar: 'barrrrrr',
        fn: this.fn
      }
    }
    methods:{
    	fn(){
    		console.log("provide/inject");
  		}
  	}
  }
  ```

+ `inject` 后代接收

  ```html
  <h3 @click="emitFn">provide/inject</h3>
  ```

  ```js
  inject: {
    bar2: {
      from: "bar",
      default: "",
    },
    fn: {
      from: "fn",
      default: "",
    },
  },
    
  methods: {
    emitFn() {
      console.log(this.bar2);	// barrrrrr
      this.fn();	// provide/inject
    },
  },
  ```

  