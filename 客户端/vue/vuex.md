# vuex

## 1、vuex执行流程

> 页面刚打开  `computed`  获取一次数据改变视图
>
> 用户操作视图 => 通过  `dispatn`  发出请求 发送给  `action`  => `Action`  通过 `store/context.commit`  向 `mutation`  提交一个异步请求 =>`mutation`  对数据进行处理 => 数据改 => `computed`   监听导数据改变   获取到数据 => 视图改变

![vuex1](/Users/mrhuang/Downloads/笔记图片/vuex1.png)

## 2、vuex 在项目中的应用

### 2-1、安装vuex

```shell
$ npm install vuex # 项目中安装vuex
```

### 2-2、使用store

#### 创建store

```javascript
//src文件下创建文件夹store，以及文件index.js  ==>路径为  src/store/index.js

import Vuex from "vuex"
import Vue from "vue"

Vue.use(Vuex)  //在vue中使用Vuex

const store = new Vuex.Store({

});

export default store;
```

#### 全局引入store

```javascript
// 全局使用store  (组件中通过this.$store访问store)

import store from './store' //在main.js中引入store模块
new Vue({
    store, //使用store，想要所有组件共享store就要 顶部加载(store放在所有组件的上面)
    router
})
```



### 2-3、store之state

```javascript
//store/index.js文件中

const store = new Vuex.Store({
    state:{
        listData:[1,2,3,4]
    }
});
```

```javascript
//组件中

/**
丢弃掉data属性;直接通过computed属性来监听store数据的变化；
通过this.$store.state来访问state中的属性，比如listData;
*/ 

computed: {
    listData () {
        return this.$store.state.listData;
    }
}
```

#### mapState

> 当一个组件需要获取多个状态时, 为了避免频繁的声明计算属性, 我们利用mapState帮我们自动生成计算属性; 我们一共有 四种 体现方式供您选择

```javascript
//组件中
import { mapState } from 'vuex'

//对象方法：
computed: mapState({
    // 箭头函数可使代码更简练 —>方法1
    count_data: state => state.count,

    // 传字符串参数 'count' 等同于 `state => state.count` —>在方法1的基础上更加简化的 方法2
    count_data: 'count',

    // 为了能够使用 `this` 获取局部状态，必须使用常规函数 —>方法3
    count_data (state) {
      return state.count + this.localCount
    },  
})

//数组方法:
computed: mapState([
  'count'
])
```

#### ...mapState

>如果我们组件内部也有自己的计算属性需要结合在store中拿到的属性, 
>
>我们就可以使用展开符将mapState生成的计算属性一个一个展开配合组件的属性来使用 

```javascript
computed: {
   count_addShi () {
    return this.count + 10;
   },
   ...mapState({
    count: "count"
   })
  }
 }
```

### 2-4、store之getter

> getter相当于是store的计算属性 ,getter返回的结果会根据对应的依赖缓存,
>
> 如果依赖不改变,那么就不会触发重新计算;
>
> 但是我们如果通过getter的方法访问时,就不会进行缓存，每次都会重新计算。 
>
> 例如：this.$store.getters.todoTrue()

#### getter

```javascript
//store/index.js文件中：

const store = new Vuex.Store({
  state: {
    todos:1
  },
  getters: {
    todoTrue: state => {
      return state.todos+10
  }
})
```

```javascript
//组件中：
 
// 在计算属性中通过 this.$store.getters.todoTrue 来访问getter中的属性
computed: {
  todoTrue() {
    return this.$store.getters.todoTrue
  },
},
```

#### ...mapGetters

```javascript
import { mapGetters } from ‘vuex'

computed: {
  // 使用对象展开运算符将 getter 混入 computed 对象中
    ...mapGetters([
      'doneTodosCount',
      'anotherGetter',
      // ...
    ])
  }

//如果你想将一个 getter 属性另取一个名字，使用对象形式：
mapGetters({
  // 把 `this.doneCount` 映射为 `this.$store.getters.doneTodosCount`
  doneCount: 'doneTodosCount'
})
```

### 2-5、store之Mutation

> 主要用于数据计算和处理，处理同步数据

