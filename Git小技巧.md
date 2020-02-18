## git add .  时报错

```shell
warning: LF will be replaced by CRLF in src/views/theme/decisionSupport/module/gisModel/traffic/controlTraffic/controlTraffic.vue.
The file will have its original line endings in your working directory

# warning: LF will be replaced by CRLF
```

```shell
#提交时转换为LF，检出时转换为CRLF
$ git config --global core.autocrlf true


#提交时转换为LF，检出时不转换
$ git config --global core.autocrlf input

#提交检出均不转换
$ git config --global core.autocrlf false

```



## git clone 时报错

```shell
ssh: Could not resolve hostname devops.wh.gsafety.com: Name or service not known
fatal: Could not read from remote repository

Please make sure you have the correct access rights and the repository exists.

# 出现这种问题的大部分原因都是 DNS 污染

# 首先我们需要拿到当前域名的 ip, 在命令窗口输入
$ ping devops.wh.gsafety.com;		# 172.18.7.24

# 打开 hosts 文件添加 ip 和域名
172.18.7.24	devops.wh.gsafety.com

# 重启电脑

# 问题解决
```

