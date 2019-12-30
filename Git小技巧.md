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

