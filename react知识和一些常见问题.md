##  react知识和一些常见问题

##### 1.什么是声明式编程

**声明式编程**关注的是你要做什么，而不是如何做。声明式编程很容易推理，因为代码本身描述了它在做什么

例如：

```js
const num = [1,2,3,4,5];

// 声明式
// 使用map函数，让编译器来完成
const doublenum = numbers.map(num => num * 2);

console.log(doublenum)

// 命令式
// 需要写出所有的流程步骤
const doubleArr= [];
for(let i=0; i<numbers.length; i++) {
    const doublenum = numbers[i] * 2;
    doubleArr.push(doublenum)
}

console.log(doubleArr)
```

##### 2.什么是函数式编程

特点：

- 不可变性(Immutability)
- 纯函数
- 数据转换
- 高阶函数
- 递归
- 组合



##### 不可变性

表示数据不可更改，也无法更改，如果要改变数据，则必须复制数据的副本

例如：

```js
const state = {
    name: 'lia',
    age: 18
    like: 'eat'   
}

const copiedState = Object.assign({}, state)
copiedState.age = 20
```



##### 纯函数

始终接收一个或多个参数并计算参数，返回数据或函数

没有副作用

例如：

使用非纯函数，是直接修改的state

```js
const state = {
    name: 'lia',
    age: 18,
    like: 'eat'   
}

function appendAddress() {
    state.address = 'chengdu'
}
appendAddress()

console.log(state)
```

使用纯函数，不会改变原来的state

```js
const state = {
    name: 'lia',
    age: 18,
    like: 'eat'   
}

function appendAddress(state) {
  const copyState = Object.assign({}, state);
  copyState.address = 'chengdu'
  return copyState
}
console.log(appendAddress(state))
console.log(state)
```



##### 高阶函数

高阶函数是将函数作为参数或者返回函数的函数

`Array.map`，`Array.fileter`，`Array.reduce`都是高阶函数

```js
const Young = age => age < 25
const message = msg => 'she is' + msg
function isYoung(age, Young, message) {
  const returnMessage = Young(age) ? message("young") : message("old")
  return returnMessage
}

console.log(isYoung(18,Young,message))
```



##### 递归

递归是一种函数满足一定条件之前自身调用的技术

*注意*：浏览器不能处理太多递归和抛出错误

```js
function print(str, count) {
  if(count <= str.length) {
    console.log(str.substring(0, count))
    print(str, ++count)
  }
}

console.log(print("abcdefghijk",1))
```

#####  组合

将功能划分为小型可重用的纯函数，，将所有较小的纯函数组合在一起成更大的函数，最终得到一个应用程序，称为组合

##### 什么是React

React是一个简单的`javascript UI`库，用于构建高效、快速的用户界面，是一个轻量级的库，遵循组件设计模式、声明式编程和函数式编程概念，使用虚拟`Dom`树操作`Dom`，遵循从高阶组件都低阶组件的单向数据流

##### React 和 Angular有什么不同

Angular是一个成熟的MVC框架，有很多特定的特性，如服务、指令、模板、模块、解释器等。

React是一个非常轻量级的库，只关注MVC的视图部分

Angular遵循两个方向的数据流，React遵循从上到下的单向数据流，在开发特性时给开发人员很大的自由，例如：调用API方式、路由等。

