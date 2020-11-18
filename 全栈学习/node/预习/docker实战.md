## 实战 nginx

**需要实现功能：本地跟新代码，代码实时同步到服务器**



```shell
# 登录到远程服务器
$ ssh root@IP

# 创建文件夹
$ mkdir docker_ci
$ cd docker_ci
```



[实战项目地址](https://github.com/su37josephxia/docker_ci)

```js
# 克隆实战项目到本地
$ git clone https://github.com/su37josephxia/docker_ci.git


// vscode 安装插件 Deploy
// 当前目录下新建 .vscode 文件，写入以下内容

{
    "deploy": {
        "packages": [{
            "files": [		// 所有文件
                "**/*",
            ],

            "exclude": [	// 排除那些文件
                "node_modules/**",
                ".git/**",
                ".vscode/**",
                "**/node_modules/**",
            ],
            "deployOnSave": false	// 是否自动同步
        }],
        "targets": [{				// 部署到什么地方
            "type": "sftp",	// 上传方式
            "name": "AliyunServer",	// 随便写，代表主机名
            "dir": "/root/source/docker_ci",	// 存放到服务上的哪个目录
            "host": "47.98.252.43",	// 云服务公网ip
            "port": 22,
            "user": "root",
            "privateKey": "/Users/xia/.ssh/id_rsa"	// 登录云服务的私钥地址
        }],
    },
}
```



```shell
# 当前项目根目录下新建 nginx 文件夹
#  nginx新建 conf.d 文件夹
# nginx/conf.d 新建文件 docker.conf 

# docker.conf 文件写入以下内容
server {
	listen 80;
	location / {
		root /var/www/html;
		index index.html index.htm;
	}
	
	# 访问静态图片
	location ~ \.(gif|jpg|png)$ {
		root /static;
		index index.html index.htm;
	}
}
```

```shell
# 右键 docker.conf
# 选择 Deploy current file/folder
```



```shell
# 登录云服务查看是否上传了
```



```shell
# 在本地项目根目录新建文件 docker-compose.yml

# 写入以下内容
version: '3.1'
services:
nginx:
	restart: always
	image: nginx
	ports:
		- 8091:80		# 将容器的 80 映射到 8091，原因是：方便nginx代理docker上的服务
	volumes:
		- ./nginx/conf.d/:/etc/nginx/conf.d		# 容器中的xx文件 ：映射到实体机的xx文件
		- ./frontend/dist:/var/www/html/
		- ./static/:/static/


# ./nginx/conf.d 、./frontend 、./static 当前项目的文件或者文件夹，上传到远程服务中的 docker 中
# 通过 volumes 将 docker 中的 nginx 配置映射到远程服务的 ningx 上
```

```shell
# 登录远程服务

# 通过 docker-compose 启动发布
$ docker-compose up

# 打开浏览器输入： 远程服务IP:8091


# 如果无法访问，需要在安全组策略中开启 8091 端口
```



## 实战 NodeJs

```shell
# node 项目根目录下新建 process.yml
# process.yml 是 pm2 的配置文件

apps:
  - script : server.js
    instances: 2
    watch  : true
    env    :
      NODE_ENV: production
```

```shell
# node 项目根目录下新建 .dockerignore
# .dockerignore 上传忽略某些文件

node_modules
```

```shell
# node 项目根目录下新建 Dockerfile

FROM keymetrics/pm2:latest-alpine
WORKDIR /usr/src/app
ADD . /usr/src/app
RUN npm config set registry https://registry.npm.taobao.org/ && \  
    npm i

# RUN npm i


EXPOSE 3000


#pm2在docker中使用命令为pm2-docker
# CMD ["pm2-runtime", "start", "--json", "process.json"]
CMD ["pm2-runtime", "start",  "process.yml"]

```

```shell
# 修改数据库配置
module.exports = {
    url: "mongodb://mongo:27017",
    dbName: 'taro',
}
```

```shell
# nginx/conf/docker.conf 下添加反向代理
location /api {
    proxy_pass  http://app-pm2:3000;
    proxy_redirect     off;
    proxy_set_header   Host             $host;
    proxy_set_header   X-Real-IP        $remote_addr;
    proxy_set_header   X-Forwarded-For  $proxy_add_x_forwarded_for;
}
```

```shell
# docker-compose文件添加
version: '3.1'
	services:
		app-pm2:
			container_name: app-pm2
			#构建容器
			build: ./backend
			ports:
				- "3000:3000"
		mongo:
			image: mongo
			restart: always
			ports:
				- 27017:27017

```



## 实战  webHook 持续集成 

目标：git push 之后，服务会自动更新

