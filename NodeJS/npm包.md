# npm 包管理

## 1、什么是npm

+ node.js的管理器npm, 成为世界上最大的开放源码的生态系统 —> 实际上就是一个大的仓库
+ 缺点: 什么人都可以往上面扔代码, 就使上面的垃圾非常多 导致我们写大的项目的时候代码非常的不优雅
+ 大的公司会搭建一个自己的内部仓库 使用自己员工写的包



## 2、npm 官方网站

[npm官方网站（中文）](https://www.npmjs.com.cn/)

[npm官方网站（英文）](https://www.npmjs.com/)



## 3、npm的使用准备

初始化当前文件夹

```bash
$ npm init   # 初始化文件夹 package.json

package name: 包的名字
version: 当前包的版本号
description: 项目的描述信息
entry point: 项目的入口文件
test command: 测试命令
git repository: 当前包在 gitHub 的地址
keywords: 
author: 作者
license: 开源许可证
```



## 4、npm 的使用

```bash
$ npm install 	# 按照package.json中的配置拉取包

$ npm install 包的名字 --save  # 安装某个包 并且将包的信息放入 package.json 的 devDependencies 中

$ npm install 包的名字@版本号 --save  # 下载某个版本的包

$ npm uninstall 包的名字 --save  	# 删除某个包

$ npm help 	#帮助文档

$ npm publish   # 向npm发布自己的包  我们需要去npm官网创建自己的账号
```



## 5、淘宝镜像的安装

```bash
$ npm install --global cnpm  # --global 全局安装 cnpm

# 临时使用淘宝镜像
$ npm install 包名字 --registry https://registry.npm.taobao.org

# npm 时 永久使用淘宝镜像
$ npm config set registry https://registry.npm.taobao.org

# 验证 npm 淘宝镜像 是否安装成功
$ npm config list
$ npm config get registry  

```



## 6、上传npm包

```bash
$ npm init # 初始化包 会出现一个 package.json 文件 
$ npm publish   # 向npm发布自己的包  我们需要去npm官网创建自己的账号
```





