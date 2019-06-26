### react

用于构建用户界面的javascript库

16之前采用diff算法，之后采用fiber算法，分成小块，异步渲染

安装：

```js
npx create-react-app react-tuts
```

转到`react-tuts`目录下

打开项目，删除public目录下的所有文件,新建`index.js`文件便于我们更好地 理解`react`的原理

这时输入`npm start`就可以打开项目

```js
import React from 'react' 
//只要使用jsx的语法都需要引入react，这是顶级api
import ReactDOM from 'react-dom'

ReactDOM.render(
  <h1>Welconme</h1>,	
    //jsx语法把h1渲染到id为root的文件中去
  document.querySelector('#root')
)
```

把jsx提成一个变量

```js
//可以理解为创建了一个简单的react元素
const app = <h1>Welconme</h1>

ReactDOM.render(
  app,
  document.querySelector('#root')
)
```

动态改变`jsx`元素的内容，并传递参数

```js
const createApp = (props) => {
    //在jsx里面写js语法需要加{}，只有这一种语法
  return <h1>Welconme{props.title}</h1>
}

const app = createApp({
  title: '16.8'
})

ReactDOM.render(
  app,
  document.querySelector('#root')
)
```

下面是使用组件的第一种方式使用箭头函数的原理

**函数式组件**

```js
//使用箭头函数，但名字要大写开始
const App = (props) => {
  return (
    <div>
      {/* 注释也是使用{} */}
      <h1>Welconme{props.title}</h1>
      <p>{props.title}</p>
    </div>
  )
}

ReactDOM.render(
  <App title="1901"/>,
  document.querySelector('#root')
)
```

**类组件**

```js
import React from 'react'
import { render } from 'react-dom'

//第二种方法，使用类继承react下的component
class App extends React.Component {
  render () {
    return  <h3>类组件!!!</h3>
  }
}

//render是react dom提供的一个方法，这个方法通常只会渲染一次
render(
  <App />,
  document.querySelector('#root')
)
```

- 传递参数

  ```js
  import React from 'react'
  import { render } from 'react-dom'
  
  //第二种方法，使用类
  class App extends React.Component {
    render () {
      return  (
        <div>
          <h3>类组件!!!</h3>
          {/* 使用this.props接受，因为渲染出来的是类的实例 */}
          <p>{this.props.desc}</p>  
        </div>
      )
    }
  }
  
  render(
    <App desc="类组件是继承React.components的"/>,
    document.querySelector('#root')
  )
    
  ```

  new一个实例

  ```js
  import React，{component} from 'react'
  import { render } from 'react-dom'
  
  //第二种方法，使用类
  class App extends Component {
    render () {
      return  (
        <div>
          <h3>类组件!!!</h3>
          {/* 使用this.props接受，因为渲染出来的是类的实例 */}
          <p>{this.props.desc}</p>
        </div>
      )
    }
  }
  //类组件渲染的原理
  // 因为app不是一个element所以必须要调实例的render方法
  const app = new App({
    desc:'类组件是继承React.Component的1'
  }).render()
  
  render(
    app,
    document.querySelector('#root')
  )
    
  ```

  - 在react6以前使用的是这种方式来创建一个类组件

  ```js
  React.createClass({
      render <h3>类组件!!!</h3>
  })
  ```

**jsx的原理**

- 嵌套组件

```js
import React from 'react'
import { render } from 'react-dom'

const Header = () => <h3>类组件!!!</h3>

//第二种方法，使用类
class App extends React.Component {
  render () {
    return  (
      <div>
        <Header/>
        {/* 使用this.props接受，因为渲染出来的是类的实例 */}
        <p>{this.props.desc}</p>
      </div>
    )     
  } 
}

const app = new App({
  desc:'类组件是继承React.Component的1'
}).render()

render(
  // 因为app不是一个element所以必须要调实例的render方法
  app,
  document.querySelector('#root')
)
  
```

- 虚拟`dom`树

