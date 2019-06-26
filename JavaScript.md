# JavaScript

#### JavaScript的组成

- ECMAScript（核心）：JavaScript语言基础

- DOM（文档对象模型）：规定了访问HTML和XML的接口

- BOM（浏览器对象模型）：提供了浏览器窗口之间进行交互的对象和方法

  

#### JS的基本数据类型和引用类型

- **原始数据类型**

1. Null
2. undefined
3. Boolean
4. Number
5. String
6. Symbol

**注意**：es10中加入了第七种原始类型`BigInt`已被`chrome`支持

`BigInt`是一个任意精度的整数，意味着现在可以代表2^53个数字，最大限度是9007199254740992

```js
10n === BigInt(10);
=> true
10n == 10;
=> true
```



- **引用数据类型**

  1. Object
  2. Array
  3. Function

  

####  JS 有哪些内置对象

- 数据封装类对象：Object、Array、Boolean、Number、String
- 其他对象：Function、Arguments、Math、Date、RegExp、Error
- ES6新增对象：Symbol、Map、Set、Promises、Proxy、Reflect

#### 变量的内存空间

内存空间分为两种： 栈内存和堆内存

##### 栈内存：

- 存储的值大小固定
- 空间较小
- 可以直接操作其保存的变量，运行效率高
- 系统自动分配存储空间

##### 堆内存：

- 存储的值大小不定，可动态调整
- 空间较大、运行效率低
- 无法直接操作其内部存储，使用引用地址读取
- 通过代码进行分配空间

##### 0、`null`和`undefiend`

*`null`*：表示被赋值过的对象，可以把对象赋值为`null`，故意表示其为空

`null`转换为数值时值为0

*undefined*：表示缺少值，即此处应有一个值，但还没有定义

`undefined`转换为数值时为`NaN`

#### Symbol类型

`symbol`是`es6`中加入的一种原始数据类型

每个`symbol()`返回的值都是唯一的，一个symbol值能作为对象属性的表示符

- `symbol`是独一无二的

例如：

```js
var sym1 = symbol('lia')
var sym2 = symbol('lia')

console.log(sym1 === sym2) //false
```

不可枚举，调用`for...in`不能将其枚举出来，调用`Object.getOwnPropertyNames`、`Object.keys()`也不能获取`symbol·属性

**应用一：防止XSS**

在`React`的`ReactElement`对象中，有一个`$$typeof`属性，它是一个`Symbol`类型的变量：

```
var REACT_ELEMENT_TYPE =
  (typeof Symbol === 'function' && Symbol.for && Symbol.for('react.element')) ||
  0xeac7;
```

`ReactElement.isValidElement`函数用来判断一个React组件是否是有效的，下面是它的具体实现。

```
ReactElement.isValidElement = function (object) {
  return typeof object === 'object' && object !== null && object.$$typeof === REACT_ELEMENT_TYPE;
};
```

**应用二：私有属性**

借助`Symbol`类型的不可枚举，我们可以在类中模拟私有属性，控制变量读写：

```
const privateField = Symbol();
class myClass {
  constructor(){
    this[privateField] = 'ConardLi';
  }
  getField(){
    return this[privateField];
  }
  setField(val){
    this[privateField] = val;
  }
}
```

#### 类型转换

- 隐式类型转换：==、+、-、*、/
- 显示类型转换：ParseInt、ParseFloat、Number、toString、String

```js
parseInt(str, 16);
//第二个参数，指定字符串的进制形式
```

parseInt()判断是否为有效数字，不是返回NaN，是的话返回有效字符串

**注意**：除了+运算符具有两层含义之外，-、'*'、''/ ''只有数字意义，所以当‘-’、‘*’、'/'运算时默认隐式类型转换为数字类型运算

#### NaN

一种特殊值，和任意数运算，结果都是NaN

算数运算时，本来期望得到一个数字，但是没有成功

isNaN(num)：该函数判断num变量的值是否是NaN，是NaN返回true，数字返回false  

#### 数组常见API

- contat()：连接两个或多个数组，并返回结果操作的不是数组本身

  ```js
  var a = arr.contat(arr1,arr2)
  ```

  

- join()：把数组的所有元素放入一个字符串，元素通过指定的分隔符分隔

  ```js
  var str = arr.join('-')
  ```

  

- pop()：删除并**返回数组的最后一个元素**

  ```j
  var num = arr.pop()
  ```

- push()：向数组的末尾添加一个或多个元素，**并返回新的长度**

  ```js
  var len = arr.push(4)
  ```

  

- shift()：删除并返回数组的第一个元素

- unshift()：向数组的开头添加一个或多个元素，并返回新的元素

- reverse()：颠倒数组中元素的顺序，**修改的是数组本身**

  ```js
  arr.reverse()
  ```

  

- slice()：从某个已有数组返回选定的元素

  ```js
  var a = arr.slice(1,2) // 含头不含尾
  // 负数指倒数第几个
  // 一个参数代表截取到末尾
  ```

  

- sort()：对数组的元素进行排序， `a-b`升序，`b-a`降序

- splice()：删除元素，并向数组添加新元素

  ```js
  arr.splice(1,1) // 从下标1开始，截取1个
  arr.splice(1,0,1,3,4) // 从下标1开始截取0个，添加1,3,4
  ```

  

- toString()：把数组转换为字符串，并返回结果

#### 数组的两种定义方法：

```js
var arr = [] // 字面量方式

