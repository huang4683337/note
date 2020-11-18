### 安装

老师有文档

```shell
# 注意：自定义安装目录时，MySQL 安装目录和 data 安装目录需要在同一级
F:\devTool\MySQL\MySQL # MySQL 安装目录
F:\devTool\MySQL\data	 # 数据库安装目录
```





## 启动停止


```shell
# window 下启动 mysql
# cmd 管理员模式下，进入到 mysql bin目录下
$ net stop mysql80	# 停止 	mysql80是服务名称
$ net start mysql80	# 启动	mysql80是服务名称


# 直接 ctrl + shift + . 在服务中找到对应服务右键启动
```





## 进入 mysql 命令模式

```shell
# 进入到 mysql bin目录下
mysql -u root -p

# 输入密码
```

```shell
# 开始菜单找到 MySQL 8.0 Command Line Client
# 一般在第一个
```

```shell
 # ctrl + alt + .
 # 服务那一项中找到 mysql 
 # 右键启动
```





