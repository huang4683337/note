# transform（变形）

## 属性

|                   属性                    |         介绍         |              例子              |
| :---------------------------------------: | :------------------: | :----------------------------: |
|                  rotate                   | 旋转（以中心为原点） |   transform: rotate(10deg);    |
|  `skew`   skew(x,y), skewX(x), skewY(y)   |      扭曲、倾斜      |    transform: skew(20deg);     |
| `scale`  scale(x,y), scaleX(x), scaleY(y) |  缩放（负数为缩小）  |     transform: scale(1.5);     |
|    `translate`  translateX,translateY     |         移动         | transform: translate(120px,0); |
|                  matrix                   |       矩阵变形       |                                |

```css
a:hover{
  transform:rotate(10deg) skew(20deg)  scale(1.5) translate(120px,0);
}
```



# transition（转换）

## 属性

|            属性            |         介绍         |                             例子                             |
| :------------------------: | :------------------: | :----------------------------------------------------------: |
|    transition-property     | 动画需要改变的 属性  | none(没有属性改变)；all（所有属性改变）这个也是其默认值；indent（元素属性名） |
|    transition-duration     | 指定元素转换所需时间 |                   transition-duration:1s;                    |
| transition-timing-function |       转换函数       | liner：匀速；ease：逐渐慢；ease-in：加速；ease-out：减速；ease-in-out:先加速后减速 |
|      transition-delay      |   延迟几秒开始动画   |                    Transition-delay: 0s;                     |

```css
  a {
    transition: background 0.5s ease-in,color 0.3s ease-out;
    transition：transform .4s ease-in-out;
  }
```

