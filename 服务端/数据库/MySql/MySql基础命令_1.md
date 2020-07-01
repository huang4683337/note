## Mac 如何登陆数据库

```shell
$ mysql -u root -p  #登陆  

# 密码：kaiwen...0

# 退出(任何一个都可以)
$ quit  
$ exit 
$ \q
control + D 组合键
```



### 数据库语法介绍

```shell
CREATE  			# 为必填
{DATABASE | SCHEMA} # {} ==> 为必填项;  | ==> 必选其一
[IF NOT EXISTS]  	# [] ==> 可选项
IF NOT EXISTS 		# 判断当前库或者表中有没有  如果有会忽略错误  
```



## 数据库以及表的显示

```shell
$ drop table 表名；
$ show databases;    	# 显示所有数据库名字
$ use t1;   			# 进入数据库 t1;
$ show tables;   		# 显示 当前 数据库下的表名
$ describe users1;   	# 显示 当前数据库下 数据表users1的结构
$ SHOW COLUMNS FROM tb1;# 查看数据表结构

$ SHOW TABLE FROM 数据库名字;  # 查询当前数据库下的所有表名
$ SELECT * FROM 表名;  		# 查询当前数据表下的内容
$ SELECT * FROM 表名\G;  		# 以网格的形式显示当前数据表下的内容
$ SHOW CREATE DATABASE +数据库名字;      # 查看数据库编码
$ SHOW WARNINGS;             # 查看错误信息
$ SELECT DATABASE();  		 # 查看用户当前打开的数据库
$ SHOW TABLES FROM mysql	 # 查看mysql数据库中所有的数据表
```



## 数据库以及表的创建

```shell
$ create database data;  # 创建数据库 data

$ use tab1； 			# 创建数据表 tab1
或者
$ create table tab1 (字段设定列表)；	# 创建数据表 tab1
```



## 数据库以及数据表的删除

```shell
$ drop database data;    # 删除数据库 data 
$ drop table tab1；  	# 删除数据表 tab1
```



## 清空/显示数据表的数据

```shell
$ delete from tab1;		# 清空数据表 tab1 中的记录
$ select * from tab1; 	# 显示数据表 tab1 中的记录 
```



## 向数据库中插入表记录

```shell
$ INSERT [INTO] tbl_name [(col_name,…)] VALUES(valL,...)
# tbl_name: 表的名字
# col_name: 那几个列
# val: 每个列所对应的值是什么
```

