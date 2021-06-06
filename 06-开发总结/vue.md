## vue - H5 中长按显示弹出层

```js
// 绑定 touchstart touchend 事件
<div v-for="(item,index) in list" class="item_info" @touchstart="touchStart($event,item)"  @touchend="touchEnd($event,item)">
```

```js
// touchStart 事件中添加一个定时器
// 长按 3 秒时显示弹出层
// 如果不够 3 秒触发了 touchEnd, 就会清空定时器，就不会执行定时器中的方法, 



private timeout:any = null;

// 手指按下
private touchStart(e,item){
  clearTimeout(this.timeout); //再次清空定时器，防止重复注册定时器
  e.preventDefault()
  this.timeout = setTimeout(() => {
    this.itemInfo = item;
    this.isShow = true;
  },3000)
  return false;
}

// 手指离开
private touchEnd(e,item){
  clearTimeout(this.timeout);
  e.preventDefault()
  console.log('手指离开');
  return false;
}
```



```js
// vue 中阻止默认事件除了 e.preventDefault()
// 还可以 .prevent
@touchstart.prevent="touchStart($event,item)"  
@touchend.prevent="touchEnd($event,item)"
```



## vue 中 @touchstart 、 @touchend、@click 冲突问题 

```js
// 使用 @touchend.prevent 代替 @click
// 通过 e.target.className.indexOf( 'checked_box') !== -1 判断点击的是哪一个
```







## Vue 如何在事件中将 Event 传入

```js
<div v-for="(item,index) in list" @click="goDetail($event,item)">

goDetail(e, item){
  // e 就是事件
}
```





## vue 图片动态路径不生效

```html
 <img :src="require(`@/assets/images/list/${key.icon}`)"  alt=""  srcset="" />
```



## 手写复选框

```less
// 复选框
.checked_box {
  display: inline-block;
  position: relative;
  border: 1px solid #c2c2c2;
  border-radius: 2px;
  box-sizing: border-box;
  width: 20px;
  height: 20px;
  background-color: #fff;
  z-index: 1;
  transition: border-color 0.25s cubic-bezier(0.71, -0.46, 0.29, 1.46),
    background-color 0.25s cubic-bezier(0.71, -0.46, 0.29, 1.46);
  top: 15px;
  left: 3px;
  box-shadow: 0px 1px 12px 0px rgba(0, 0, 0, 0.15);
}
.checked_box::after {
  box-sizing: content-box;
  content: '';
  border: 1px solid #fff;
  border-left: 0;
  border-top: 0;
  height: 10px;
  left: 6px;
  position: absolute;
  top: 3px;
  transform: rotate(45deg) scaleY(0);
  width: 4px;
  transition: transform 0.15s ease-in 0.05s;
  transform-origin: center;
}

// 选中效果
.ischecked {
  background-color: #409eff;
}
.ischecked::after {
  transform: rotate(45deg) scaleY(1);
}
```



## 服务端请求JSON

**场景：**开发过程中有些需求是通过动态配置 JOSN 文件来实现某种功能，但是在打包之后 JSON 文件内容会被编译到引用的文件中，这样在服务端修改可配置的 JSON 是不生效的。

**解决方案**：通过接口请求，获取 JSON文件的内容，这样在服务器上修改后，是可以生效的

```js
// 配置面板地址，打包后可在服务上修改相关配置文件生效

let isProd = process.env.NODE_ENV !== 'development'

let isDefaultPanel = []
let isPanelInitData = {}
let isAllPanel = {}


function ajax({ url, url_ENV, success }) {
    let xmlHttp = new XMLHttpRequest();
    xmlHttp.onreadystatechange = function () {
        if (xmlHttp.readyState === 4 && xmlHttp.status === 200) {
            success(JSON.parse(xmlHttp.responseText))
        }
    }

    xmlHttp.open('get', isProd ? url : process.env.baseurl + url_ENV, false)
    xmlHttp.send()
}

ajax({
    url: 'static/assets/moduleConfig/defaultPanel.json',
    url_ENV: 'assets/moduleConfig/defaultPanel.json',
    success: (res) => {
        isDefaultPanel = res;
    }
})

ajax({
    url: 'static/assets/moduleConfig/panelInitData.json',
    url_ENV: 'assets/moduleConfig/panelInitData.json',
    success: (res) => {
        isPanelInitData = res;
    }
})

ajax({
    url: 'static/assets/moduleConfig/allPanel.json',
    url_ENV: 'assets/moduleConfig/allPanel.json',
    success: (res) => {
        isAllPanel = res;
    }
})

export { isDefaultPanel, isPanelInitData, isAllPanel }
```





## vue 中刷新页面时保存store中的数据

+ 获取到 store 的数据时，直接存在 `sessionStorage` 中，那么这样毫无意义，我们为什么不直接将数据存入到`sessionStorage` 中呢。
+ 通过一个方法监听浏览器的刷新，当浏览器刷新时，我们将需要的数据存入到 `sessionStorage` 中。



**beforeunload**  监听页面刷新时，将vuex中需要存储的数据存入到 `sessionStorage` 中

因为 `sessionStorage`  在页面关闭后就会清空，所以选择它。

```js
private created() {
  window.addEventListener('beforeunload', () => {
    sessionStorage.setItem(
      'detailInfo',
      JSON.stringify(this.$store.state.qucikForm.getDetailParams),
    );
  });
}
```



## vue组件双向绑定 v-model 原理

+ `:value`

+ `@input`

```vue
// 通过 :value 将值传入到 input
// 通过 @input 来改变 value 的值
```

```vue
// 这也可以用于组件的双向绑定
```





## vue使用slot时，子组件如何调用父组件方法

```vue
// parent
<div>
	<slot></slot>
</div>

// child
this.$parent.$on('父组件方法名')
```

