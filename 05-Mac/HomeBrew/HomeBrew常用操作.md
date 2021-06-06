## 1、安装卸载

```shell
$ brew -v									# 查看 brew 版本信息
$ brew install <name> 		# 安装指定软件
$ brew uninstall <name> 	# 卸载指定软件
$ brew uninstall <name> --force 	# 彻底卸载指定软件，包括旧版本
$ brew list								# 显示所有已安装过的软件
$ brew search	node				# 搜索本地远程仓库的软件，已安装会显示绿色的勾
$ brew search /正则/			 # 使用正则表达式搜软件

$ brew deps --installed --tree # 查看已安装的包的依赖，树形显示
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



## 4、更新源

```shell
# 查看镜像源
$ git -C "$(brew --repo)" remote -v
# 替换brew.git:
$ cd "$(brew --repo)"
# 中国科大:
$ git remote set-url origin https://mirrors.ustc.edu.cn/brew.git
# 清华大学:
$ git remote set-url origin https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/brew.git



# 查看镜像源
$ git -C "$(brew --repo homebrew/core)" remote -v
# 替换homebrew-core.git:
$ cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
# 中国科大:
$ git remote set-url origin https://mirrors.ustc.edu.cn/homebrew-core.git
# 清华大学:
$ git remote set-url origin https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-core.git



# 查看镜像源
$ s
# 替换homebrew-core.git:
$ cd "$(brew --repo)/Library/Taps/homebrew/homebrew-cask"




# 应用生效:
$ brew update
```



**重置更新源**

```shell
# 重置brew.git:
$ cd "$(brew --repo)"
$ git remote set-url origin https://github.com/Homebrew/brew.git

# 重置homebrew-core.git:
$ cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
$ git remote set-url origin https://github.com/Homebrew/homebrew-core.git
```



## 5、解决 homebrew 问题

```shell
# 诊断Homebrew的问题:
$ brew doctor

# 重置brew.git设置:
$ cd "$(brew --repo)"
$ git fetch
$ git reset --hard origin/master

# homebrew-core.git同理:
$ cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
$ git fetch
$ git reset --hard origin/master

# 应用生效:
$ brew update 
```

