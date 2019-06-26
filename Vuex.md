#### Vuex

是一个专为 Vue.js 应用程序开发的**状态管理模式**,采用集中式存储管理应用的所有组件的状态，并以相应的规则保证状态以一种可预测的方式发生变化,vuex可以帮助我们管理共享状态，适合开发大型单页应用

Vue Components 发出(Dispath)一个Actions commit(提交)到Mutations发出Mutate改变状态State再重新渲染(Render)Vue Components

**安装vuex**

```js
npm install vuex
```

在mian.js中引入vue

```js
import vue from 'vue'
import vuex from 'vuex'
vue.use(vuex)
```

每一个 Vuex 应用的核心就是 store（仓库）。“store”基本上就是一个容器，它包含着你的应用中大部分的**状态 (state)**

**getter**

`getter`是没有副作用的

`Vuex `允许我们在 store 中定义“getter”（可以认为是 store 的计算属性）。就像computed一样，getter 的返回值会根据它的依赖被缓存起来，且只有当它的依赖值发生了改变才会被重新计算。

**Mutation**

mutations是唯一一个可以更改state的地方，同时必须同步修改数据

```js
mutations: {
    //可以向store.commit提供额外的参数，即载荷payload
    increment (state, n=1) {
        state.count += n
    }
}
```

```js
methods: {
    CountIncrement() {
        this.$store.commit('increment')
    }
}
```

可以写成对象，这样可以包含多个字段并且mutation会更容易读

```js
mutations: {
    increment (state, payload) {
        state.count += payload.amount
    }
}
```

```js
store.commit('increment',{
    amount: 10
})
```



**mapState辅助函数**

```js
import {
    mapState
}from 'vuex'
export default {
    computed () {
        //使用对象展开运算符将此对象混入到外部对象中
        ...mapState({
            
        })
    }
}
```

### 使用常量替代 Mutation 事件类型

可以把这些常量放在单独的文件中可以让整个app的mutation一目了然

新建文件mutationType.js

```js
export const SOME_MUTATION = 'SOME_MUTATION'
```

**Action**

可以做异步操作

```js
store.dispatch('increment')

actions: {
    incrementAsync({ commit }) {
        setTimeout(() => {
            commit('increment')
        },1000)
    }
}
```

**开发环境严格模式**

```js
  strict: process.env.NODE_ENV !== 'production'
```

**全局混入**

全局注册一个混入，影响注册之后所有创建的每个 Vue 实例。

```js
//mutation.js中定义
Vue.mixin({
  filters: {
    num100 (value) {
      if (!Number.isInteger(value)) {
        return value
      } else {
        return value > 99 ? '99+' : value
      }
    }
 })
```

```js
//在需要的页面中引入
import { mapMutations } from 'vuex'

<span class="circle" v-if="item.path === '/cart'">{{totalCartCount | num100}}</span> 
//使用竖线分隔
```



