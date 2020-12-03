## 说明

vue初始化部分：数据代理  模板解析  

vue更新部分：数据绑定

[vue的一个简单实现](github.com/DMQ/mvvm)



## 准备

```js
Array.prototype.split.call(li); // 将伪数组转为真实数组

node.nodeType	// 获得节点类型 元素节点1、属性节点2、文本节点3

Object.definedProperty(obj,properName,{});	// 给对象添加属性（指定描述）

Object.keys(obj); // 得到对象自身可枚举属性组成的数组

obj.hasOwnProperty(prop);  // 判断 prop 是否是 obj 自身的属性

DocumentFragment  // 文档碎片 （高效批量更新多个节点）,不会跟页面进行关联，改变文档碎片中元素不会改变页面
```



## DocumentFragment

```html
<ul id='ul'>
	<li>1<li/>  
  <li>2<li/> 
  <li>3<li/> 
</ul>
```

```js
// 不会跟页面进行关联，改变文档碎片中元素不会改变页面，可以批量更新
// document 中一次元改变素就会进行一次页面渲染（回流）

const ul = document.getElementById('ul');

// 1- 创建文档碎片
const fragment = document.createDocumentFragment();

// 2- 取出 ul 中的 li
while (ul.firstChild) {     // 如果ul下的第一个子节点存在
    fragment.appendChild(ul.firstChild);    
    // 因为每个节点都只能存在一个父节点，那么将节点插入fragment中后，ul中对应的第一个子节点就会消失
}

// 3- 更新 fragment 节点中的 li
let arr = Array.prototype.slice.call(fragment.childNodes);  // 伪数转数组
arr.forEach(node => {
    if(node.nodeType === 1){ // 如果节点为元素节点
        node.textContent = '哈哈';
    }
});

// 4- 将更新后的节点插入 ul
ul.appendChild(fragment);
```



## 数据代理

> 1、数据代理就是 通过一个对象代理另一个对象中的属性操作（读/写）
>
> 2、vue中的数据代理 是通过 vm 代替 data 中所有属性的操作
>
> 3、当你 读 / 写时，会自动触发执行 get / set 函数
>
> 4、实现了 vm.data.xx  => vm.xx

vue数据代理实现流程：

+ 通过 `Object.definedProperty()` 给 vm 添加 与 data对象的属性对应的属性描述符

  ```js
  /**
   * Object.definedProperty(targetObj,prop,propDescribe)
   * targetObj: 目标对象
   * prop: 在目标对象上添加或者修改的属性名
   * 对属性名添加的描述
   */
  var obj = {};
  Object.defineProperty(obj,"name",{
      value: '我是name的属性描述value, 给name属性一个值',
      configurable: false,     // 决定当前属性是否可以被删除,或者重新设置描述
      enumerable: false,      // 决定当前属性是否能被枚举
      writable: false,        // 决定当前属性是否能被修改
  })
  console.log(obj);	// {name: "我是name的属性描述value, 给name属性一个值"}
  ```

+ 所有添加的属性都包含 getter / setter

+ getter / setter 去操作 data中对应的属性数据



### 案例演示

```js
function Vue(obj){
    this.obj = obj;
}
var vm = new Vue({name:'名字'});

console.log(vm.obj.name); // 正常我们使用实例中的数据时 应该 vm.obj.name
```



**实现代理**

```js
let vm = {};
let data = {
    name: '名字',
    other: {
        age: 18
    }
}

// 得到 data 中的每个 key
Object.keys(data).forEach(key => {
    defineReactive(key)
})

// 将 data 中的 key 代理到 vm
function defineReactive(key) {
    Object.defineProperty(vm, key, {
        configurable: false, // 是否可以重新定义
        enumerable: true,   // 是否可以枚举
        // 通过 vm.xxx 读取属性值时调用，从data中获取对应的属性值返回，代理读取操作
        get: function proxyGetter() {
            return data[key];
        },
        // 通过 vm.xxx = value 时，value将被保存到data对应到属性上， 代理写入操作
        set: function proxySetter(newVal) {
            data[key] = newVal;
        }
    });
}


console.log(data)
console.log(vm)


vm.name= 'ss';
console.log(data)
console.log(vm)
```



## 模板解析

1、将 el 下所有的子节点取出放入创建的文档碎片（DocumentFragment）中