var arr = new Array() // 构造函数的方式
```

#### ES5新增数组常见方法：

- 2个索引方法：indexOf()和lastIndexOf()
- 5个迭代方法：forEach()、map()、filter()、some()、every()
- 2个归并方法：reduce()、reduceRight()

some：只有满足条件的即可

every：每个条件都要满足

```js
arr.forEach(function(item, index){
    // 遍历arr中的每一个item
})

arr.filter(function(item, index){
    return item < 10
    // 过滤掉不满足条件的，返回布尔值
})

//可去重、排序
arr.reduce(function(prev, next){
    return prev = prev + next
    //上次返回的结果作为下一次的prev
},0)
```

#### 数组去重

###### 1.利用splice()方法

```js
var arr= [2,3,4,2,3,2,6,7,8,9]

for(var i = 0; i < arr.length; i++) {
    for(var J = i+1; j < arr.length; j++) {
        if(arr[i] === arr[j]) {
            arr.splice(j--, 1)
            // 当删除一个值之后，后面的值的索引会全部减1
        }
    }
}

console.log(arr)
```

###### 2.利用对象，属性名不能重复的原理

```js
var obj = {}
var arr= [2,3,4,2,3,2,6,7,8,9] 

for(var i = 0; i< arr.length; i++) {
    // 判断obj中是否有arr[i]这个属性
    if(!obj[arr[i]]) {
        obj[arr[i]] = 1
    } else {
        arr.splice(i--, 1)
    }
}

console.log(arr)
```

###### 3.使用ES6中的方法

`Array.from()`从一个类似数组或可迭代队形中创建一个新的数组实例

`Set`()默认不允许重复

```js
var arr= [2,3,4,2,3,2,6,7,8,9] 

var arr2 = Array.from(new Set(arrr))
```

#### JavaScript内置对象

1. Object对象：是所有JavaScript对象的超类（基类）
2. Array对象：定义数组属性和方法
3. Boolean对象：布尔值相关
4. Date对象：日期对象
5. Error对象：错误对象，处理程序错误
6. Function对象：函数对象、定义属性和方法
7. Math对象：各种数学运算工具（不是构造函数）
8. Number对象：定义数字属性和方法
9. RegExp对象：正则表达式对象——定义文本匹配与筛选规则
10. String对象：定义字符串属性和方法

#### Date内置对象

**时间戳**：指格林威治时间1970年01月01日00时00分00秒（北京时间1970年01月01日08时00分00秒）到现在的总毫秒数

```js
var date = new Date() // 当前时间对象

