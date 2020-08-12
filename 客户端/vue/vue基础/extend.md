创建 `testComponent.js`

```js
import Vue from 'vue'

export default function fn(node) {
    const testComponent = Vue.extend({
        template: `<div>{{ text }}</div>`,
        data: function () {
            return {
                text: 'extend test'
            }
        }
    })
    new testComponent().$mount(`#${node}`)
}

```

在 `vue` 文件中使用

```html
<div id="hello"></div>

import testCompontent from "./../testComponent";

mounted() {
	testCompontent('hello');
},
```





注意：

```js
/*
new testComponent().$mount(`#${node}`)
会用 extend 中生成的模板替换掉 <div id="hello"></div>
*/
```



解决方案

```js
// 第一种：在模板中添加对应的 id
template: `<div id=${node}>{{ text }}</div>`
new testComponent().$mount(`#${node}`)


// 第二种：将生成的模板插入到对应的 id 下
const extendComponent = new testComponent().$mount()   
document.querySelector(`#${node}`).appendChild(extendComponent.$el)
```





实现一个demo

```js
import Vue from 'vue'

function Fn() {
    this.extendComponent = '';
}

Fn.prototype.testCompontent = function (node) {
    const testComponent = Vue.extend({
        template: `<div>{{ text }}</div>`,
        data: function () {
            return {
                text: 'extend test'
            }
        }
    })
    // new testComponent().$mount(`#${node}`)
    this.extendComponent = new testComponent().$mount(`#${node}`)
}


let fn = new Fn();

export default fn;


// html
<template>
  <div>
    <div id="hello"></div>
    <div @click="btn">ssfsfsf</div>
  </div>
</template>

<script>
import fn from "./../testComponent";
export default {
  name: "HelloWorld",
  data() {
    return {};
  },
  methods: {
    btn() {
      fn.extendComponent.text = "111111";
    },
  },
  mounted() {
    fn.testCompontent("hello");
    console.log(fn.extendComponent.text);
  },
};
</script>

```

