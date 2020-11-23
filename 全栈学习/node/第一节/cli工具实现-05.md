## 创建工程

```shell
# 创建 demo-cli 文件夹
$ mkdir demo-cli
cd demo-cli

# 初始化仓库
$ npm init -y

# 安装依赖
$ npm i commander download-git-repo ora handlebars figlet clear chalk open -s
```



## 实现 cli

```shell
# demo-cli 下新建文件夹 bin/start.js

# 写入以下内容
# #!/usr/bin/env node  制定脚本解释器为 node
#!/usr/bin/env node 
console.log('cli.....')


# package.json 中添加以下内容，放在 "scripts": {...} 就行
"bin": {
  "start": "./bin/start.js"
}
```

