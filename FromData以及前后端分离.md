![](C:\Users\qianfeng\Desktop\1.png)

##### FormData

对象用以将数据编译成键值对，以便用`XMLHttpRequest`来发送数据。其主要用于发送表单数据

处理FromData可以使用multer和fomidable

`npm i formidable -S`

```js
 <h1><%= title %></h1>
    <p>Welcome to <%= title %></p>

    <div class="avatar">
      <img src="" alt="" id="avatarImg">
    </div>
    <label class="upload-fields">
      <span class="btn">选择文件</span><span class="filename"></span>
      <input type="file" id="avatar" onchange="handleFileInputChange(this)"/>
    </label>
    <button onclick="uploadFile()">上传</button>
    <script>
      const handleFileInputChange = (e) => {
        document.querySelector('.filename').innerHTML = e.files[0].name
      }

      const avatarImg = document.querySelector('#avatarImg')
      // 这个方法里的内容必须掌握
      const uploadFile = () => {
        // FormData是一个对象，用于ajax上传文件
        // 首先需要先new一个实例
        const data = new FormData()
        // 使用append方法添加字段 formDataInstance.appen(key, value)

        // 这里是获取input:file里的文件
        const avatar = document.querySelector('#avatar').files[0]
        // 这里添加要上传的文件，注意这里的key就是后端所需要的参数名
        data.append('avatar', avatar)

        // 这里添加除了文件之外的其它字段
        data.append('name', 'LEO')

        // 使用ajax上传，这里使用了fetch，也可以使用jQuery，也可以使用其它任何的ajax库，比如后期咱们工作天天用的axios
        fetch('/api/v1/upload', {
          method: 'post',
          body: data
        })
        .then(resp => resp.json())
        .then(json => {
          console.log(json)
          if (json.code === 200) {
            avatarImg.setAttribute('src', json.data.url)
          }
        })
      }
```



###### 基于express和mock.js搭建自己的前后端分离Mock服务器

1. 运行`$npx express-generator api-server`创建一个express项目

2. `$ cd api-server`进入项目目录

3. `$ npm install`安装项目所需要的依赖

4. `$ npm install nodemon -D`安装[nodemon](<https://www.npmjs.com/package/nodemon>)

5. `$ npm install mockjs -S`安装[mockjs](<http://mockjs.com/>)包 

6. 打开项目目录下的`package.json`,  更改`scripts`: 

   ```sh
   - "start": "node ./bin/www"
   + "start": "nodemon ./bin/www"
   ```

7. 根据需要配置路由

8. 比如，有一个叫users的路由挂载在`/api/v1/users`下，就可以这么来写这个mock数据

   ```js
   // 引入express
   const express = require('express');
   // 只使用router
   const router = express.Router();
   // 引入Mock对象
   const Mock = require('mockjs')
   
   
   // 定义生成数据列表的方法
   const generateData = (limited=10,offset=0) => {
     // 使用Mock.mock方法来生成mock数据
     return Mock.mock({
       "code": 200,
       "data|12": [
         {
           "id": "@id",
           "title": "@ctitle(15, 25)",
           "author": "@cname",
           "volume": "@int(100, 300)",
           "createAt": "@int(10000000000000, 1554363040517)"
         }
       ]
     })
   }
   
   // 定义另外一个方法，用于生成单个数据
   const generateDataById = (id) => {
     return Mock.mock({
       "code": 200,
       data: {
         id,
         "title": "@ctitle(15, 25)",
         "author": "@cname",
         "volume": "@int(100, 300)",
         "createAt": "@int(10000000000000, 1554363040517)"
       }
     })
   }
   /* 获取用户列表 */
   router.get('/', function(req, res, next) {
     res.json(generateData())
   });
   /* 获取单个用户，根据用户的id, 这里有一个express通配符路由(动态路由) */
   router.get('/:id', function(req, res, next) {
     const  {
       id
     } = req.params
     //params获取路由里面的参数id
     res.json(generateDataById(id))
   });
   
   module.exports = router;
   
   ```

   这样我们就可以使用 `http://localhost:port/users`获取用户列表, 使用 `http://localhost:port/users/任意的id参数`获取用户信息

   ```js
   const generatorData =(limited = 10,offset = 0)=>{
     //使用Mock.mock方法生成mock数据
     return Mock.mock({
       code :200,
       data:{
           //当前页数
         currentPage:(offset/limited)+1,
         isLastPage:false,
         tatal:20,
         //[表达式]
         [`list|${limited}`]:[
           {
             "id":"@id",
             "title":"@ctitle(15,25)",
             "author":"@cname",
             "volume":"@int(100,300)",
             "createAt":"@int(1110000000,155937335)"
           }
         ]
       }
     })
   }
   //定义另一个方法用于生成单个数据
   const generatorDataById=(id)=>{
     return Mock.mock({
       "code" :200,
       data:
         {
           id,
           "title":"@ctitle(15,25)",
           "author":"@cname",
           "volume":"@int(100,300)",
           "createAt":"@int(1110000000,155937335)"
         }
       
     })
   }
   router.get('/',function(req,res,next){
     const{
       limited = 10,
       offset = 0
     }=req.query
     //获取？后的参数
     res.json(generatorData(limited,offset));
   })
   
   //相当于详情传id就显示id相对应的数据
   router.get('/:id',function(req,res,next){
     const{
       id
     }=req.params
     res.json(generatorDataDataById(id));
   })
   ```

   

- `browser sync`

​	静态网页想在服务器运行，实现实时刷新的方法

​	`ctrl+shift+p` 启动一个端口

```js
 <input tytpe="email" name="emial" oninput="handelchange(this)">
    <input type="text" name="username" oninput="handelchange(this)">
    <button onclick="btn()">提交</button>

    <script>
        const handelchange=(e)=>{
            //方法名如果是一个变量加中括号[变量]
           state[e.getAttribute('name')]=e.value;
        }
        const state={
            username:'',
            email:''
        }

        const btn=()=>{
            console.log(state);
        }
    </script>
```

