# Node综合Web案例

## 1、目录结构

```shell
|-- app.js							入口js文件
|-- contorllers
|-- models							设计的数据库模型
|-- node_modules				第三方包
|-- package.json				包描述文件
|-- package-lock.json		第三方包版本锁定文件（npm 5版本以后才有）
|-- public							公共的静态资源
|-- README.md						项目说明文档
|-- routes							路由目录
|-- views								视图目录
```



## 2、模板页

[art-template](http://aui.github.io/art-template/zh-cn/docs/)

## 3、路由设计

| 路径      | 方法 | get参数 | post参数                  | 是否需要登录权限 | 备注         |
| --------- | ---- | ------- | ------------------------- | ---------------- | ------------ |
| /         | GET  |         |                           |                  | 渲染首页     |
| /register | GET  |         |                           |                  | 渲染注册页面 |
| /register | POST |         | email、nickname、password |                  | 处理注册请求 |
| /login    | GET  |         |                           |                  | 渲染登陆页面 |
| /login    | POST |         | email、password           |                  | 处理登陆请求 |
| /logout   | GET  |         |                           |                  | 处理退出请求 |



## 4、模型设计



## 5、功能实现