```js
import React from 'react'
import { render } from 'react-dom'

//这里使用类的形式创建的组件这是jsx的语法，但不是合法的javascript代码
class App extends React.Component {
  render () {
    return  (
      <div classname="app" id="appRoot">
        <h3 classname="title">类组件!!!</h3>
        <p>类组件是继承React.Component的</p>
      </div>
    )
  }
}

//表示一个虚拟dom树
const appVDom = {
  tag: 'div',
  attrs: {
    className: 'app',
    id: 'appRoot'
  },
  children: [
    {
      tag: 'h3',
      attrs: {
        className: 'title'
      },
      children: [
        '类组件!!!'
      ]
    },{
      tag: 'p',
      attrs: null,
      children: [
        '类组件是继承React.Component的'
      ]
    }
  ]
}


render(
  document.querySelector('#root')
)
  

```

- 所以react在真正渲染的时候会把上面的代码成下面这个样子，这是合法的`js`代码

```js
class App extends Component {
  render () {
    return (
        // React.createElement是一个方法，用于创建元素，可以有很多参数，但前两个是固定的
        //第一个为标签名
        //第二个为标签的属性
      React.createElement(
        'div',
        {
          className: 'app',
          id: 'appRoot'
        }
      ),
      React.createElement(
        'h3',
        {
          className: 'title',
        },
        '类组件'
      ),
      React.createElement(
        'p',
        null,
        '类组件是继承React.Component的'
      )
    )
  }
}
```

**元素中的样式**

```js
import React, { Component } from 'react'
import { render } from 'react-dom'


class App extends React.Component {
  render () {
    const style = {color: '#f00'}
    return  (
     <div>
        //也可以写成这样
        //<h1 style={{color: '#f00'}}>样式</h1>
        //第一个{}告诉jsx这是一个js 第二个{}表示是一个对象
       <h1 style={style}>样式</h1>
     </div>
    )
  }
}

render(

```

- 也可以独立为一个文件

  ```js
  import React, { Component } from 'react'
  import { render } from 'react-dom'
  
  import './index.css'
  class App extends React.Component {
    render () {
      const style = {color: '#f00'}
      return  (
       <div>
         <h1>样式</h1>
         <ol>
          <li style={style}>使用style内联样式</li>
          <li className="has-text-red">使用class的方式，但在react中class要写成className</li>
         </ol>
       </div>
      )
    }
  }
  
  render(
    <App />,
    document.querySelector('#root')
  )
    
  
  ```

- 下载class包

  ```js
  npm i classnames --save
  ```

  ```js
  import React, { Component } from 'react'
  import { render } from 'react-dom'
  import classNames from 'classnames'
  
  import './index.css'
  class App extends React.Component {
    render () {
      const style = {color: '#f00'}
      return  (
       <div>
         <h1>样式</h1>
         <ol>
          <li style={style}>使用style内联样式</li>
          <li className="has-text-red">使用class的方式，但在react中class要写成className</li>
          <li className={classNames('a')}>动态添加className使用classNames</li>
         </ol>
       </div>
      )
    }
  }
  render(
    <App />,
    document.querySelector('#root')
  )
    
  ```

- style-component 把样式独立出来

  ```js
  npm i style-component
  ```

  ```js
  import React, { Component } from 'react'
  import { render } from 'react-dom'
  import classNames from 'classnames'
  import styled from 'styled-components'
  import './index.css'
  
  const Title = styled.h1`
    color:#ff0
  `
  
  class App extends Component {
    render () {
      const style = {color: '#f00'}
      return  (
       <div>
         <Title>样式</Title>
         <ol>
          <li style={style}>使用style内联样式</li>
          <li className="has-text-red">使用class的方式，但在react中class要写成className</li>
          <li className={classNames('a')}>动态添加className使用classNames</li>
          <li>styled-component的使用</li>
         </ol>
       </div>
      )
    }
  }
  render(
    <App />,
    document.querySelector('#root')
  )
    
  
  ```

**安装`ES7 React`插件**

这时就可以使用`rcc`快速创建类组件

或者使用`rfc`快速创建函数型组件