2、对 fragment 中所有层次的子节点递归进行编译处理

+ 对 {{}} 文本表达式进行解析
+ 对元素节点的指令属性进行解析
  * 对事件指令解析
  * 一般指令解析



### {{}} 表达式解析

1、得到满足正则匹配的 文本节点

2、从 data 中取出表达式所对应的属性值

3、将属性值设置为文本节点的 textContent



### 事件指令解析

1、从指令中取出事件名

2、根据表达式的值从 methods 中得到对应的事件处理函数对象

3、给当前元素节点半丁指令事件名和对调函数的 dom 事件监听，并将回调函数 this 指向 vm

4、指令解析完成后，移除此指令属性



### 一般指令解析

1、得到指令名和指令值  text / html / class

2、从 data 中根据表达式得到对应的值

3、根据指令名确定需要操作的元素节点是什么属性

+ v-text => textContent 属性
+ v-html => innerHTML 属性
+ v-class => className 属性

4、将得到的表达式的值设置到对应的属性上

5、移除元素的指令属性



## 数据绑定

一旦更改 data 中的某个属性值，页面上相关的值就会自动发生改变

### 数据劫持

1、数据劫持是 vue 中用来实现数据绑定的一种技术

2、基本思想：通过 Object.definePrototy() 来监听 data 中所有属性的变化，一旦数据变化就去更新界面。



### Observer 观察者

```js
// 用于建立 watcher 和 dep 之间的关系

/**
 * 
 * @param {*} data  // vue 中的 data 属性
 * @param {*} vm    // vue 实例  
 */
function _proxyData(data, vm) {
    Object.keys(data).forEach(key => {
        this.defineReactive(vm, key)
    })
}

Observer.prototype = {
    defineReactive: function(data,){
        Observer(data[key]);	// 递归所有属性
        Object.defineProperty(vm, key, {
        	get: function gett() {
            	return data[key];  // return 后结果相当于 => get: data[key]
          	},
           	set: function sett(val) {
           		return data[key] = val;
           	}
       	})
    }
}

```



### dep 订阅者

+ dep 什么时候创建

  初始化时，对 data 进行数据劫持前一步创建的。

  ```js
  var dep = new Dep();
  Object.defineProperty();
  ```

+ dep 实例的个数

  一个 dep 对应 data 中的一个属性（对 data 中的所有属性进行一一对应）

  ```js
  data:{
  	name: "名字",	  			=> name 会产生一个 dep
  	a:{						 => a 会产生一个 dep
  		a1:"a1",			 => a1 会产生一个 dep
  		b1:"b1"				 => b1 会产生一个 dep
  	}
  }
  ```

+ dep 的结构

  ```js
  /*
  id：当前 dep 的标识
  subs: 
  	存放每个表达式对应的 watcher	  => 因为同一个属性可能在多个表达式中使用，一个表达式对应一个 watcher;
  	<div>{{name}}</div>			 => div 中的 {{name}} 会产生一个 watcher
  	<span>{{name}}</span>		 => span 中的 {{name}} 会产生一个 watcher
  */
  ```

+ 实现一个 dep

  ```js
  
  var id = 0;
  
  function Dep() {
  	this.id = id++;
    this.subs = [];
  }
  
  
  Dep.prototype = {
    
    	// 将 watcher 添加到 dep
      addWatcher(watcher) {
          this.subs.push(watcher);
      },
  
    	// 遍历所有的 watcher， 通知 watcher 更新
      notify:function() {
          this.subs.forEach(function (sub) {
              sub.update();
          })
      },
  
    	// 建立 dep 和 watcher 关系
    	setMessage: function(){
       	// 调用 watcher 的添加方法
    		Dep.target.addDep(this);
  	}
  }
  
  Dep.target = null;
  ```



### watcher

+ wacher 什么时候创建

  初始化(模板解析) 花括号({{}}) 和 一般指令(v-xx)时创建了 watcher

+ wacher 的个数

  与模板中表达式（ {xx}, v-xx ）的数量一一对应（事件指令除外）

  ```js
  /*
  	<div>{{name}}</div>		=>	div 中的 {{name}} 会产生一个 watcher
  	<span>{{name}}</span>	=>	span 中的 {{name}} 会产生一个 watcher
  */
  ```