>注意事项：
>
>1、Mutation 必须是同步函数
>
>2、最好提前在你的 store 中初始化好所有所需属性。
>
>3、当我们需要添加store中不存在的属性时
>
>​		1>、Vue.set(state.obj, 'name', '名字’);
>
>​		 2>、使用对象展开符替换掉原有的对象  state.obj = { ...state.obj, name: '名字’ }

```javascript
//store/index.js文件中：

const store = new Vuex.Store({
  state: {
    count: 0
  },
  mutations: {
    increment (state, n) {   //increment为mutations事件类型
      // 变更状态
      state.count += n;
      //state.count += n.amount; //组件使用对象方式提交时的处理
    }
  }
})
```

```javascript
//组件中：

<p @click='add'>增加</p>
{{count}}

methods:{
  add(){
    this.$store.commit('increment', 10);
    //this.$store.commit({ type: 'increment', amount: 10 });  //也可以使用对象的方式进行提交 
  }
},
computed: {
    ...mapState({
        count:'count'
    })
}
```

#### 使用常量替代 Mutation 事件类型

```javascript
//新建mutation_types.js文件 ==> store/mutation_types.js

export const SOME_MUTATION = 'SOME_MUTATION'
```

```javascript
//在store.js文件中

import Vue from "vue"
import Vuex from 'Vuex'
Vue.use(Vuex) //在vue中使用Vuex
import { SOME_MUTATION } from './mutation_types'

const store = new Vuex.Store({
  state: {
    count: 0
  },
  mutations: {
    // 我们可以使用 ES2015 风格的计算属性命名功能来使用一个常量作为函数名
    [SOME_MUTATION] (state, n) {
      state.count+=n;
    }
  }
})
export default store;
```

```javascript
// 组件中：

<p @click="add">按钮</p>
<p>{{count_zh}}</p>


computed: {
    count_zh() {
        return this.$store.state.count;
    },
},
methods: {
    add() {
        this.$store.commit('SOME_MUTATION', 10);
    }
}
```

#### ...mapMutations

```javascript
//html部分:
<p @click="increment">按钮</p>
<p @click="incrementBy(10)">按钮</p>  	 // 传递参数到 mutations
<p @click="add">按钮</p>
<p @click=“para(10)">按钮</p>						 // 传递参数到 mutations 


//js部分：
import { mapMutations } from 'vuex'

methods: {
    ...mapMutations([
        'increment', // 将 `this.increment()` 映射为 `this.$store.commit('increment')`

        // `mapMutations` 也支持载荷：
        'incrementBy' // 将 `this.incrementBy(amount)` 映射为 `this.$store.commit('incrementBy', amount)`
    ]),

    ...mapMutations({
        add: 'increment' // 将 `this.add()` 映射为 `this.$store.commit('increment')`
        para: 'increment', // 将 `this.add(amount)` 映射为 `this.$store.commit('increment’,amount)`
    })
}
```

### 2-6、store之Action

> 主要是用于接口调用(处理异步数据)

> Action 函数接受一个与 store 实例具有相同方法和属性的 context 对象,因此可以使用commit，getters，state等

```javascript
const store = new Vuex.Store({
  state: {
    count: 0
  },
  mutations: {
    increment (state) {
      state.count++
    }
  },
  actions: {
    increment (context) {
      context.commit('increment’); 
    }
  }
})
  

//当我们需要多次调用commit时,可以利用参数解构来赋值

actions: {
  increment ({ commit }) {
    commit('increment')
  }
}
```

```javascript
//组件中：

methods: {
    add(){
        this.$store.dispatch('increment'); //通过dispatch触发action中对应的函数
        store.dispatch('incrementAsync', { amount: 10 }); // 以载荷形式分发
        store.dispatch({ type: 'incrementAsync', amount: 10 }); // 以对象形式分发
    }
}
```

#### ...mapActions

```javascript
 methods: {
    ...mapActions([
      'increment', // 将 `this.increment()` 映射为 `this.$store.dispatch('increment')`

      // `mapActions` 也支持载荷：
      'incrementBy' // 将 `this.incrementBy(amount)` 映射为 `this.$store.dispatch('incrementBy', amount)`
    ]),
    ...mapActions({
      add: 'increment' // 将 `this.add()` 映射为 `this.$store.dispatch('increment')`
    })
  }
```

#### 组合Action

```javascript
// Promise:
actions: {
  actionA ({ commit }) {
    return new Promise((resolve, reject) => {
      setTimeout(() => {
        commit('someMutation')
        resolve()
      }, 1000)
    })
  }
}

store.dispatch('actionA').then(() => {
  // ...
})

// async / await 方法:
actions: {
  async actionA ({ commit }) {
    commit('gotData', await getData())
  },
  async actionB ({ dispatch, commit }) {
    await dispatch('actionA') // 等待 actionA 完成
    commit('gotOtherData', await getOtherData())
  }
}
```

