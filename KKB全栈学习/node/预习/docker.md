## 安装 docker

[centos 安装docter](https://www.runoob.com/docker/centos-docker-install.html)



## docker 开启远程访问

[docker开启远程访问（CentOS系统）](https://blog.csdn.net/longzhanpeng/article/details/82217398)



## vscode使用docker

[vscode远程连接docker](https://www.cnblogs.com/adawoo/p/11056658.html)





## docker 上的 nginx 虚拟服务

```shell
# 拉取官方镜像 - 面向docker的只读模板
docker pull nginx

# 查看
docker images nginx


# 启动镜像 - 在 nginx 的 www 下创建一个 index.html 内容是 hello docker!!
mkdir www
echo 'hello docker!!' >> www/index.html


# 启动
docker run -p 8000:80 -v $PWD/www:/usr/share/nginx/html nginx
# -p 端口映射 
# 8000：发布到服务器的8000端口 （实体机）
# 80：容器使用的80端口 （映射机）
# $PWD：访问哪个文件
# nginx：使用的镜像名叫 nginx
# 打开浏览器：云服务公网ip：8000 访问


# 后台启动
docker run -p 80:80 -v $PWD/www:/usr/share/nginx/html -d nginx
# 会打印出一个 uuid



# 停止
docker stop ff6
# ff6: uuid 的一部分



# 查看进程
docker ps
docker ps -a // 查看全部


# 伪终端（进入docter虚拟机） ff6容器的uuid
docker exec -it ff6 /bin/bash


# 删除镜像（先停止）
docker rm ff6

```



## docker 运行过程

+ 镜像（Image）
  面向Docker的只读模板：在空的 docker 虚拟系统上安装了 nginx 服务并保存起来
+ 容器（Container）
  镜像的运行实例：将镜像运行起来
+ 仓库 (Registry)
  存储镜像的服务器：从docker仓库拉取的虚拟镜像，就像从git拉取代码



+ docker pull 从 docker 仓库拉取镜像
+ docker run 将拉取的镜像实例化（运行起来）

、



## 如何定制一个镜像

>镜像的定制实际上就是定制每一层所添加的配置、文件。如果我们可以把每一层修改、安装、构建、操作的
>命令都写入一个脚本，用这个脚本来构建、定制镜像，那么之前提及的无法重复的问题、镜像构建透明性的
>问题、体积的问题就都会解决。这个脚本就是 Dockerfile。



**定制自己的web服务器**

```shell
# 登录已经安装 docker 的服务
# 创建一个 nginx 镜像文件
$ mkdir nginx
```



```shell
# Dockerfile: 定制镜像的描述文件

$ vi Dockerfile

# 文件中粘贴
FROM nginx:latest
RUN echo '<h1>Hello, Kaikeba!</h1>' > /usr/share/nginx/html/index.html

# FROM nginx 开始往下写
# latest： 最新版

# RUN 在定制镜像过程中执行的命令
# echo： 输出 '<h1>Hello, Kaikeba!</h1>'
# >： 将 '<h1>Hello, Kaikeba!</h1>' 输出到 docker虚拟镜像的 /usr/share/nginx/html/index.html 文件
```

```shell
# 定制镜像
docker build -t nginx:kaikeba .
# nginx：定制镜像的名字
# kaikeba： 版本
# . ：Dockerfile: 定制镜像的描述文件 在当前目录下


# :wq 保存退出

# 运行
docker run -p 80:80 nginx:kaikeba


# 打开浏览器： 云服务ip:80
```



## 定制nodeJS镜像

```shell
# 登录已经安装 docker 的服务

# cd source/docker

# 创建一个 node 镜像文件
$ mkdir node

# 进入 node 文件夹
$ cd node

# 初始化 node 仓库
$ npm init -y 

# 安装一个 koa 包
$ npm i koa -s

# 查看 koa 包是否安装
$ cat package.jsn
```

```js
// vi app.js
const Koa = require('koa')
const app = new Koa()
app.use(ctx => {
  ctx.body = 'Hello Docker'
})
app.listen(3000, () => {
  console.log('app started at http://localhost:3000/')
})
```



```shell
# 定制 Dockerfile
$ vi Dockerfile

# 添加以下内容
FROM node:10-alpline  # 定制 node 版本是10-alpline
ADD . /app/ #将此文件添加到 app 目录下 
WORKDIR /app	# 进入 app 目录下
RUN npm install # 运行 npm install
EXPOSE 3000 # 暴露 3000 端口
CMD ["node","app.js"] # docker run 运行时执行 


# :wq 保存退出
```

```shell
# 代码编译为 mynode，并且输入 Dockerfile 的文件路径 -- . 代表Dockerfile在当前目录下
$ docker build -t mynode . 


# 运行
docker run -p 3000:3000 mynode


# 打开浏览器： 云服务ip:3000
```



## 定制PM2镜像

```shell
# 登录已经安装 docker 的服务

# cd source/docker

# 将上面 node 文件克隆到 pm2文件
$ cp -R node pm2
```

```shell
$ vi process.yml

# 写入以下内容
apps:
	- script : app.js				# 执行：app.js
	instances: 2						# 启动两个进程
	watch : true						# 开启监听
	env :										
		NODE_ENV: production	#	生产环境
```

```shell
# Dockerfile

FROM keymetrics/pm2:latest-alpine
WORKDIR /usr/src/app
ADD . /usr/src/app

# 设置镜像地址 并且安装 node 包
RUN npm config set registry https://registry.npm.taobao.org/ && \
npm i

EXPOSE 3000

#pm2在docker中使用命令为pm2-docker
CMD ["pm2-runtime", "start", "process.yml"]

# :wq 保存退出
```

```shell
# 代码编译为 mynode，并且输入 Dockerfile 的文件路径 -- . 代表Dockerfile在当前目录下
$ docker build -t mypm2 . 


# 运行
docker run -p 3000:3000 -d mypm2


# 打开浏览器： 云服务ip:3000
```



## 安装 docker-compose

[安装参考地址](https://blog.csdn.net/ytangdigl/article/details/103831739)

**验证是否安装成功**


```shell
# 进入自己存放docker镜像的目录，随便哪个都行
$ cd source/docker

# 新建一个 hell-world 文件夹
$ mkdir hello-world
$cd hello-world


$ vi docker-compose.yml

# 写入以下代码
version: '3.1'
services:
 hello-world:
   image: hello-world
```

```shell
# 启动
$ docker-compose up

# 关闭
$ docker-compose down

# 查看帮助
$ docker-compose help

# 查看运行的镜像
$ docker-compose ps
```



## Compose 配置 mongoDB

Compose项目是 Docker 官方的开源项目，负责实现对 Docker 容器集群的快速编排。

简单来说就是，如果是好几个 docker 项目想一起运行怎么做，Compose 就可以



[docker如何配置阿里云加速器](https://blog.csdn.net/xcc_2269861428/article/details/103781826)



**使用 Compose 搭建 mongoDB 环境**

```shell
# 登录安装了 docker 的远程服务

# 进入 docker
$ cd source/docker

# 新建一个 mongodb 文件夹
$ mkdri mongo
$ cd mongo

#
$ vi docker-compose.yml


# docker-compose.yml
# 写入以下内容
version: '3.1'      # docker-compose 版本
services:	          # 下面有几个镜像要启动
  mongo:
    image: mongo    # 镜像名字,登录时填入
    restart: always # 镜像挂了自己重启
    ports:
      - 27017:27017 # 镜像端口映射到哪里
  mongo-express:
    image: mongo-express
    restart: always
    ports:
      - 8000:8081
```

```shell
# 运行镜像
$ docker-compose up

# 第一次运行会直接下载镜像，添加阿里云镜像加速器简直飞速
```

```shell
# 打开浏览器 云服务ip:8000
```

