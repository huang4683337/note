#### ssh key 存在，无法提交到github

```shell
# window
# C:\Users\Amd\.ssh 文件下删除 known_hosts 文件，重新 git pull 

# window 重新生成 ssh
$ ssh-keygen -t rsa -C 'huang19940214@163.com'
```

