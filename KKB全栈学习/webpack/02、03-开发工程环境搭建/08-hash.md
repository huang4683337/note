## hash

- **hash影响整个工程环境**

  ```js
  output: {
      path: path.resolve(__dirname, "./dist"),
      // 文件名+6位hash码
      filename: "[name]-[hash:6].js",
  },
    
  // 编译时内容发生改变时，hash 会发生改变
  ```

- **chunkhash**

  chunkhash 只影响一个 chunk 下的模块

  ```js
  filename: "[name]-[chunkhash:6].js",
  ```

  ```shell
  一个 entry 的入口对应一个 chunk
  ```

- **contenthash**

  contenthash只根据自身内容是否发生改变

  ```js
  filename: "[name]-[contenthash:6].js",
  ```

  



## 注意

图片使用 contenthash，因为图片一般不会改变

css 使用 contenthash。

整个 output 使用 hash。