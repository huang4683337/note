[Tomcat集群，Nginx集群，Tomcat+Nginx 负载均衡配置，Tomcat+Nginx集群](https://blog.csdn.net/weixin_34375233/article/details/86337188?utm_medium=distribute.wap_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-4.nonecase&depth_1-utm_source=distribute.wap_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-4.nonecase)



[Tomcat下部署vue项目[history模式]](https://www.jianshu.com/p/18dd6eedd899)



## Mac 安装 tomcat

```shell
$ brew search tomcat	# 检查 tomcat 是否存在

# 结果显示如下
==> Formulae
tomcat              tomcat-native       tomcat@7            tomcat@8
```

```shell
$ brew install tomcat #	安装 tomcat
```

```shell
$ catalina -h  # 检查是否安装成功
```

```shell
$ catalina run	# 临时运行tomcat

$ brew services start tomcat  # 正式启动 
```

```js
// Tomcat的默认端口是8080，如果运行成功可通过 http://localhost:8080 访问

// 我自己将端口改为 8888 ==> http://localhost:8080

// webapp的根目录(CATALINA_HOME)为:/usr/local/Cellar/tomcat/7.0.33/libexec/webapps/ROOT/
```