var date = new Date(2012,6,10) // 2012年7月10日
```

###### get系列API

- getFullYear()：返回年
- getMonth()：返回月份 0-11
- getDate()：返回某一天
- getDay()：返回星期 0-6
- getHours()：返回小时
- getMinutes()：返回分钟
- getSeconds()：返回秒
- getTime()：返回1970年1月1日午夜到指定日期的毫秒数

###### set系列API

- setFullYear()：设置年
- setMonth()：设置月
- setDate()：设置天
- setHours()：设置小时
- setMinutes()：设置分钟
- setSeconds()：设置秒
- setTime()：使用毫秒形式设置时间对象

#### BOM(Browser Object Modal)浏览器对象模型

window是全局浏览器内置顶级对象

全局变量默认是挂在window下的

##### window下的子对象

- location：

  - loaction.reload()

    刷新页面的方法，一般情况下reload()传递一个true，刷新不使用缓存，缓存的东西一般为js文件css文件等

  - loaction.assign(URL)

    加载URL到指定的新的HTML中

  - navigator.userAgent

    返回浏览器信息，可以用此判断当前浏览器

  - history

    - history.go(1)

    参数可以写任意整数，正数前进，负数后退

    - history.back() 后退
    - history.forward() 前进

- screen
  - screen.width	返回当前屏幕宽度
  - screen.height       返回当前屏幕高度

- window下的弹框方法
  - alert()
  - prompt()
  - confirm()     带有提示信息和确认以及取消按钮

- 定时器

  - 超时定时器
    - setTimeout()
    - clearTimeout()
  - 间隔定时器
    - setInterval()
    - clearInterval()

- window.onload事件绑定函数在页面加载后执行

- window.onScroll   存在兼容问题

  ```js
  var scrolltop = document.documentElement.scrollTOp || document.body.scrollTop
  ```

####  DOM(Document Object Model) 文档对象模型

定义了表示修改文档所需的对象、行为和属性以及这些对象之间的关系

- ##### 获取DOM节点

  - document.getElementById(id名)
  - getElementsByTagName(标签名)     
    -  得到的是一个集合
  - getElementsByName()          
    - 返回的是集合，通常用来获取有name的input值
  - children属性
    - 获取DOM袁术的所有子元素，返回值是一个集合
  - parentNode属性
    - 获取DOM元素的父级元素
  - getElementsByclassName(class名称)
    - IE8以下不能使用
  - ES5选择器
    - documen.querySelector()
      - 一旦匹配成功一个，就不往后匹配了
    - document.querySelectorAll()
      - 匹配所有满足的元素，支持IE8+

##### 属性的获取和操作

- getAttribute()

  - 获取元素的属性值，是节点的方法，所以前缀必须是节点

    ```js
    document.getElementById(id值).getAttribute()
    ```

- setAttribute()

  - 设置元素的属性值，设置的属性永远都是字符串属性

- removeAttribute()

  - 删除属性

##### 操作DOM

- 创建节点

  ```js
  var a = document.createElement('div')
  ```

- 克隆节点

  - 只有一个参数，传入一个布尔值，true表示深克隆，复制该节点下的所有子节点，false表示浅克隆，只复制该节点

  ```js
  clonedNode = Node.cloneNode(boolean)
  ```

  

- 插入节点
  - parentNode.appendChild(childNode)
    - 将新节点追加到子节点列表的末尾
  - parentNode.insertBefore(newNode, targetNode)
    - 将newNode插入到targetNode之前

- 替换节点
  - parentNode.replaceChild(newNode, targetNode)
    - 使用newNode替换targetNode

- 移除节点

  - parentNode.removeChild(childNode)	移除目标节点

  - node.parentNode.removeChild(node)	在不清楚父节点的情况下使用

##### node节点

w3c的HTML DOM标准，HTML文档中的所有内容都是节点

根节点：root——HTML没有父节点

- nodeType	节点种类，返回值是数字
- nodeValue       获取文字节点的文本内容
- nodeName       返回node节点名称（#text、注释、标签……）

nodeType值：

- 1		元素（DIV、Body、Li、span）
- 2                代表属性节点（class、src、href)
- 3                文本节点（text节点）
- 8                代表注释节点
- 9                代表document节点

 

