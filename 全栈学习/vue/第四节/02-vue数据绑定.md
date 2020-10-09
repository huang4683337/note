
暗号： BBQ



vue源码迁移到gitee


![11111](F:\桌面\11111.png)
初始化数据 后面的可以覆盖前面的


ObServer 会对data中的数组做处理，对数据添加 _ob_


当数据改变时，通过判断段数据是否有 _ob_ 来确定是否进行过数据劫持




一个组件对应一个watcher

煮茶听书 22:09:33

![img](file:///E:\QQ\782460001\Image\C2C\OISFJ5O`%1J3@`DP0RX`N%Y.png)


dep和watcher会建立双向关系；
在组件中，一个属性对应一个dep，一个dom对应一个watcher
一个组件对应一个watcher，组件中有多个属性，所以对应多个dep


一个组件对应一个watcher，是因为组件是一个大dom，可以参考vue.component('router-view', 组件)


Object,creat()会复制一个对象，深拷贝？浅拷贝？

  



大管家 dep， 小管家dep

key对应的值发生改变 ==》 a：1 =》 a = 2 走小管家

key里面的值发生改变 -- a：1 ------a = [111] 走大管家

