[nginx参考文档](https://zhuanlan.zhihu.com/p/112501786)



##  windows

```shell
# 想要通过 nginx 访问 windows 下的某个目录

root		D:/dist;
```





```shell
nginx -v 	# 查看版本
nginx -V	# 查看安装路径
nginx -t	# 查看nginx配置文件位置
```



## 查找nginx.conf

```shell
# 打开访达

command + shift + g	

/usr/local/etc/nginx
```

```shell
nginx							# 启动
nginx -s stop			# 停止	
nginx -s reload		# 重启
nginx -s reopen		# 重新打开日志文件

#测试nginx配置文件是否正确
nginx -t -c /path/to/nginx.conf
```

