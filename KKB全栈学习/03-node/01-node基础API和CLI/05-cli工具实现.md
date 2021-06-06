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



## 实现 cli雏形

```shell
# demo-cli 下新建文件夹 bin/start.js

# 写入以下内容
# #!/usr/bin/env node  制定脚本解释器为 node
#!/usr/bin/env node 
console.log('cli.....')


# package.json 中添加以下内容，放在 "scripts": {...} 就行
"bin": {
  "abc": "./bin/start.js"
}

# 生成一个软连接，相当于全局安装我们自己设置的这个包
$ npm link 
```

```js
// 全局任何位置启动命令行，输入 abc 

// 控制台会显示 console.log('cli.....')
```





## 定制命令行界面

```js
// bin/start.js 下添加内容

#!/usr/bin/env node
const program = require('commander');   // 使用 commander 库 
program.version(require('../package').version)  // 定义版本，版本号是package.json 中的version
program
    .command('init <name>')     // 初始化名
    .description('init project')    //init <name> 是干嘛的
    .action(name => {
        console.log('init ' + name)
    })
program.parse(process.argv)


// 命令行输入 abc 可以看大相关信息
// acb -V 查看版本信息
```





## 打印欢迎界面

```js
// demo-cli 下新建文件夹 lib/init.js

const { promisify } = require('util')       // node 自带 promise 
const figlet = promisify(require('figlet')) // 将信息转成大字
const clear = require('clear')  // clear 清除控制台
const chalk = require('chalk')  // 把 console 字体变色
const log = content => console.log(chalk.green(content))    // 将 log 信息包装下，变个颜色
module.exports = async name => {
    // 打印欢迎画面
    clear()
    const data = await figlet('ABC Welcome')
    log(data)
}

```

```js
// bin/kkb.js 修改 action
.action(require('../lib/init'))
```

```js
// 控制台输入 abc init 项目名称
$ abc init a
```





## 克隆脚手架

```js
// /lib/download.js
const { promisify } = require('util')
module.exports.clone = async function (repo, desc) {    // repo： 下载谁; desc：下载到哪
    const download = promisify(require('download-git-repo'))    // 使用三方库
    const ora = require('ora')  // 下载进度的三方库
    const process = ora(`🚗下载.....${repo}`) // 提示下载信息
    process.start()     // 开始下载进度提示
    await download(repo, desc)
    process.succeed()   // 终止下载进度提示
}
```

```js
// /lib/init.js

// 添加以下

const { clone } = require('./download')
module.exports.init = async name => {
    // console.log('init ' + name)
    log('🚀创建项目:' + name)
    // 从github克隆项目到指定文件夹
    await clone('github:su37josephxia/vue-template', name)
}
```

```js
// 控制台输入 abc init a
```







## 自动安装依赖

就像 vue-cli 一样自动安装路由等

实现逻辑就是 cd 到某目录下执行 npm i

```js
// /lib/init.js 添加

// promisiy化spawn
// 对接输出流
const spawn = async (...args) => {
    const { spawn } = require('child_process');
    return new Promise(resolve => {
        const proc = spawn(...args)
        proc.stdout.pipe(process.stdout)
        proc.stderr.pipe(process.stderr)
        proc.on('close', () => {
            resolve()
        })
    })
}

module.exports = async name => {
  ...
   // 安装依赖
    // npm i
    log('安装依赖')
    await spawn('cnpm', ['install'], { cwd: `./${name}` })
    log(chalk.green(
        `👌安装完成：
        To get Start:
        ===========================
        cd ${name}
        npm run serve
        =========================== 开课吧web全栈架构师
        启动项目
        约定路由功能
        loader 文件扫描
        代码模板渲染 hbs Mustache风格模板
        /lib/refresh.js
        `
    ))
}
```

```js
// 控制台输入 abc init a
```

