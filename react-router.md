react-router

```js
npm i react-router-dom -S
```

使用router要在index.js里包一层Router

```js
//HashRouter
import { BrowserverRouter as Router, Route} from 'reat-router-dom'
```

只有最顶级需要router

```js
render(
<Router>
    <Route component={App} path='/' />
</Router>
)
```



```js 
import {route, NavLink as Link,Redirect}
```



Link

```js
<li><Link to="/hmoe">首页</Link></li>
```

NavLink as Link 会加active

Redirect

```js
import { BrowserRouter as Router, Route,Redirect} from 'reat-router-dom'
```

Switch匹配到一个就不会往下匹配

```js
<Switch>
    <Route component={Small} path='/small' />
    <Route component={Atical} path='/artical' exaact />
    <Route component={Atical
                     detail} path='/artical/:id' />
    
    <Route component={Home} path='/' />
    <Redirect to='/home' from='/'/>
</Switch>
    
```

##### withRouter

作用：把不是通过路由切换过来的组件，将`react-router`的`history`、`location`、`match`三个对象传入到props对象上

默认情况下必须是经过路由匹配渲染的组件才存在`this.props`,才拥有路由参数，才能使用编程式导航的方法，执行`this,props.history.push('/detail')`跳转到对应的页面

但是不是所有的组件都直接与路由相连(通过路由跳转到此组件)的，当这些组件需要路由参数是，使用`withRouter`就可以将此组件传入路由参数，此时就可以使用`this.props`

使用方法：

1.引入

```js
import { withRouter } from 'react-router-dom'
```





2.将组件`withRouter()`一下

```js
export default withRouter(ArticleList)
```

