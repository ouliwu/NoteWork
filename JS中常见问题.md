## JS中常见问题

#### 原始类型：

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

