#### Provider

`provider`是`react-redux`提供的一个高阶组件

使用Provider的简单步骤：

1. 创建reducers
2. 合并reducers
3. createStore
4. Provider store={store}
5. connect(mapStateTopProps,{...actionCreatos})(YourComponent)
6. actionCreators
7. 修改reducers



reducers/cart.js

```js
import actionType from '../actions/actionTypes'
const initState = [{
  id: 1,
  title: 'Apple',
  price: 12,
  amount:100
},{
  id: 2,
  title: 'Orange',
  price: 5,
  amount:160
}]
export default (state = initState, action) => {
  switch(action.type) {
    default:
      return state
  }
}
```



reducers/index.js

```js
//createStore的参数只能接受一个reducer，所以reduxt提供了一个合并reducer的方法，注意不要手动合并
import { combinReducers } from 'redux'
//引入cart reducer，如果有多个，继续引入
import cart from './cart'
//导出合并之后的reducers
export default combinReducers({
    //把多个reducers作为combineReducers对象参数传入，在外部就可以通过
    //store.getState().cart来获取cartReducer里面的state
    cart
}
```



store.js

```js
//ccreateStore是redux提供的一个用于常见store的方法
import { createStore } from 'redux'
//引入合并之后的reducer
import rootReducer from ‘./reducers’
//creatStore的第一个参数必须是一个reducer，如果有多个必须在reducer目录下使用combineReducer合并之后导出
export default creatStore(rootReducer)
```





index.js

```js
import { Provider } from 'react-redux'
```

```js
render(
    //一般把这个组件放在应用程序的最顶层,这个组件必须有一个store属性
    //只要在最外层包裹了这个Provider，那么所有后代组件都可以使用Redux.connect做连接
	<Provider store={store}>
    	<App />
    </Provider>
	document.querySelector('#root')
)
```





Provider的返回值是一个高阶组件，只要在最外层加了Provider之后，就可以在任何一个组件使用

```js
import React, { Component } from 'react'
import { connect } from 'react-redux'
//这时在组件里可以通过this.props得到数据

// 导入actionCreators
import { increment, decrement } from '../../actions/cart'


class CartList extends Component {
  render() {
    return (
      <table>
        <thead>
          <tr>
            <th>id</th> 
            <th>商品名称</th>
            <th>价格</th>
            <th>数量</th>
            <th>操作</th>
          </tr>
        </thead>
        <tbody>
          {
            this.props.cartList.map(item => {
              return (
                <tr key={item.id}>
                  <td>{item.id}</td>
                  <td>{item.title}</td>
                  <td>{item.price}</td>
                  <td>
                    <button onClick={this.props.decrement.bind(this, item.id)}>-</button>
                    <span>{item.amount}</span>
                    <button onClick={this.props.increment.bind(this, item.id)}>+</button>
                  </td>
                  <td></td>
                </tr>
              )
            })
          }
        </tbody>
      </table>
    )
  }
}

//这里这个state就是store.getState()的值
const mapStateToProps = (state) => {
    return {
        cartList:state.cart
    }
}

//const mapDispatchTOProps = dispatch => {
//    return {
//        add(id) => dispatch(increment(id)),
//        reduce(id) => dispatch(decrement(id))
//    }
//}


//第一个括号会生成一个state注入到组件中
//第一个括号第二个参数就相当于派发器
//export defalult connect(mapStateToProps, mapDispatchTOProps)(CartList)//这里调用之后是一个高阶组件


//如果有actionCreator可以这样使用
//这样就会自动具有dispatch的功能
export defalult connect(mapStateToProps, {increment, decrement})(CartList)
```

actions/actionTypes.js

为了避免actionType重复，所以一般会把actionType放在一个文件里统一进行管理，也可以避免写错actionType

```js
export default {
    CART_ITEM_INCREMENT: 'CART_ITEM_INCREMENT',
    CART_ITEM_DECREMENT: 'CART_ITEM_DECREMENT',
    
}
```

actions/cart.js

```js
import actionTypes from './actiontypes'
//actionCreator是一个方法，返回一个对象，方便动态传递参数
export const increment = (id) => {
    return {
        type:actionTypes.CART_ITEM_INCREMENT,
        payload: {
            id
        }
    }
}

export const increment = (id) => {
    return {
        type:actionTypes.CART_ITEM_DECREMENT,
        payload: {
            id
        }
    }
}
```

修改deducers在这里处理数据

```js
import actionType from '../actions/actionTypes'
//这里是一个初始化状态
const initState = [{
  id: 1,
  title: 'Apple',
  price: 12,
  amount:100
},{
  id: 2,
  title: 'Orange',
  price: 5,
  amount:160
}]
//reducer的固定写法是两个参数，第一个参数是state并有一个初始值，第二个参数是action
export default (state = initState, action) => {
    //根据不同的action.type做不同的处理，每次返回一个新的state,返回的数据类型要一样
  switch(action.type) {
    case actionType.CART_ITEM_INCREMENT:
      return state.map(item => {
        if(item.id === action.payload.id) {
          item.amount += 1
        }
        return item
      })
      case actionType.CART_ITEM_DECREMENT:
          return state.map(item => {
            if(item.id === action.payload.id) {
              item.amount -= 1
            }
            return item
          })
          //一定要有default当actionType不对的时候，就不做任何处理返回上一次的state
    default:
      return state
  }
}
```



```js

```

