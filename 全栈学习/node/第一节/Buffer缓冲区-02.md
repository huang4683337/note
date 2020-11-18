## 位、字节、二进制、十六进制关系

```shell
# 位 = bit = 比特 = 1个二进制位 ( 0 | 1)

# 字节 = byte = 8个二进制位 ( 00 00 00 00 )

# 16进制 = 16个数字 = 4个二进制位

# 1字节 = 2个16进制 = 8个二进制位
```



## Buffer 的基本使用

### 创建 buffer

+ `Buffer.alloc`

  返回一个已初始化的 Buffer，可以保证新创建的 Buffer 永远不会包含旧数据。

  ```shell
  # 语法
  Buffer.alloc[size[, fill[, encoding]]]
  
  # size: 新 buffer 的长度 （必填）
  # fill：填充新 buffer 的默认值，默认为0。 类型：string | buffer | Uint8Array | integer
  # encoding：fill 的字符编码，默认‘utf-8’。
  
  # Uint8Array：数组类型表示一个8为无符号整型数组，初始化为0
  # integer：number类型
  ```

  ```js
  // 创建一个大小为 5 字节的新 Buffer，初始值为0
  const buffer = Buffer.alloc(5);    // <Buffer 00 00 00 00 00>
  ```

+ `Buffer.allocUnsafe(size)`

  ```js
  // 创建一个长度为 10、且未初始化的 Buffer。
  // 这个方法比调用 Buffer.alloc() 更快，
  // 但返回的 Buffer 实例可能包含旧数据，
  // 因此需要使用 fill() 或 write() 重写。
  const buf3 = Buffer.allocUnsafe(10);
  ```

+ `Buffer.from`

  ```js
  const b1 = Buffer.from('10');            // <Buffer 31 30>
  const b2 = Buffer.from('10', 'utf8');    // <Buffer 31 30>
  const b3 = Buffer.from([10]);            // <Buffer 0a>
  const b4 = Buffer.from(b3);              // <Buffer 0a>
  ```

  

### Buffer 字符编码

**node支持的字符编码**

+ `ascii` : 仅适用于 7 位 ASCII 数据。此编码速度很快，如果设置则会剥离高位

+ `utf8`  : 多字节编码的 Unicode 字符。许多网页和其他文档格式都使用 UTF-8
+ `utf16le` :2 或 4 个字节，小端序编码的 Unicode 字符。支持代理对（U+10000 至 U+10FFFF）
+ `ucs2` :  utf16le 的别名
+ `base64` : Base64 编码。当从字符串创建 Buffer 时，此编码也会正确地接受 RFC 4648 第 5 节中指定的 “URL 和文件名安全字母”。
+ `latin1` - 一种将 Buffer 编码成单字节编码字符串的方法（由 RFC 1345 中的 IANA 定义，第 63 页，作为 Latin-1 的补充块和 C0/C1 控制码）

+ `binary` : `latin1` 的别名
+ `hex` : 将每个字节编码成两个十六进制的字符

```js
const token = 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJkYXRhIjp7InVzZXJuYW1lIjoiYWJjIiwicGFzc3dvcmQiOiIxMTExMTEifSwiZXhwIjoxNTkxOTMzODcyLCJpYXQiOjE1OTE5MzAyNzJ9.oKAj1dYjiHaNmKB4l5hUU84yycwZMIMLg47Rt5QxKFQ';

// 创建一个内容为 token 的 Buffer，Buffer 的编码格式为 base64
const buffer = Buffer.from(token, 'base64');

// 将 base64 编码的 Buffer 转化为 utf-8 编码的字符串
const jwt = buffer.toString("utf-8");
```



### 使用 Buffer 实现转换 

**可以用于 token 解码**

```js
const token = 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJkYXRhIjp7InVzZXJuYW1lIjoiYWJjIiwicGFzc3dvcmQiOiIxMTExMTEifSwiZXhwIjoxNTkxOTMzODcyLCJpYXQiOjE1OTE5MzAyNzJ9.oKAj1dYjiHaNmKB4l5hUU84yycwZMIMLg47Rt5QxKFQ';

// 创建一个内容为 token 的 Buffer，Buffer 的编码格式为 base64
const buffer = Buffer.from(token, 'base64');

// 将 base64 编码的 Buffer 转化为 utf-8 编码的字符串
const jwt = buffer.toString("utf-8");
```



### Buffer 其他方法

+ `concat`

  Buffer.concat 将两个 Buffer 组合起来

  ```js
  const buf2 = Buffer.from('a');	// <Buffer 61>
  const buf3 = Buffer.from('中');	// <Buffer e4 b8 ad>
  
  const buf4 = Buffer.concat([buf2,buf3]);
  console.log(buf4);  // <Buffer 61 e4 b8 ad>
  
  console.log(buf4.toString());  // a中
  ```

+ 写入 Buffer 数据

  ```js
  const buf1 = Buffer.alloc(10);
  buf1.write('hello');
  console.log(buf1);  // <Buffer 68 65 6c 6c 6f 00 00 00 00 00>
  ```
  
+ 读取 Buffer 数据

    ```js
  const buf3 = Buffer.from('中');
  console.log(buf3.toString());   // 中
    ```
