# Nginx

## 下载安装

[nginx配置详解](https://blog.csdn.net/finnson/article/details/82461600)



## 安装好的文件位置

```shell
/usr/sbin/nginx：主程序
/etc/nginx：存放配置文件
/usr/share/nginx：存放静态文件
/var/log/nginx：存放日志
```



## 配置 Nginx

### 连接服务器

```bash
sudo ssh 服务器外网ip
输入电脑密码，输入服务器密码
```



### 配置文件

配置文件为 `/etc/nginx/nginx.conf` 和 `/etc/nginx/conf.d/default.conf`

```bash
cd /etc/nginx
vim nginx.conf
cd /etc/nginx/conf
vim default.conf
```



### 自定义nginx.conf 配置

```bash
#运行用户，默认即是nginx，可以不进行设置
user root;
#Nginx进程，一般设置为和CPU核数一样
worker_processes 1;
#进程pid存放位置
pid /run/nginx.pid;

events {
    worker_connections  1024; # 单个后台进程的最大并发数
    # multi_accept on;
}

http {
    ##
    # Basic Settings
    ##
    sendfile on; #开启高效传输模式
    tcp_nopush on; #减少网络报文段的数量
    tcp_nodelay on;
    keepalive_timeout 65; #保持连接的时间，也叫超时时间
    types_hash_max_size 2048;
    # server_tokens off;

    # server_names_hash_bucket_size 64;
    # server_name_in_redirect off;

    include /etc/nginx/mime.types; #文件扩展名与类型映射表
    default_type application/octet-stream; #默认文件类型

    ##
    # SSL Settings
    ##

    ssl_protocols TLSv1 TLSv1.1 TLSv1.2; # Dropping SSLv3, ref: POODLE
    ssl_prefer_server_ciphers on;

    ##
    # Logging Settings
    ##

    access_log /var/log/nginx/access.log;
    error_log /var/log/nginx/error.log;

    ##
    # Gzip Settings
    ##

    gzip on; #开启gzip压缩
    gzip_disable "msie6";

    # gzip_vary on;
    # gzip_proxied any;
    # gzip_comp_level 6;
    # gzip_buffers 16 8k;
    # gzip_http_version 1.1;
    # gzip_types text/plain text/css application/json application/javascript text/xml application/xml application/xml+rss text/javascript;

    ##
    # Virtual Host Configs
    ##

    include /etc/nginx/conf.d/*.conf; #包含的子配置项位置和文件
    # include /etc/nginx/sites-enabled/*;
}

#mail {
#    # See sample authentication script at:
#    # http://wiki.nginx.org/ImapAuthenticateWithApachePhpScript
#
#    # auth_http localhost/auth.php;
#    # pop3_capabilities "TOP" "USER";
#    # imap_capabilities "IMAP4rev1" "UIDPLUS";
#
#    server {
#        listen     localhost:110;
#        protocol   pop3;
#        proxy      on;
#    }
#
#    server {
#        listen     localhost:143;
#        protocol   imap;
#        proxy      on;
#    }
#}

```



### 自定义default.conf 配置

```bash
server {
        listen       80;   #配置监听端口
        server_name  localhost;  #配置域名
        #charset koi8-r;
        #access_log  /var/log/nginx/host.access.log  main;

        location / {
            #服务默认启动目录
            root /usr/share/nginx/html/pc;   #pc
            # nginx适配pc和app设备
            if ($http_user_agent ~* '(Android|webOS|iPhone|iPod|BlackBerry)') {
                root /usr/share/nginx/html/app;  #app
            }
            index  index.html;    #默认访问文件
            allow  all; #允许访问的ip
            # deny   all;  #拒绝访问的ip
        }

        #  redirect  error pages to the static page /404.html
        error_page  404   /static/html/404/404.html;   # 配置404页面
        # redirect server error pages to the static page /50x.html
        error_page   500 502 503 504  /static/html/404/500.html;   #错误状态码的显示页面，配置后需要重启

        # ^~ 表示uri以某个常规字符串开头,大多情况下用来匹配url路径，nginx不对url做编码，因此请求为/static/20%/aa，
        # 可以被规则^~ /static/ /aa匹配到（注意是空格）。
        location ^~ /static/ {
            root /usr/share/nginx/html;
            allow  all; #允许访问的ip
            # deny   all;  #拒绝访问的ip
        }

        location = /50x.html {
            root /usr/share/nginx/html/pc/;   #pc
            # nginx适配pc和app设备
            if ($http_user_agent ~* '(Android|webOS|iPhone|iPod|BlackBerry)') {
                root /usr/share/nginx/html/app/;  #app
            }
        }

        # proxy the PHP scripts to Apache listening on 127.0.0.1:80
        #
        #location ~ \.php$ {
        #    proxy_pass   http://127.0.0.1;
        #}

        # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
        #
        #location ~ \.php$ {
        #    root           html;
        #    fastcgi_pass   127.0.0.1:9000;
        #    fastcgi_index  index.php;
        #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
        #    include        fastcgi_params;
        #}

        # deny access to .htaccess files, if Apache's document root
        # concurs with nginx's one
        #
        #location ~ /\.ht {
        #    deny  all;
        #}
    }

```



### 自定义默认目录

我们服务的默认目录放在 `/usr/share/nginx/html` 了下。

```bash
cd /usr/share/nginx/html
ls
```

> 假如你要定义服务器的默认访问目录，修改 `location /` 中的root即可，不过需要开通下你自定义目录的权限。

以 `/root/www` 是自定义目录为例

```bash
# 需要一层层分别开通权限
chmod -R 777 /root
chmod -R 777 /root/www
```



#### 自定义错误页面

```bash
# redirect  error pages to the static page /404.html
error_page  404   /static/html/404/404.html;   # 配置404页面
# redirect server error pages to the static page /50x.html
error_page   500 502 503 504  /static/html/404/500.html;   #错误状态码的显示页面，配置后需要重启
```

然后在你默认或者自定义的服务器目录下，根据你的错误页配置，新建相关的页面即可。



### Nginx访问权限和路径匹配规则

在匹配规则里面，有2个字段可以控制这个规则下的访问权限。

```bash
location / {
    allow  all; #允许访问的ip
    # deny   all;  #拒绝访问的ip
}
```

实际情况中，访问权限的控制还是比较复杂的，例如，要求服务器 `static`（静态目录）所有用户都能访问，且，重新定义访问路径,我们需要location块来完成相关的需求匹配。

```bash
# ^~ 表示uri以某个常规字符串开头,大多情况下用来匹配url路径，nginx不对url做编码，因此请求为/static/20%/aa，
# 可以被规则^~ /static/ /aa匹配到（注意是空格）。
location ^~ /static/ {
root /usr/share/nginx/html;
allow  all; #允许访问的ip
# deny   all;  #拒绝访问的ip
}
```



**对于nginx路径匹配规则，也需要简单的了解一下**

+ = 表示精确匹配

+ ^~ 表示uri以某个常规字符串开头,大多情况下用来匹配url路径，nginx不对url做编码，因此请求为/static/20%/aa，可以被规则^~ /static/ /aa匹配到（注意是空格）。

+ ~ 正则匹配(区分大小写)

+ ~* 正则匹配(不区分大小写)

+ !~和!~*分别为区分大小写不匹配及不区分大小写不匹配 的正则

+ / 任何请求都会匹配

  

**符号的优先级**

> 首先匹配 =，其次匹配^~, 其次是按文件中顺序的正则匹配，最后是交给 / 通用匹配。当有匹配成功时候，停止匹配，按当前匹配规则处理请求