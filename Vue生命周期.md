##### Vue生命周期

- 创建阶段

  - beforeCreate

    - 没什么用处

  - created

    - 没有this.$el

    - 也没有真实的dom

    - 但是已经有数据，那么就可以在这里去更改数据了，如果数据是同步修改，就带入到下一个生命周期。如果是异步修改，当数据修改完之后就会进入更新阶段，在这里做ajax请求是比较推荐的方法	

- 挂载阶段

  - ​	beforemount
    - 这里已经能看到this.$el，但是还没有进行真实的模板数据替换，看到的还是插值表达式，一般来说，也不会在这里做太多的事情

  - mounted
    - 在这里this.$el已经是真实的数据渲染的dom。在这里才能取到真实的dom,如果想要获取真实的dom，一般都在这个之后。一些第三方的dom操作插件也会在这里来进行初始化(仅限没有异步数据的dom)。有些人有喜欢在这里做ajax请求

- 更新阶段
  - beforeUpdate
    - 这里基本上不会做太多的事情

  - updated
    - 有可能需要在这里重新初始化第三方dom操作插件

- 销毁阶段

  - beforeDestroy

    - 在这里一般去解除一些事件的监听

  - destroyed

    

从开始创建、初始化数据、编译模板、挂载Dom、渲染-更新-渲染-销毁等一系列过程，我们称为Vue的生命周期

1. 实例、组件通过new Vue() 创建出来之后会初始化事件和生命周期，然后就会执行beforeCreate钩子函数，这个时候，数据还没有挂载呢，只是一个空壳，无法访问到数据和真实的dom，一般不做操作
2. 挂载数据，绑定事件等等，然后执行created函数，这个时候已经可以使用到数据，也可以更改数据,在这里更改数据不会触发updated函数，在这里可以在渲染前倒数第二次更改数据的机会，不会触发其他的钩子函数，一般可以在这里做初始数据的获取
3. 接下来开始找实例或者组件对应的模板，编译模板为虚拟dom放入到render函数中准备渲染，然后执行beforeMount钩子函数，在这个函数中虚拟dom已经创建完成，马上就要渲染,在这里也可以更改数据，不会触发updated，在这里可以在渲染前最后一次更改数据的机会，不会触发其他的钩子函数，一般可以在这里做初始数据的获取
4. 接下来开始render，渲染出真实dom，然后执行mounted钩子函数，此时，组件已经出现在页面中，数据、真实dom都已经处理好了,事件都已经挂载好了，可以在这里操作真实dom等事情...
5. 当组件或实例的数据更改之后，会立即执行beforeUpdate，然后vue的虚拟dom机制会重新构建虚拟dom与上一次的虚拟dom树利用diff算法进行对比之后重新渲染，一般不做什么事儿
6. 当更新完成后，执行updated，数据已经更改完成，dom也重新render完成，可以操作更新后的虚拟dom
7. 当经过某种途径调用$destroy方法后，立即执行beforeDestroy，一般在这里做一些善后工作，例如清除计时器、清除非指令绑定的事件等等
8. 组件的数据绑定、监听...去掉后只剩下dom空壳，这个时候，执行destroyed，在这里做善后工作也可以

##### vue标签

- ':key' vue自带在渲染时会查找上次和这次有没有区别，在循环的时候使用

- 在vue里推荐使用ref来获取dom或组件

```js
<tag ref="a"></tag>

this.$.refs.a  //取到tag
```

- slot占位

- ```js
  
  ```

- 