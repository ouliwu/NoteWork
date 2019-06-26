#### 组件化项目的目录结构

新建一个`components`目录写一个`components`的`index.js`文件用来导出各个组件，然后写好需要的模块

并为每个模块写一个`index.js`文件

例如：

```js
import React, { Component } from 'react'

export default class index extends Component {
  render() {
    return (
      <div>
        <input type="text" /><button>添加</button>
      </div>
    )
  }
}

```

在`components`的`index.js`文件中引入

```js
import React, { Component } from 'react'

export default class index extends Component {
  render() {
    return (
      <div>
        <input type="text" /><button>添加</button>
      </div>
    )
  }
}
```

这时就能在'App.js'中引入模块，直接使用

```js
import React, { Component } from 'react'
import {
  TodoHeader,
  TodoInput,
  TodoList
} from './components'

export default class App extends Component {
  render() {
    return (
      <>
        {/* 多个组件组合的时候必须有一个根元素 */}
        <TodoHeader />
        <TodoInput />
        <TodoList />
      </>
    )
  }
}

```





```js
import React, { Component } from 'react'
import {
  TodoHeader,
  TodoInput,
  TodoList
} from './components'

export default class App extends Component {
  render() {
    return (
      <div>
        {/* 多个组件组合的时候必须有一个根元素 */}
        <TodoHeader />
        <TodoInput />
        <TodoList />
      </div>
    )
  }
}

```

**每个组件return的东西必须只有一个根元素**

**多个组件组合的时候，外层就需要一个`div`**

**但是加div会额外生成一个空的div，但有时候是需要这个`dom`的**

**如果不想要这个`div`，`react`提供了一个组件`fragment`，用来展示空标签**

```js
import React, { Component, Fragment } from 'react'
import {
  TodoHeader,
  TodoInput,
  TodoList
} from './components'

export default class App extends Component {
  render() {
    return (
      <Fragment>
        {/* 多个组件组合的时候必须有一个根元素 */}
        <TodoHeader />
        <TodoInput />
        <TodoList />
      </Fragment>
    )
  }
}
```

也可以写成空标签

```js
import React, { Component } from 'react'
import {
  TodoHeader,
  TodoInput,
  TodoList
} from './components'

export default class App extends Component {
  render() {
    return (
      <>
        {/* 多个组件组合的时候必须有一个根元素 */}
        <TodoHeader />
        <TodoInput />
        <TodoList />
      </>
    )
  }
}
```

- 使用prop传递参数

  ```js
  import React, { Component } from 'react'
  import {
    TodoHeader,
    TodoInput,
    TodoList
  } from './components'
  
  export default class App extends Component {
    render() {
      return (
        <>
          {/* 多个组件组合的时候必须有一个根元素 */}
          <TodoHeader 
           desc="今日事，今日毕"
          >
          {/* 传递子元素 props-children*/}
            待办事项
          </TodoHeader>
          {/* 向组件传递参数 */}
          <TodoInput btnText="Add"/>
          <TodoList />
        </>
      )
    }
  }
  
  ```

  - 使用函数式组件接受参数

    ```js
    import React from 'react'
    import PropTypes from 'prop-types'
    // 让组件变得刚强大，更方便开发人员检查
    
    export default function TodoHeader( props) {
      console.log(props)
      return (
        <>
        <h1>
          {/* 代表的是标签里面的内容 */}
          {props.children}
        </h1>
        <h3>
          {props.desc}
        </h3>
        </>
      )
    }
    
    ```

  - 使用组件类接受参数

    ```js
    import React, { Component } from 'react'
    import PropTypes from 'prop-types'
    export default class index extends Component {
      render() {
        return (
          <div>
            <input type="text" /><button>{this.props.btnText}</button>
          </div>
        )
      }
    }
    
    ```

    

- 使用prop-types检测传入的参数是否符合要求

```js
npm i prop-types --save
```

​     函数式组件设置默认值

