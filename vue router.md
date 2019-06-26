#### vue router

##### 安装

```js
npm install vue-router
```

```js
import Vue from 'vue'
import VueRouter from 'vue-router'

Vue.use(VueRouter)
```



##### 全局前置守卫

router.beforeEach

每个守卫方法接收三个参数：

- **to: Route**: 即将要进入的目标 [路由对象](https://router.vuejs.org/zh/api/#%E8%B7%AF%E7%94%B1%E5%AF%B9%E8%B1%A1)
- **from: Route**: 当前导航正要离开的路由
- **next: Function**: 一定要调用该方法来 **resolve** 这个钩子。

```js
const router = new VueRouter({ ... })

router.beforeEach((to, from, next) => {
  // ...
})
```

##### 路由独享的守卫

你可以在路由配置上直接定义 `beforeEnter` 守卫：

```js
const router = new VueRouter({
  routes: [
    {
      path: '/foo',
      component: Foo,
      beforeEnter: (to, from, next) => {
        // ...
      }
    }
  ]
})
```

##### 组件内的守卫

```js
const Foo = {
  template: `...`,
  beforeRouteEnter (to, from, next) {
    // 在渲染该组件的对应路由被 confirm 前调用
    // 不！能！获取组件实例 `this`
    // 因为当守卫执行前，组件实例还没被创建
  },
  beforeRouteUpdate (to, from, next) {
    // 在当前路由改变，但是该组件被复用时调用
    // 举例来说，对于一个带有动态参数的路径 /foo/:id，在 /foo/1 和 /foo/2 之间跳转的时候，
    // 由于会渲染同样的 Foo 组件，因此组件实例会被复用。而这个钩子就会在这个情况下被调用。
    // 可以访问组件实例 `this`
  },
  beforeRouteLeave (to, from, next) {
    // 导航离开该组件的对应路由时调用
    // 可以访问组件实例 `this`
  }
}
```

##### 路由元信息

```js
 meta: {
      'title': '我的',
      'isTabbar': true,
      'iconName': '&#xe616;',
      'isTitle': true,
      'isAuthRequired': true
    }
//根据路由元信息来判断是否显示页面
router.beforeEach((to, from, next) => {
  if (to.meta.isAuthRequired === true) {
    if (store.getters.isLogin) {
      next()
    } else {
      next({
        name: 'login',
        params: {
          //这里的from是自定义方法
          from: to.path  
        }
      })
    }
  } else {
    next()
  }
})
```

##### router-link

通过名称来标识一个路由

```js
<router-link :to="{ name: 'user', params: { userId: 123 }}">User</router-link>

//或者

router.push({ name: 'user', params: { userId: 123 }})
```

##### 重定向

从根目录重新定向到home

```js
export default [
  {
    path: '/',
    redirect: '/home',
    meta: {
      'isTabbar': false
    }
  }
}
```

##### history

vue-router默认hash模式，如果不想让URL看起来很丑，就可以用history模式，这样URL看起来就像正常的URL

```js
export default new VueRouter({
  mode: 'history',
  routes
})
```

