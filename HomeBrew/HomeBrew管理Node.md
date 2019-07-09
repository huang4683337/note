```shell
$ brew search node  # 查看可用的node版本 打✅的是已安装版本

$ brew install node@6 #	安装需要的版本

$ brew install node6-lts # 安装6版本中 最新的版本
```



切换版本

```shell
$ brew unlink node # 取消当前版本

$ brew link node@6 [--force] # 切换到需要的版本  --force 强制执行

$ rm '/usr/local/include/node/pthread-fixes.h'  # 切换版本时可能删除一些不需要的文件
```



添加 path

```shell
$ which node # 查看node安装路径
$ export NODE_PATH='/usr/local/lib/node_modules' # <--- add this ~/.bashrc
```



删除所有 node

```shell
$ brew uninstall node
# 或者 `brew uninstall --force node` 移除所有版本
$ brew prune
$ rm -f /usr/local/bin/npm /usr/local/lib/dtrace/node.d
$ rm -rf ~/.npm
```

