## 单页面应用SPA架构

#### 什么是单页应用：

就是无刷新，整个`webapp`就一个html文件，里面的各个功能页面是`javascript`通过`hash`,或者`history api`来进行路由，并通过`ajax`拉取数据来实现响应功能。因为整个`webapp`就一个`html`，所以叫单页面！

1. 优点
   - 前后端职责分离，架构清晰：前端进行交互逻辑，后端负责数据处理。
   - 前后端单独开发、单独测试。
   - 良好的交互体验，前端进行的是局部渲染。避免了不必要的跳转和重复渲染。

2. 缺点
   - 不利于搜索引擎(SEO)优化

#####  路由重定向

当用户访问时，"/"会被替换为"home"

```js
const router = new VueRouter({
  routes: [
    { path: '/', redirect: '/home' }
  ]
})
```



##### 导航守卫

1. 全局前置守卫

   - to: Router即将要进入的目标
   - from：Router当前导航要离开的路由
   - next：Function一定要调用该方法来resolve这个钩子

   ```js
   const router = new VueRouter({ ... })
   
   router.beforeEach((to, from, next) => {
     // ...
   })
   ```

2. 组件内的守卫

   - beforeRoutEnter
     - 在渲染之前拦截，在created之前调用
     - beforeRouterEnter不能获取组件实例this,因为守卫执行前，组件还没有渲染

   - beforeRouterUpdate
     - 有路由时会在before/routerEnter后走beforeRouterUpdate,因为路由创建了就不会再创建，组件实例会被复用

   - beforeRouterLeave
     - 导航离开组件的对应路由时被调用

3. 路由懒加载

   - 已经渲染的路由，就不会重复渲染

     ```js
     const Home = ()=>import('../views/Home')
     //import实际上是一个promise
     ```

   -  有时候我们想把某个路由下的所有组件都打包在同个异步块 (chunk) 中

     ```js
     const Home = () => import(/* webpackChunkName: "goods" */ '../views/Home')
     ```

     

4. scss安装

   `npm i node-sass sass-loader -D`

5. scss-reset
   - github上查找
   - normalize