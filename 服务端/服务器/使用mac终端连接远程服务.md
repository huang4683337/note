# 使用本地终端连接远程服务

## 格式

```shell
# ssh 用户名@ip地址 -p端口号    
$ ssh root@xxx.xx.xxx.xx -p 22
```



## 服务别名

**如果想要连接的远程服务器太多，IP太多会记不住，我们需要给我服务配置别名**



+ mac 下

  ```shell
  # 使用 vim 编辑 ssh 配置文件
  $ vim ~/.ssh/config
  
  # 配置别名
  # 服务器1
  Host 别名
      HostName IP地址
      Port 22
      User 用户名
  # 服务器2
  Host 别名
      HostName IP地址
      Port 22
      User 用户名
      
  # 使用
  $ ssh 别名
  ```

+ window 下

  ```shell
  # 安装 git 
  # 从开始菜单中打开 git base
  
  $ vim ~/.ssh/config
  
  # 添加以下内容
  Host 别名
      HostName IP地址
      Port 22
      User 用户名
    
  # 使用
  $ ssh 别名
  ```
  
  

