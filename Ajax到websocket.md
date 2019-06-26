## Ajax到websocket

#### ajax是什么

全称是（asynchronous javascript and xml）是已有技术的组合，主要用来实现客户端与服务器的异步通信效果，实现页面的局部刷新，早期的浏览器并不能原生支持ajax，可以使用隐藏帧（iframe）方式变相实现异步效果

#### ajax的创建

- 创建xhr对象，也就是创建一个异步调用对象
- 创建一个新的HTTP请求，并指定该HTTP请求的方法、URL及验证信息
- 设置回调
- 发送HTTP请求
- 获得异步调用返回的数据
- 使用JavaScript和DOM实现局部刷新

```js
var xhr = null // 创建对象
if(window.XMLHttpRequest) { // 标准浏览器
    xhr = new XMLHttpRequest()
} else {// 早期浏览器
    xhr = new ActiveXObject('Microsoft.XMLHTTP') // 参数是规定的
}
xhr.open('方式','地址','标志位') // 初始化请求
xhr.setRequestHeader('','') // 设置http头请求
xhr.onreadystatechange = function(){
    // 4表示响应内容解析完成，可以在客户端调用了
    // 200表示HTTP请求成功
     if(xhr.readyState === 4 && xhr.status === 200) {
        console.log(xhr.responseText)
    }
} // 指定回调函数
xhr.send() // 发送请求
```

#### ajax最大的特点

ajax可以实现异步通信效果，实现页面局部刷新，带来更好地用户体验，按需获取数据，带来更好地用户体验

#### ajax的缺点

1. ajax不支持浏览器back按钮
2. 安全问题ajax暴露了与服务器交互的细节
3. 对搜索引擎的支持比较弱
4. 破坏了程序的异常机制

#### JSOP

Jsonp并不是一种数据格式，json才是数据格式，jsonp是用来解决跨域获取数据的一种解决方案，通过动态创建script标签，然后通过标签的src属性获取js文件中的js脚本，该脚本中的内容是一个函数调用，参数就是服务器返回的数据，为了处理这些返回的数据，需要事先在页面定义好回调函数，本质上使用的并不是ajax技术

**利用`<script>`在引入外部js时不受同源策略限制的特性，来实现跨域**

```js
<script src=""></script>
```



**使用jquery的getJSON:**

```js
$.getJSON('http://www.baidu.com?callback=?', function(result){
    console.log(result)
})
```

**使用$.ajax发送Jsonp:**

```js
$.ajax({
    url:'http://www.baidu.com?name=zhangsan',
    dataType: 'jsonp',
    success: function(name){
        console.log(name)
    }
})
```

#### SSE(Server-Sent Events)服务器推送

ajax和JSONP都是client-fetch的操作，但有时候，我们需要服务器主动给我们发信息。和websokect不同，它依赖于原生的HTTP，比如在node.js中只要不执行res.end()，并且一定时间持续发送信息的话，那么该连接就会持续打开(keep-alive)

##### 	SSE和AJAX的区别

- 数据类型不同：SSE只接受type/event-stream类型，AJax可以接受任意类型
- 结束机制不同：虽然AJAX长轮询也可以实现这样的效果，但以node.js为例，必须在一定时间内执行res.end()才行。而SSE只需要执行res.write()即可

#### 服务器使用SSE

由于使用的是http协议，所以服务器基本上没有什么太大的改变

唯一注意的是发送数据使用res.write()即可，断开的时候使用res.end()

#### WebSokect

不同于其他HTTP协议，它是独立于HTTP存在的另一种通信协议，通常的通信并不会传输大量的内容，对于HTTP协议来说，通信时需要传递，cookie和request Headers这种方式的通信协议会造成延时，而websocket不会，并且支持双向通信

##### 发送方式

```js
socket.send('Hello world')
socket.send(JSON.stringtify('msg': 'welcome'))

var buffer = new ArrayBuffer(128)
socket.send(buffer)

var intview = new Unit32Arraay(buffer)
socket.send(intview)

var blob = new Blob([buffer])
sokect.send(blob)
```

##### 使用WebSocket跨域

设置相应的头：`Access-Control-Allow-Origin:example.com`。这时只有example.com可以进行跨域请求，其他的都会deny



#### fetch

号称ajax的替代品，它的API是基于Promise设计的，旧版本浏览器不支持Promise

```js
//原生XHR
var xhr = new XMLHttpRequest()
xhr.open('GET', url)
xhr.onreadystatechange = function() {
    if(xhr.readyState === 4 && xhr.status === 200) {
        console.log(xhr.responseText)
    }
}
xhr.send()

//fetch
fetch(url)
    .then(response => {
    if(resopnse.ok) {
        response.json()
    }
})
.then(data => console.log(data))
.catch(err => console.log(err))
```

#### axios

```js'
function getUserAccount() {
  return axios.get('/user/12345');
}

function getUserPermissions() {
  return axios.get('/user/12345/permissions');
}
```

