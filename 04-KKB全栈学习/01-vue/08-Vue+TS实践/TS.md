vue使用ts

+ 通过 exted 实现

  ```js
  export default Vue.extends({
    
  })
  ```

+ 通过 `vue-property-decorator` 实现





interface 只定义结构不定义实现



联合类型

```js
let a: string | number;
a 既可以是数字 也可以是 字符串
```



交叉类型

```js

```



泛型

```js
在定义时不指定具体类型，使用时传入具体类型
```





装饰器

装饰器是⼯⼚函数，它能访问和修改装饰⽬标。

@xx 和 @xx( ) 区别

```js
// @xx
function log(target: Function) {
  target.prototype.log = function () {
    console.log('11111')
  }
}

@log
class Foo {
  bar = 'bar'
}

const foo = Foo();

foo.log();
```



```js
// @xx()
function log(fn: any) {
  return function (target: Function) {
    target.prototype.log = function () {
      fn('111')
    }
  }
}

@log(window.alert)
class Foo {
  bar = 'bar'
}

const foo = Foo();

foo.log();
```

+ 类装饰器
+ 方法装饰器
+ 属性装饰器

