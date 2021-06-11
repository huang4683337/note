## 好处

+ 文件拆分：预处理器扩展了 `@import` 指令的能力，通过编译环节将切分后的文件重新合并为一个大文件

+ 模块化
+ 通过嵌套形成作用域，解决了命名冲突等问题
+ 变量、运算、函数、混合的使用，实现了 css 的封装

+ 工程化：代码校验、压缩等功能



## 变量

```
@ + 属性: 值
```

```less
@width: 10px;
@height: @width + 10px;

#header {
  width: @width;
  height: @height;
}
```

```css
#header {
  width: 10px;
  height: 20px;
}
```



## 混合 Mixins

混合（Mixin）是一种将一组属性从一个规则集包含（或混入）到另一个规则集的方法

**定义一个类**

```less
.bordered {
  border-top: dotted 1px black;
  border-bottom: solid 2px black;
}
```

**其它规则中使用**

```less
#menu a {
  color: #111;
  .bordered();
}
```



##  嵌套（Nesting）

Less 提供了使用嵌套（nesting）代替层叠或与层叠结合使用的能力

```less
#header {
  color: black;
  .navigation {
    font-size: 12px;
  }
  .logo {
    width: 300px;
  }
}
```

你还可以使用此方法将伪选择器（pseudo-selectors）与混合（mixins）一同使用。下面是一个经典的 clearfix 技巧，重写为一个混合（mixin） (`&` 表示当前选择器的父级）

```less
.clearfix {
  display: block;
  zoom: 1;

  &:after {
    content: " ";
    display: block;
    font-size: 0;
    height: 0;
    clear: both;
    visibility: hidden;
  }
}
```



##   运算（Operations）

+ 加、减运算时，单位会进行换算
+ 单位换算以最左侧为准

```less
// 所有操作数被转换成相同的单位
@conversion-1: 5cm + 10mm; // 结果是 6cm
@conversion-2: 2 - 3cm - 5mm; // 结果是 -1.5cm

// conversion is impossible
@incompatible-units: 2 + 5px - 3cm; // 结果是 4px

// example with variables
@base: 5%;
@filler: @base * 2; // 结果是 10%
@other: @base + @filler; // 结果是 15%
```



+ 乘、除运算时，单位不会做换算

```less
@base: 2cm * 3mm; // 结果是 6cm
```



+ 还可以对颜色进行运算

```less
@color: #224488 / 2; //结果是 #112244
background-color: #112244 + #111; // 结果是 #223355
```





## 函数

Less 内置了多种函数用于转换颜色、处理字符串、算术运算等。

利用 percentage 函数将 0.5 转换为 50%，

将颜色饱和度增加 5%，

以及颜色亮度降低 25% 并且色相值增加 8 等用法

```less
@base: #f04615;
@width: 0.5;

.class {
  width: percentage(@width); // returns `50%`
  color: saturate(@base, 5%);
  background-color: spin(lighten(@base, 25%), 8);
}
```



##  命名空间和访问符

有时候为了组织结构调整、或者是封装目的，希望对 混合（Mixins） 进行分组。



假设你希望将一些混合（mixins）和变量置于 `#bundle` 之下，为了以后方便重用或分发

```less
#bundle() {
  .button {
    display: block;
    border: 1px solid black;
    background-color: grey;
    &:hover {
      background-color: white;
    }
  }
  .tab { ... }
  .citation { ... }
}
```



现在，如果我们希望把 `.button` 类混合到 `#header a` 中，我们可以这样做：

```less
#header a {
  color: orange;
  #bundle.button();  // 还可以书写为 #bundle > .button 形式
}
```





##  映射（Maps）

从 Less 3.5 版本开始，你还可以将混合（mixins）和规则集（rulesets）作为一组值的映射（map）使用

```less
#colors() {
  primary: blue;
  secondary: green;
}

.button {
  color: #colors[primary];
  border: 1px solid #colors[secondary];
}
```



输出

```less
.button {
  color: blue;
  border: 1px solid green;
}
```





##  导入（Importing）

```less
@import "library"; // library.less
@import "typo.css";
```

