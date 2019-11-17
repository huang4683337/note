

# mongoose

+ [mongodb官方配置的原始包](https://www.npmjs.com/package/mogodb)
+ [官网](http://mongoosejs.com)
+ [官方指南](http://mongoosejs.com/docs/guide.html)
+ [官方API](http://mongoosejs.com/docs/api.html)
+ [mongoose基于mongodb包的在次封装的](https://github.com/mongodb/node-mongodb-native)



## 1、MongoDB数据库的基本概念

+ 首先要有数据库（可以有多个数据库）
  - 集合（sql中叫做表，一个数据库中可以有多个数据表）
    * 文档（sql中叫做表记录，一个数据表中可以存多个文档）

> 文档结构很灵活，无任何限制, 不需要像 `sql` 那样建库、表等。`MongoDB` 只需要指定往哪个数据库哪个集合中插入就行，剩下的就交给 `MongoDB` 来处理了。

```javascript
{
  qq数据库:{
    user数据表:[
      {}  // 一个对象就是一个文档
    ],
  },
  taobao数据库:{
    
  }
}
```

## 2、MongdoDB基本命令

```shell
$ use 数据库名称		# 切换到指定的数据（如果没有就会新建）

$ show dbs  # 查看所有的数据库

$ db 	# 查看当前操作的数据库

$ show collections	# 显示当前数据库中的集合（类似关系数据库中的表）

$ db.集合名字.find()	# 查看文档数据

$ db.dropDatabase()	# 删除当前使用数据库
```



==**mongoose中所有的方法都能使用promise**==



## 3、起步

安装：

```shell
$ npm i mongoose	# 安装 mongoose 三方库
```

使用：创建helloword.js

```javascript
const mongoose = require('mongoose');

mongoose.connect('mongodb://localhost:27017/test', { useNewUrlParser: true });

const Cat = mongoose.model('Cat', { name: String });

const kitty = new Cat({ name: 'Zildjian' });
kitty.save().then(() => console.log('meow'));
```



## 4、官方指南

### 4-1、设计Schema、发布model

```javascript
// 引入 mongoose
const mongoose = require('mongoose');

// 引入 Schema 用于设计表结构
const Schema = mongoose.Schema;


// 1- 链接数据库 test， test不存在时当我们添加第一条数据时会自动创建
mongoose.connect('mongodb://localhost/test', {useNewUrlParser: true});

// 2- 设计集合结构（表结构）
// 添加约束required 是为了保证数据的完整性，避免产生脏数据
let userSchema = new Schema({
    userName: String, // 字段为：userName  类型为：string  未添加约束
    name: {  // 字段为：name  类型为：String  默认值为：名字
        type: String,
        default: '名字',
        required: true, // 必填
    },
    password: {
        type: Number,
        default: 111111,
        enum: [0, 1], // 枚举
    }
})



// 3- 将文档结构发布为模型
/*
mongoose.model 将一个架构发布为模型

参数1: 大写的、代表单数的字符串来表示数据库名称 ==> User; mongoose会自动将其转换成小写的、表示复数的字符串 ==> users

参数2: 自己定义的架构 blogSchema
*/

const User = mongoose.model('User', userSchema);


//4- 可以开始操作数据库了
```

### 4-2、增加

```javascript
const admin = new User({
    userName: 'admin',
    name: '管理人员',
    password: 123456
})

admin.save((err, ret) => {
    if (err) {
        console.log(err);
    } else {
        console.log(ret);
    }
})
```

### 4-3、查询

```javascript
// 查询所有
User.find((err, ret)=>{
  if(err){
		console.log(err);
  }else{
    console.log(ret); //查询所有
  }
});

// 条件查询
User.find({userName: 'admin'},(err, ret)=>{
  if(err){
		console.log(err);
  }else{
    console.log(ret); 
  }
});
```

### 4-4、修改

```javascript
// 根据id修改
User.findByIdAndUpdate('id',{
  password:1111,
},(err, ret)=>{
  	console.log(ret);
})
```

### 4-5、删除

```javascript
// 删除一条
User.deleteOne({userName: 'admin'},(err)=>{
               
})


// 删除匹配的
User.deleteMany({ userName: /admin/, age: { $gte: 18 } }, (err)=>{
  
});


//根据条件删除一个
Model.findOneAndRemove(conditions,[options],[callback])

// 根据条件删除所有
Model.remove({
  age:10
},(err, ret)=>{

})

// 根据 id 删除一个
Model.findByIdAndRemove(id,[options],[callback])
```



