# MongoDB

##1、HomeBrew安装 MongoDB

```shell
$  brew install mongodb  # mac 上有homebrew

$ brew install mongodb --devel # 安装最新版本

$ brew install mongodb --with-openssl # 如果要安装支持 TLS/SSL 
```

> 安装命令输入完毕后我们可能会碰到这么一个错误
>
>   [#<Dependency: "python@2" []>, #<Options: []>]
>
> cannot be installed as binary package and must be built from source.
>
> Install the Command Line Tools:
>
>   xcode-select --install



```shell
$ brew search mongodb  # 这种情况下我们需要一个兼容版本

# 将得到以下答案
`==> Searching local taps...
 mongodb                 mongodb@3.0             mongodb@3.2             
 mongodb@3.4             percona-server-mongodb`
```



```shell
$ brew install mongodb@3.0  # 选择一个版本进行安装
```



##2、配置MongoDB

>安装完 MongoDB 后，需要配置一下 MongoDB ，不然是无法启动服务端的。
>
>如果是第一次运行MongoDB，需要指定MongoDB进程读写数据的目录，默认情况下，MongoDB会使用 /data/db 这个目录来写入数据，因此在根目录下直接创建 data/db文件夹

###2-1、创建数据库文件夹

```shell
$ mkdir -p /data/db

# 如果出现 permission denied提示 ，记得加上 sudo 命令：

$ sudo mkdir -p /data/db
```

### 2-2、给 /data/db 文件夹赋予权限

```shell
$ whoami #	查看用户名
$ sudo chown  用户名 /data/db
```

### 2-3、配置环境变量

```shell
# 通过 which mongod 查看mongodb安装路径

$ vim ~/.bash_profile # 使用vim编辑 文件

# 或者 open -e .bash_profile # 使用记事本编辑

export PATH=${PATH}: mongodb安装路径

$ source ~/.bash_profile # 使环境变量生效
```



## 3、运行 MongoDB

```shell
# 打开一个终端
$ mongod  # 是用来连接到mongodb数据库服务器的，即服务器端。

# 打开浏览器 localhost:27017 出现以下内容证明成功
# It looks like you are trying to access MongoDB over HTTP on the native driver port.



# 再次打开一个终端
$ mongo  # 是用来启动MongoDB shell的，是mongodb的命令行客户端
```



## 4、退出MongoDB

```shell
# 在打开 mongo 的终端
$ use admin;

$ db.shutdownServer();

$ exit;
```

