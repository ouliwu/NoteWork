### fetch

原生做ajax请求的常用语手机、平板电脑开发

fetch会返回Promise，所以在获取资源后，可以使用.then方法做你想做的

```js
  //地址 返回值
         fetch('api/v1/signin',{
                 body:JSON.stringify(data),
                 headers:{
                 'content-type':'application/json'
                 },
                    method:'POST'
                })
                .then(response => response.json())
                .then(resp => {
                    if(resp.code!== 200){
                      msg.innerHTML =resp.data.errMsg;
                        msg.classList.add('error');
                        mag.classList.remove('success');
                    }else{
                        msg.innerHTML = resp.data.errMsg;
                        msg.classList.add('success');
                        msg.classList.remove('error');
                        window.location.href='/dashboard'
                    }
                })
```

- 查兼容性

  `https://caniuse.com`

### bcrypt

是一个跨平台的文件加密工具

`npm i bcrypt`

```js
const bcrypt = require('bcrypt');
const saltRounds = 10;
bcrypt.genSalt(saltRounds, function(err, salt) {
    bcrypt.hash(Password, salt, function(err, hash) {
        
    });
});
```

### express-session

- 该模块是express模块中间件，方便我们处理客户端的session
- cookie处理服务端
- express-session是针对nodejs express框架提供的一套session扩展
- secure设置为true只有https才传递到服务端，http是是不会传递的
- secret用来注册session id到cookie中，相当于一个密钥

`npm i express-session`

```js
var session = require('express-session');//session中间件
```

