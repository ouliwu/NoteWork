processOn流程图



this.setState()

老编辑器使用的是`document.designModle`

textarea不能随内容自动增高，不能显示图片

##### 富文本编辑器

```html
<div contenteditable style="border:1px solid #dedede; with:500px; min-height:300px" >
</div>
```



```js
var BoldStyle = function() {
    document.execCommand('Bold')
}

var BoldStyle = function() {
    document.execCommand('Italic')
}
```



##### 常见的富文本编辑器

1. iframe
2. kindEditor
3. wangEditor
4. editor.md 支持markdown格式的编辑器

推荐使用后两个





```js
this.editorRef.current
```





生成随机色

##### 网页上图表的技术

1. canvas   位图
2. svg       矢量图
3. 三维     webgl

**开源库**

​	echarts  占有量最大

​	highcharts

**数据可视化**

​	d3

​	dataV	阿里用于做大型项目的 如：双11

​	egret(百度)	用于做游戏的

​	antV

​	rapheal.js	兼容最老的版本

​	p5.js	





extra



xlsx生成excel表格

```js
npm i xlsx -S
```



##### 通知消息使用高阶组件传递

安装redux 和thunk 

新建reducers文件

​	store 写入初始化数据

​	reducers.index.js中合并

使用高阶组件Provider传递state

在需要接收消息的组件中使用connet接收

在这里可以使用 state.notification.list

这时打印console.log(this.props)中如果有dispatch说明连接成功



actionTypes根据不同的action执行不同的操作

项目中点击标记为已读其实是异步操作因为要把消息传递给后端

在Notifications组件中使用actions中的方法传入id,在actions中写处理接收id ,在reducers中编写处理方法



路由已经销毁但是setState还在取数据，这时就可以使用这个方法

if(！this.updatoer.isMounted(this) = true) return 



使用translate3d时会使用GPU加速，这时会独立一个层来渲染

##### 删除分为逻辑删除和物理删除

逻辑删除只是做一个标识，会做在回收站里面，但是大部分的删除都是做的逻辑删除isDlete

物理删除是直接从数据库中删除