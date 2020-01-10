# 远程管理

+ putty  只有命令行窗口
+ SSHSecureShellClient  集成两者的功能
+ WinSCP  可以直接将本地文件拖入linux , 或将linux文件拖入本地



## 连接远程服务器

```shell
$ ip addr # 获取linux ip地址

# 如果看不到就是没开网络
```



##  如果安装系统时忘记开启网络

```shell
$ vi /etc/sysconfig/network--scripts/ifcfg-ens33

# 进入文件后找到
ONBOOT = 'NO'  # 将值改为 yes

# 重启网络
$ service network restart
```



## 开启网络后还不能连接远程

```shell
# 在虚拟机设置中查看设备状态是不是 NAT 模式
```

