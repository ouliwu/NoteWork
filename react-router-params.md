#### react-router

component优先但不能传参，render以传参

render和component是排他的

动态路由传递的参数都在props里

state用于隐式传参

1.`query`

2.动态路由` /path/:params => params`

3.使用`state`

#### 埋点

是网站分析的一种常用的数据采集方法

埋点的作用就是用来做动作行为分析的

在产品流程关键部位植相关统计代码，用来追踪每次用户的行为，统计关键流程的使用程度。

#####  向后端发送数据

1.`ajax` 成功率最差

2.`img:`兼容性最好 只能处理get请求 成功率高 数据会丢失10%-15%

```js
const img = new Image()

img.src="http://www.dominname.vom/button-01.gif?x=1&y=2"
```



3. `navigator.sendBeacon()`兼容性不高 数据不会丢失 

```js
this.props.history.push('/home')

this.props.history.push({

	

})
```



`withRouter`高阶组件

只有使用`router`渲染的组件才有`route`属性，

否则只能使用`withRouter`



####  Ant Design（antd）UI组件库

```js
npm i antd -S
npm i react-app-rewired customize-cra
```



```js
npm i less less-loader -D
```