```js
import React from 'react'
import PropTypes from 'prop-types'
// 让组件变得刚强大，更方便开发人员检查

export default function TodoHeader( props) {
  console.log(props)
  return (
    <>
    <h1>
      {/* 代表的是标签里面的内容 */}
      {props.children}
    </h1>
    <h3>
      {props.desc}
      <p>{props.x + props.y}</p>
    </h3>
    </>
  )
}

// eslint-disable-next-line react/no-typos
TodoHeader.propTypes = {
  desc: PropTypes.string,
  x: PropTypes.number.isRequired,
  y: PropTypes.number.isRequired
}
```

类组件设置默认值

```js
import React, { Component } from 'react'
import PropTypes from 'prop-types'
export default class index extends Component {
  static propTypes = {
    btnText: PropTypes.string
  }
  static defaultProps = {
    // 组件默认值传了的话就使用外面的，没传就使用默认的
    btnText: "添加Todo"
  }
  render() {
    return (
      <div>
        <input type="text" /><button>{this.props.btnText}</button>
      </div>
    )
  }
}

```





```js
import React, { Component } from 'react'
import {
  TodoHeader,
  TodoInput,
  TodoList
} from './components'

export default class App extends Component {
  render() {
    return (
      <>
        {/* 多个组件组合的时候必须有一个根元素 */}
        <TodoHeader desc="今日事，今日毕" x="1" y={2}>
        {/* 传递子元素 */}
          待办事项
        </TodoHeader>
        {/* 向组件传递参数 */}
        <TodoInput btnText="Add"/>
        <TodoList />
      </>
    )
  }
}

```

- props默认值

  - 函数式组件设置默认值

    ```js
    TodoHeader.defaultProps = {
      desc: '明天'
    }
    ```

  - 类组件设置默认值

    ```js
      static defaultProps = {
        // 组件默认值传了的话就使用外面的，没传就使用默认的
        btnText: "添加Todo"
      }
    ```

    state定义组件内部状态，有状态组件

    第一种初始化state的方式

    ```js
     state = {
        title: '待办事项列表1'
      }
    ```

    第二种初始化state的方式

    ```js
      constructor() {
        super()
        this.state = {
          title: '待办事项列表'
        }
      }
    ```

    16.8之前函数式组件没有this，也没有state

  ```js
  import React, { Component } from 'react'
  import {
    TodoHeader,
    TodoInput,
    TodoList
  } from './components'
  
  export default class App extends Component {
    // state = {
    //   title: '待办事项列表1',
    // }
  
    constructor() {
      super()
      this.state = {
        title: '待办事项列表',
        desc:'今日事，今日毕',
        article: '<div>jiagsgsg</div>',
        todos : [{
          id: 1,
          title: '学习',
          isCompleted: 'false'
        },{
          id: 2,
          title: '吃饭',
          isCompleted: 'true'
        }]
      }
    }
    render() {
      return (
        <>
          {
            <div dangerouslySetInnerHTML = {{__html:this.state.article}}/>
          }
          {
            this.state.todos.map(todo => {
              return <div key="todo.id">{todo.title}</div>
            })
          }
        </>
      )
    }
  }
  
  ```



```js
import React, { Component } from 'react'
import TodoItem from './TodoItem'
export default class TodoList extends Component {
  render() {
    console.log(this.props)
    return (
      <ul>
        {
          this.props.todos.map(todo => {
            return(
              // <TodoItem
              // key={todo.id}
              // id={todo.id}
              // title={todo.title}
              // isCompleted={todo.isCompleted}
              // />
              //使用对象展开的方法在这里就能取到更新的数据
              <TodoItem
                key={todo.id}
                {...todo}
              />
            )
          })
        }
      </ul>
    )
  }
}

```



```js
import React, { Component } from 'react'
import PropTypes from 'prop-types'
export default class index extends Component {
  static propTypes = {
    btnText: PropTypes.string
  }
  static defaultProps = {
    // 组件默认值传了的话就使用外面的，没传就使用默认的
    btnText: "添加Todo"
  }
  constructor () {
    super()
    this.state = {
      inputValue: ''
    }
  }
  handleInputChange = (e) => {
    this.setState({
      inputValue:e.currentTarget.value
    })
  }
  render() {
    return (
      <div>
        <input 
        type="text" 
        value={this.state.inputValue}
        onChange={this.handleInputChange}
        />
        <button>{this.props.btnText}</button>
      </div>
    )
  }
}

```

