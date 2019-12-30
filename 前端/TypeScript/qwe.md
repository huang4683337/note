# TypeScript

## 类型

### 基本类型

```js
let str: string;	// 值必须是 String 类型
let num: number;	// 值必须是 Number 类型
let boolen: boolean;	// 值必须是 Boolen 类型

str = 123; // Type '123' is not assignable to type 'string'
str= '1233';
```



### 数组类型

```js
// 定义一个数组，类型是Number。意思就是数组中的成员只能是数字。
let arr: number[];
arr = [1,2,3];
arr = ['a'];	// Type 'string' is not assignable to type 'number'

// 定义一个数组，类型是 any。意思就是数组中的成员可以是任何类型。
let arr: any[] = [1,'2',{a:'3'}]
```



### 类型推断

根据第一次赋值的类型，来决定这个变量在以后的代码中的类型

```js
let str = '123';
str = true; // 报错 Type 'true' is not assignable to type 'string'

/*
第一次进行变量赋值时，没有定义变量str的类型，但是给 str 的值是一个 String 类型的。
这样 ts 就推断你给这个变量定义的就是 String 类型
当将 true 赋值给 str 时，因为类型不匹配 所以报错
*/
```



### any 类型

本质上就是解除`ts`对变量严格类型的检查。 不过不推荐使用。因为他会造成类型的滥用。

```js
let str: string;
str = 1;	// Type '1' is not assignable to type 'string'

// 定义变量类型是 any 类型
let str: any;
str = [1,2];

// 定一变量时 不给初始值且不定义变量类型时，会默认为 any 类型
let str;
str = '1233';
str = 2323;
```



#### 显示转换 \<any>

通过 `<any>`  显式转换一个对象的类型。

```js
let item1 = <any>{ id: 1, name: 'item1' }; 
item1 = {name:22};
/*
通过 <any> 证明 { id: 1, name: 'item1' } 对象是一个 any 类型
将对象赋值给 item1 之后，又因为类型推断，所以 item1 也是 any 类型
*/
```



### 枚举类型 enum

通过`enum`定义一个枚举

```js
enum sex {
    '男',
    '女',
    '妖'
}

console.log(sex[0]);  // 男
console.log(sex['男']);	// 0
console.log(sex);	// { '0': '男', '1': '女', '2': '妖', '男': 0, '女': 1, '妖': 2 }
```



#### 常量枚举

```js
const enum sex {
    '男',
    '女',
    '妖'
}

console.log(sex['男']); // 0 
```



### 联合类型

用来表示两个或者两个以上的数据类型的联合。 使用符号为 `|`

```js
// 定义参数 union 的数据类型为 number 或者 string
let union: number | string;

union = 1233;
union = '1233';
union = [1,2,3];	// Type 'number[]' is not assignable to type 'string'
```



#### 类型守卫

**在联合类型的函数中，对进来的参数再次进行判定**

```js
function sum(
    a: string | number,
    b: string | number,
) {
      if(typeof a==='number' && typeof b==='number'){
        console.log('对联合类型进行一次守卫=>再次判断类型');
      }
}

sum(1,"2");
```



####  类型别名

当使用联合类型时，可能会因为太多、太复杂我们可以给这个类型定个别名。

使用关键字 `type`

```js
type strNum = string | number;	// 定义一个联合类型( 字符串 | 数字 )，这个类型的别名是 strNum

let a:strNum;
a = 1;
a= '2';
a=[1,2,3];	// Type 'number[]' is not assignable to type 'string'
```



###  null 和 undefined

```js

```



## 函数

我们也可以通过强类型约束，来约束函数的返回值。

### 函数无返回值

```js
function sum(): void{
  console.log(1);
}

sum(); // 1
```

### 函数返回值为 number 类型

```js
function sum(): number{
  return 1;
}
console.log( sum() ); //1
```

### 函数的可选参数

函数的每个参数可以规定类型

```js
function sum(x: number, y: string): void {
    console.log(x + y)
}

sum(1, 'a'); // 1a
```

**可选参数只能是函数的最后一个参数，通过添加 ？ 来表示。 可以不传参数，不传时为undefined**

```js
function sum(x: number, y?: string): void {
    console.log(x);	// 5
    console.log(y)	// undefined
}

sum(5);
```

### 参数的默认值

```js
function sum(x: number=2, y: string='a'): void {
    console.log(x); // 2
    console.log(y); // a
}

sum();
```

### rest 剩余参数

+ 可以接收任何数量的参数
+ 类似于 arguments

```js
// 接收数字类型的一个或多个参数
function rest(...arr:number[]){
    arr.forEach(item=>{
        console.log(item);
    })
}

rest(9);	// 9
rest(1,2,3,4,5);	// 1 2 3 4 5
```



### 回调函数之函数签名

+ 参数 callback 必须声明它是一个函数类型 => void
+ callback  的参数必须是一个 string

```js
function callbackFn(text: string) {
    console.log(text);
}
function fn(
    fn_cs: string,
    callback: (cb_cs: string) => void
) {
    console.log(fn_cs); 
    callback(fn_cs);
}

fn('ssss',callbackFn);
```

**注意：函数签名中的参数名称可以不一样，只要参数的个数、类型、函数返回值的类型一致就可以**

> 函数签名是TypeScript的非常强大的特征之一。
>
> 当我们使用第三方 JS 代码库时，我们需要知道并且定义第三方JS代码库提供的函数签名，这些对第三方代码库的函数签名进行描述的文件，我们称之为：类型描述文件（或者声明文件），以 .d.ts 扩展名来展示。



### 函数重载

不同类型的参数调用同一个函数。

+ 定义函数需要使用的参数类型, 以及函数的类型
+ 定义执行函数体 函数以及参数类型为 any

```js
// 定义函数需要使用的参数类型, 以及函数的类型
function add(a: number, b: number): number;
function add(a: string, b: string): string;

// 定义执行函数体 函数以及参数类型为 any
function add(a: any, b: any): any{
    return a+b;
};

console.log( add(1,2) );    // 3
console.log( add('1','2') );    // 12
```



## 自定义类型 Interface



## 鸭式辨形