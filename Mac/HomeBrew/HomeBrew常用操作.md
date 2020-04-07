## 1、安装卸载

```shell
$ brew -v									# 查看 brew 版本信息
$ brew install <name> 		# 安装指定软件
$ brew uninstall <name> 	# 卸载指定软件
$ brew uninstall <name> --force 	# 彻底卸载指定软件，包括旧版本
$ brew list								# 显示所有已安装过的软件
$ brew search	node				# 搜索本地远程仓库的软件，已安装会显示绿色的勾
$ brew search /正则/			 # 使用正则表达式搜软件
```



## 2、升级相关

```shell
$ brew update				 	 	# 升级 homebrew
$ brew outdated					# 检测过时的软件
$ brew upgrade					# 升级所有已过时的软件
$ brew upgrade <name>		# 升级指定软件
$ brew pin <name>				# 禁止指定软件升级
$ brew unpin <name>			# 解锁禁止升级
$ brew upgrade --all		# 升级所有的软件包，包括为清理干净的旧的版本包
```



## 3、清理相关

```shell
# homebrew再升级软件时候不会清理相关的旧版本，在软件升级后我们可以使用如下命令清理

$ brew cleanup -n # 列出需要清理的内容
$ brew cleanup <name> # 清理指定的软件过时包
$ brew cleanup # 清理所有的过时软件
```

