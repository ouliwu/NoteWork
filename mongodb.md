# MongoDB

​	MongoDB非关系型数据库 默认端口号为27017

​	`sequelize` 用于关系型数据库 例如：`mysql`

- `sudo mongod`启动服务

- `mongo` 用来操作 相当于git bash

- `show dbs` 查看所有数据库

- MongoDB中默认的数据库为test,如果你没有创建新的数据库，集合将存放在test数据库中

- 插入文档

- ```js
  db.insert({
      name:'菜鸟教程'，
      age :18
  })
  ```

- 

#### `express` 连接 `mongodb`

​	`npm i mongoose -S`

​	新建文件 `db.js`

​	`app.js `里连接` require('./db.js')`

​	schema定义文档数据结构

​	新建models文件  User.js(直接导出models实例就小写)

```js
module.exports = new model('user',userschema)
```

- user视图通过routes下的users渲染

```js
const usermoudle = require("../modules/users")
```

- `user.save()`保存到数据库中

- 查所有

```js
const users = UserModel.find({})
//UserModel.find({age:{$gte:24}})大于等于
.then(resp=>{//返回前端
    res.json(resp)
})
.catch(()=>{//错误返回的代码
    res.json({
        
    })
})
```

- 改数据

`Edit document`

- 返回视图

`res.render('userlist')`

###### 让接收的数据以列表的形式显示

- 在routes的users.js中传入一个对象

```.js
.then(resp=>{
    res.render('userlist',{users:resp})
})
```

- 在`userlist.ejs`中

```js
<ul>
    <%users.forEach(function(user){%>
    	<li>
    	<span><%=user.name%></span>
        <span><%user.age%></span>
         </li>
<%});%>
```

## ejs

- `<%=` 输出数据到模板

- `<%-` 输出非转义的数据到模板
- `include` 将相对于模板路径中的模板片段包含进来

## mongoose

Mongoose的一切都基于Schema

- 查询 Tank.find({size:'small'});
- 删除  Tank.remove();
- 可以通过mongoose.connect()方法连接MongoDb

```js
var mongoose = require('mongoose'),
    DB_URL = 'mongodb://localhost:27017/test';
mongoose.connect(DB_URL);
```

- `connection`是mongoose模块的默认引用，返回一个connection对象

  ```js
  var db = mongoose.connection;//获取connection实例
  //使用Connetion监听连接状态
  db.on('connected',function(err){
      if(err){
          console.log('连接数据库失败：'+err);
      }else{
          console.log('连接数据库成功！');
      }
  });
  ```

  - schema

    schema可以理解为mongoose对表结构的定义(不仅仅可以定义文档的结构和属性，还可以定义文档的实例方法、静态模型方法、复合索引等)，每个schema会映射到mongodb中的一个collection，schema不具备操作数据库的能力

    ```js
    var studentsSechma=new mongoose.Schema({
        name:String,
        age:Number
    })
    ```

    - 实例方法
       实例方法就是给model的实例使用的方法，在Schema上使用methods定义

    ```
    var mongoose=require("mongoose");
    mongoose.connect("mongodb://localhost/test");
    var animalSchema=new mongoose.Schema({
        name:String,
        type:String
    });
    //Schema定义的方法,model的实例就可以直接使用
    animalSchema.methods.findSimilarType=function (cb) {
        //那个实例调用的，这个this就指向谁
        this.model("Animal").find({type:this.type},cb)
    }
    var Animal=mongoose.model("Animal",animalSchema);
    
    var dog=new Animal({
        name:"小狗",
        type:"dog"
    });
    
    dog.save();
    
    dog.findSimilarType(function (err,result) {
        console.log(result);
    })
    ```

    - 静态方法
       静态方法是给model使用的，在Schema上使用statics定义

    ```
    var mongoose=require("mongoose");
    mongoose.connect("mongodb://localhost/test");
    var animalSchema=new mongoose.Schema({
        name:String,
        type:String
    });
    //Schema定义的静态方法
    animalSchema.statics.findSimilarType=function (type,cb) {
        //这里的this指的是model
       this.find({type:type},cb)
    }
    var Animal=mongoose.model("Animal",animalSchema);
    
    var dog=new Animal({
        name:"小狗",
        type:"dog"
    });
    Animal.findSimilarType("dog",function (err,result) {
        console.log(result);
    })
    ```

    - Model

      定义好了Schema，接下就是生成Model。
       model是由schema生成的模型，可以对数据库的操作

      ```
      var mongoose=require("mongoose");
      mongoose.connect("mongodb://localhost/test");
      var animalSchema=new mongoose.Schema({
          name:String,
          type:String
      });
      var Animal=mongoose.model("Animal",animalSchema);
      ```

      

  