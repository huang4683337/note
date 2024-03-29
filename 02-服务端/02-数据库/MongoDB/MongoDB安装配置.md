# MongoDB

## 1、HomeBrew安装 MongoDB

[MongoDB下载地址](https://www.mongodb.com/download-center/community?jmp=nav)



## 2、配置MongoDB

>安装完 MongoDB 后，需要配置一下 MongoDB ，不然是无法启动服务端的。
>
>如果是第一次运行MongoDB，需要指定MongoDB进程读写数据的目录，默认情况下，MongoDB会使用 /data/db 这个目录来写入数据，因此在根目录下直接创建 data/db文件夹

### 2-1、创建数据库文件夹

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

```shell
$ mongod --version # 查看版本
```

## 3、运行 MongoDB

```shell
# 打开一个终端
$ mongod  # 是用来连接到mongodb数据库服务器的，即服务器端。

# 打开浏览器 localhost:27017 出现以下内容证明成功
# It looks like you are trying to access MongoDB over HTTP on the native driver port.


# 链接数据库
# 再次打开一个终端
$ mongo  # 是用来启动MongoDB shell的，是mongodb的命令行客户端
```



## 4、退出MongoDB

```shell
# 在打开 mongo 的终端
# $ use admin;

# $ db.shutdownServer();

$ exit;
```

