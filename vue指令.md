### vue指令

**注意**：只要使用`vue`的指令就会被认为是`javascript`

`vue`指令都是以`v-`这种格式，一旦使用了指令，虽然我们写的值看起来是字符串，但这个值已经是`javascript了`

`vue`语法使用插值表达式

```js
//{{这里必须是一个表达式}}
{{ 1 + 1 === 2 ? '正确' : '错误'}}
//不能在{{}}中使用语句
```

#####  v-text 用于替换插值表达式

在v-text之后直接等于一个字符串会被认为是`javascript`语法这时需要加单引号

```js
//这时{{msg}}不会起作用
<div v-text="'abc'">{{msg}}</div>
```

**v-html**

如果要输出正确的不带html标签转义的内容，就需要v-html指令

```js
<div id="root"> 
	<div>{{content}}</div>
</div>

const title = new Vue({
    el: '#root'
    data: {
    	msg: 'Hello'
    	content: '<h1>这是标题一</h1>'
}
})
```

**v-cloak**

如果网络加载延迟，·vue.js·还没有加载的时候就会出现乱码	 例如:出现`{{msg}}`

与style标签一起使用，防止页面刷新不出来的时候出现乱码

```htm
<!--在数据还没渲染之前不显示{{msg}}-->
<!--这个样式，只有在Vue实例化之前才有效-->
[v-cloak] {

    display: none;

}
```



```js
<div id="root" v-cloak>
    <div>
        {{msg}}
    </div>
</div>
```

**v-for:**

**用来循环渲染数据，通常使用key元素给每个元素添加识别码**

遍历数组

```html
<ul>
    <li 
        v-for="item in list"
        key={{item.id}}
    >
        <span>{{item.title}}</span>
    </li>  
</ul>

```

```js
const app = new Vue({
    el: '#root',
    data: {
        msg: 'Hello',
        list: [{
            id: 1,
            title: '吃饭'
        }, {
            id: 2,
            title: '睡觉'
        }，{
            id: 3,
            title: '写代码'
        }]
    }
})
```

v-for也可以遍历对象

```js
<ul>
    <li v-for="(value, index) in lia">
        {{index}} : {{value}}
    </li>
</ul>
```



```js
const app = new Vue({
    el: '#root',
    data: {
        msg: 'Hello',
        lia: {
            name: 'lia',
            age: '18',
            favor: {
                '吃饭'，
                ‘睡觉’，
                ’写代码‘
            }
        }
    }
})
```

v-for遍历数字

```html
<li v-for="n of 100">{{n}}</li>
```

**v-if**

```html
<span>{{item.isCompleted ? '已完成' ： ‘未完成’}}</span>

<span v-if="item.isCompleted === true" style="color:#090">
    已完成
</span>
```

**v-show 和 v-if 的区别**

```html
<div v-if="isModalShow === true">弹窗</div>
<div v-show="isModalShow === true">弹窗</div>
```

​    相同点：v-if与v-show都可以动态控制`dom`元素显示隐藏

​    不同点：v-if是直接从`dom`元素里把节点添加或删除，而v-show是通过`style.display:none`来确定该节点的是否显示，`dom`元素还在。对于需要频繁切换显示和隐藏的节点特别实用。比如：弹窗、 手机注册和邮箱注册两个tab的切换



**v-model**

实现双向数据绑定，常用于input和textarea

是一个典型的MVVM模型

**底层的实现原理：**

**Vue自身实现一个Watcher，作为连接Observer和Compile的桥梁。而数据层的Observe和视图层的Compile都是基于观察者模式实现的，再加上Watcher这个中间桥梁，Vue实例能够订阅并收到每个属性变动的通知，执行指令绑定的相应回调函数，从而更新视图**

![img](https://upload-images.jianshu.io/upload_images/13185362-46aa8d8b2a6fb028?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



**双向数据绑定原理：**

**Object.defineProperty()函数可以定义对象的属性相关描述符， 其中的set和get函数对于完成数据双向绑定起到了至关重要的作用，下面，我们看看这个函数的基本使用方式。**

var obj = {

​      foo: 'foo'    }

​    Object.defineProperty(obj, 'foo', {

​      get: function () {

​        console.log('将要读取obj.foo属性');

​      },

​      set: function (newVal) {

​        console.log('当前值为', newVal);

​      }

​    });

​    obj.foo; // 将要读取obj.foo属性obj.foo ='name';// 当前值为 name

**v-bind**特性被称为指令，指令带有前缀v-表示他们是Vue提供的特殊特性

**v-on**

v-on:事件名    用于绑定一个事件，这个事件可以直操作data里面的数据

v-on:事件名    可以直接缩写为@事件名

**v-bind**

用于动态绑定元素的属性

可以直接简写为 ：属性

​    v-bind:key="item.id"

​    :key="item.id"

**按键修饰符**

可以通过`v-on`添加按键修饰符

`v-on`指令绑定enter键事件，点击执行`submit()`函数事件，

同样也可以简写为`@keyup.enter="onAddValue()"`

下面的`button`按钮也绑定了点击事件，执行函数`onAddValue()`

同样`vue`监听按键事件也可以添加其他的按键：只要加上相应的按键的名称就行了

```html
<div id="root">
   <input type="text" v-model="inputValue" @keyup.ctrl.enter="onAddValue()">

   <button @click="onAddValue">添加</button>

</div>
```

也可以自定义按键修饰

```js
<input type="text" v-model="inputValue" @keyup.lia="onAddValue()">
vue.config.keyCode.lia = 13
```

**阻止事件冒泡**

```html
<div @click="onWrapperClick">
    外层
    <div onClick.stop="onInnerClick">
       内层
    </div>
</div>
```



**v-on.prevent:**

阻止事件的默认行为



vue中事件是写在methods中的