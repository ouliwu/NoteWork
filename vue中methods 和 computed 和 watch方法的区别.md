####  methods 和 computed 和 watch方法的区别

computed是计算属性，是有依赖缓存的，只有在它的依赖发生改变时才会重新计算，这个计算出来的值，是可以直接当成data来用的，在用的时候不需要添加()，computed必须要有一个return值

methods没有依赖缓存，只要发生重新渲染，methods方法总会执行该函数

**数据量大，需要缓存的时候用computed，每次确实需要重新-加载，不需要缓存 的时候使用methods**

watch是Vue提供的一种更通用的方式来观察和响应Vue实例上的数据变动

**尽量使用computed计算属性来监视数据的变化，因为他本身就有这个属性，watch没有computed自动，手动设置代码更复杂**

##### filter



##### 组件

定义一个组建的配置项，一般规范是大驼峰去命名一个组件，实际上还是一个Vue实例

```js
 const HelloWorld = {
            template: '<div>hello world</div>'
        }
```

 **全局注册**一个组建 ，就可以在任何地方使用

第一个参数是要使用的标签名字，不管是使用中线还是大驼峰，在使用组建的时候，都要使用小写字母加中划线

第二个参数就是组建的配置项

```js
Vue.component('HelloWorld', HelloWorld)
       
```

**局部注册**一个组件，只有当前Vue实例中才能使用这些组件. 注意这里是components

```js
<div id="app">
  <h3>app</h3>
  <hello-world></hello-world>
</div>
<div id="app1">
  <h3>app1</h3>
  <!-- 这里就报错了，因为我把全局注册的组件关闭了 -->
  <hello-world></hello-world>
</div>

const app = new Vue({
  el: '#app',
  // 局部注册一个组件，只有当前Vue实例中才能使用这些组件. 注意这里是components
  components: {
    HelloWorld
  },
  data: {
    msg: 'hello'
  }
})
```

##### component-data

组件的data必须是一个方法return一个对象，因为这样才能保证每个组件的数据是独立的而不是共享的

```js
 const HelloWorld = {
      template: '<div>{{msg}}</div>',
      data () {
        return {
          msg: 'hello world'
        }
      }
    }
  const app = new Vue({
      el: '#app',
      components: {
        HelloWorld
      },
      data: {
        msg: 'hello'
      }
    })
```

##### props

父组件控制子组件的值

在调用组件的时候通过text属性绑定一个值，在组件内部就可以通过props来接收这个值

通过props来接收调用的时候传过来的值。一旦接收了，就可以直接把props当data用，但是不能直接在内部修改props的值。这是基于单向数据流的原则的。

```js
<div id="app">

  <my-text :text="text1"></my-text>
  <my-text text="world"></my-text>
  <my-text text="!"></my-text>
</div>
const MyText = {
  template: '<span>{{text}}</span>',
  props: ['text']
}
const app = new Vue({
  el: '#app',
  data: {
    text1: 'hello',
    text2: 'world',
    text3: '!'
  },
  components: {
    MyText
  }
})
```

如果要对传入的props进行类型检查，就需要使用对象的方式来写props

如果要对x进行更多约束，就可以再把这个x写成一个对象，里面可以有默认值，必须这些选项

```js
  props: {
    x: {
      //判断属性
      type: Number,
      //必传值
      required: true
    },
    y: {
      type: Number,
      //默认值
      default: 0
    }
  }
}
```

##### props坑

```js
<div id="app">
  <!-- 由于html的属性是忽略大小写的，所以在传递的时候要使用中线连接 -->
  <hello hello-text="hello world!"></hello>
</div>
<script src="./vue.js"></script>
<script>
const Hello = {
  template: '<div>{{helloText}}</div>',
  // 由于js的命名又是驼峰类型的，所以在接收的时候又得使用驼峰
  props: ['helloText']
}
```



如果想要通过子组件修改父组件的话，需要早子组件的按钮点击时间上，通过this.$emit触发一个自定义事件

在调用这个组件的时候，就通过 @事件名 这种方式去监听组件内部触发的自定义事件， 当内部有这个自定义事件触发的时候，就会执行app里的onChangeText方法

最终的数据修改在父组件里面

##### 事件总线

兄弟组件，不能直接通信，需要通过事件总线实现通信

事件总线，相当于总指挥中心, 啥都不渲染。只是一个空的vue实例

同样通过$emit触发事件

```js
const bus = new Vue()
```

