## 1、起步

安装：

```shell
$ npm install mysql --save
```

使用：

```javascript
var mysql = require('mysql');

// 1、创建连接
var connection = mysql.createConnection({
    host: 'localhost',
    user: 'root', 					// 用户
    password: 'root的密码',  // 用户权限密码
    database: 'my_db'  			// 你要连接的数据库
});

// 2、连接数据库
connection.connect();

// 3、执行数据操作
connection.query('此处写Sql语句', function (error, results, fields) {
    if (error) throw error;
    console.log('The solution is: ', results);
});

// 4、关闭连接
connection.end();
```



## 2、文档地址

>  在[npm](https://npms.io/)官网输入 mysql 点击第一个就是

[文档](https://github.com/mysqljs/mysql)

