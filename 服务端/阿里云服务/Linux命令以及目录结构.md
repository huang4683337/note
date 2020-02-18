# 命令以及目录结构

## 命令

```shell
$ init 0	# 关机
$ init 6	# 重启
$ ls	# 查看目录下的文件夹
$ cd	# 切换到某个目录
$ pwd	# 查看当前目录

$ history	# 查看历史命令
$ !历史编号	 # 使用某个历史命令
```

```shell
$ touch file	# 创建文件 ( 也可以批量创建 )
$ rm -rf file	# 删除文件
	-r: 递归的删除目录下面文件以及子目录下的文件
	-f: 强制删除, 忽略不存在的文件,从不给出提示
	
$ mv fielOldName fileNewName	# 文件重命名
$ mv file 文件夹	# 将某个文件移动到某个文件夹
$ cat file	# 查看文件内容
	cat file | head -3	# 查看文件前三行
	cat file | tail -3	# 查看文件后三行
	
$ cp fielOldName fileNewName	# 复制文件

$ vi file	# 编辑文件
# 在vim编辑器中 输入 / 代表搜索 ==> /list 搜索list

$ find 目录 -name 文件名	# 搜索文件
$ updatedb 
$ locate 文件名	# 使用 updatedb 搜索文件, 需要安装updatedb包
```



## 目录

**root 目录:**  linux 超级权限 root 的主目录

**home 目录:**  系统默认的用户主目录, 如果添加用户不是指定用户的主目录, 默认在 /home 下创建与用户同名的文件夹  --- 没存在一个账户, 就会在home下创建一个与账户同名的文件夹

**bin 目录:**  存放系统所需要的重要命令, 比如文件或目录操作的命令 ls, cp, mkdir 等, 另外 /usr/bin也放了一些系统命令, 这些命令对应着文件都是可以执行的

**sbin 目录:**  存放只有 root 超级管理员才能执行的程序

**boot 目录:**  存放着linux启动时内核及引导程序所需要的核心文件,内核文件和grub系统引导管理器都位于此目录

**dev 目录:** 存放linux系统下的设备文件, 如光驱等

**etc 目录:**  存放系统的配置文件,  作为一些软件启动时默认配置文件读取的目录, 如 etc/fstal存放系统分析信息

**mnt 目录:**  临时文件挂载目录, 也可以说是测试目录

**opt 目录:**  第三方软件存放目录

**medai 目录:**  即插即用型设备挂载点, 光盘默认挂载点, 通常光盘挂载于 /mnt/cdrom下

**tmp 目录:**  临时文件夹

**usr 目录:**  应用程序存放目录, 安装linux软件包是默认安装到 usr/local 目录下

**var 目录:**  目录经常变动 /var/log 存放系统日志,  var/log 存放系统库文件