+ wacher 的结构

  ```js
  /*
  cb: 用于更新页面的回调函数;
  vm:	vue 实例;
  exp: 当前 watcher 对应的表达式;
  depIds: 
  	存放当前表达式对应的 data 中的属性所对应的 dep => {dipId:dep}
  	<div>{{name}}</div>		=> 	div 中的表达式 {{name}} 会产生一个 watcher, data 中的 name 属性对应的 dep 会存放在 这个watcher 中;
  value: 当前表达式对应的 value;
  */
  ```

+ 实现一个 watcher

  ```js
  function Watcher(vm, expOrFn, cb){
      this.cb = cb;	// 更新页面的回调
    	this.vm = vm;
      this.expOrFn = expOrFn;	// 当前 watcher 对应的 data 中的属性
  }
  
  Watcher.prototype= {
      constructor: Watcher;
      relationDeP: function(){
          Dep.target = this;	// 将当前 watcher 存在一个全局变量中，将 watcher 添加到 dep 时使用
          let value = this.vm.data[this.expOrFn];	// 强制触发 getter => getter 中有 dep 
          Dep.target = null;	// 和 dep 建立关系后清空
      }
  }
  ```
  
  

### 总结

+ 数据改变时

  ```js
  vm.name=1 => data 中的 name 值发生改变 => name 对应的 set() 调用 => name 属性对应的 dep => dep 去调用 name 属性对应的所有的 watcher => 每个 watcheer 调用自己的 update 渲染当前 watcher 对应的表达式
  ```

+ dep 和 watcher 之间关系

  ```js
  多对多的关系
  	一个 data 中的属性对应一个 dep => 因为一个属性可以被使用多次，所以一个 dep 可以对应多个 watcher
  	一个表达式对应一个 watcher => 一个表达式可能使用了多个属性，所以一个 watcher 对应多个 dep
    
  例如: 
  	{{a.b.c}} 一个表达式用了a、b、c三个属性;
  	{{a.b.c}} 表达式对应一个 watcher; a、b、c 三个属性对应三个 dep。所以一个 watcher 可以对应多个watcher
  ```

+ dep 和 watcher 之间关系怎么建立的

  在 data 属性中的 get() 中建立的

  ```js
  /*
  创建 watcher 之后，将 watcher 存入一个全局变量 Dep.target 中，调用属性的 getter 。
  
  属性的 get 中调用当前属性的 dep 的添加方法，将dep 添加到 watcher 中。
  */
  
  /*
  watcher
  */
  Dep.target = this;	// 将 watcher 存入全局变量中; this 是当前 watcher 实例
  在模板解析时，需要将模板中的属性换成 data 中属性对应的值, 此时就会触发属性对应的 getter	// 调用当前表达式所用属性的 getter
  
  addDep: function(dep){
    // this.depIds[dep.id] = dep;	// 将 dep 添加到 watcher
    dep.addWatcher(this);	// 将 watcher 添加到 dep
  }
  
  /*
  Observer
  */
  var dep = new Dep()
  Object.defineProperty(data, key, {
    get: function(){
      // 调用 dep 的添加方法
      if(Dep.target){ dep.setMessage() }
    }
  }
  
  /*
  dep
  */
  // 建立 dep 和 watcher 关系
  setMessage: function(){
    // 调用 watcher 的添加方法
    Dep.target.addDep(this);
  }
  
  // 将 watcher 添加到 dep
  addWatcher: function(watcher){
    this.subs.push(watcher);
  }
  ```

+ dep 和 watcher 之间关系什么时候建立的

  初始化时解析模板中的表达式会创建一个 watcher 对象时

  ```js
  因为vue首先会对数据进行数据劫持，添加 getter 、 setter; 然后才会进行模板解析。
  数据劫持的时候 data 中的每个属性对应一个 dep, 此时还没有进行模板解析, 还没有产生 watcher。
  模板解析时, 每个表达式对应一个 watcher , 此时 dep 、watcher 都存在了。
  所以会在创建 watcher 是建立两者之间的关系。
  ```

  

## 实现一个Vue

```js
function Vue(options) {
    this.$options || {};
    var data = this._data = options.data;

    // 实现数据代理
    // ...

    // 创建一个观察者
    new Observer(data, this);
}
```

