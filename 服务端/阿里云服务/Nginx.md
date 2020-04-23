## 下载安装

```shell
$ yum install -y nginx	# 安装 nginx
```



## 配置 Nginx

### 连接服务器

```bash
$ sudo ssh 服务器外网ip
输入电脑密码，输入服务器密码

$ nginx -v	# 查看 nginx 版本
$ nginx -t 	# 查看 nginx 有无报错同时能看到默认的配置文件是哪个。于此同时也知道了 nginx 的安装位置
# nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
# nginx: configuration file /etc/nginx/nginx.conf test is successful
# 从信息可以看出 nginx 路径为：/etc/nginx； 配置文件为：nginx.conf
```



### 编辑配置文件

```shell
$ vim 上面 nginx -t 中得到的配置文件路径
```



### 配置 SSL

+ 申请一个 SSL 证书、下载 SSL 证书、解压到本地。
+ 在 nginx 文件下创建 cert 文件

```shell
$ cd /etc/nginx
$ mkdir cert
```

+ 将解压后的 SSL 放入服务器

```shell
$ cd cert
$ vim cert.pem	# 将 SSL 对应的 .pem 后缀内容粘贴进去
$ vim cert.key	# 将 SSL 对应的 .key 后缀内容粘贴进去
```

+ 修改 nginx 配置文件

```shell
# 添加一个新的 server https 一般都监听443
server {
        listen       443 ssl http2;
        root         /var/www/hexo;     # 指向git用户的个人博客存放目录
        
        # https 配置
        # ssl on;	# 有的 nginx 会报错
        ssl_certificate   cert/cert.pem;
        ssl_certificate_key  cert/cert.key;
        ssl_session_timeout 5m;
        ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
        ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
        ssl_prefer_server_ciphers on;

        location /{
            root /var/www/hexo;
            index index.html index.htm;
        }
}

# 在 server 监听的 80 端口下添加重定向
server {
	listen       80;
  # 将所有 http 请求通过 rewrite 重定向到 https
  rewrite ^(.*)$ https://$host$1 permanent;
}

# 保存并退出

# 开启 nginx 服务
$ nginx -t	# 查看是否报错
$ nginx -s stop	# 停止 nginx
$ nginx	# 打开 nginx

# 浏览器输入域名或者端口进行访问
xxx.xxx.xxx
```

+ 如果还没有进入 https

```shell
# 查看 nginx 是否监听 443 80 端口
$ netstat -tpln	# 查看是否有 443 、 80端口的进程

# 查看云服务安全组是否添加 443 端口

# 云服务防火墙是否打开 443 端口
$ netstat -tpln	# 查看是否有 443 端口的进程
$ firewall-cmd --zone=public --add-port=443/tcp --permanent	# 防火墙开启 443 端口
$ firewall-cmd --reload	# 重启防火墙

# 关闭 nginx 进程 重启
$ netstat -tpln	# 查看 80 端口进程
$ kill 进程
$ ngixn	# 启动 ngixn
```



### CentOS7 防火墙

```shell
$ firewall-cmd --state	# 查看防火墙状态
$ firewall-cmd --list-ports	# 查看防火墙开启端口
$ systemctl stop firewalld.service	# 关闭防火墙
$ systemctl start firewalld.service	# 开启防火墙
$ firewall-cmd --zone=public --add-port=443/tcp --permanent	# 防火墙开启 443 端口
$ firewall-cmd --reload	# 重启防火墙
```

