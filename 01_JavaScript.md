## 数据类型

- 基本数据类型：Number、String、Boolean、Undefined、Null、Symbol、BigInt
- 派生（引用）数据类型：Object、Array、Function



## 包装类

- 隐式类型转换原则

  1. 原始值之间进行比较

     1. 同类型原始值比较：直接判断是否一模一样

     2. 不同类型原始值比较：

        1. 数字和字符串：将字符串用Number包装类转换后再与数字比较

        2. 字符串和布尔值：将二者都用Number转换后再互相比较

        3. 数字与布尔值：将布尔值用Number转换后再与数字比较

           总结：不同类型原始值比较，则用数字包装类转换再比较

  1. 原始值与引用值进行比较

     核心思想：将引用值转换为原始值

     1. 调用引用值的**valueOf**方法，返回结果若为原始值，则用其与引用值比较
     2. 返回结果若为引用值，则调用其**toString**方法，返回结果为原始值则可进行比较，为引用值则报错

     补充：null和undefined相等，它们自身和自身也相等



## 函数

- 函数名装的是函数的地址，执行函数时要找到函数定义的位置执行
- 函数执行上下文（AO）与函数执行位置无关，与定义位置有关



## 预编译

- 全局预编译
  1. 创建GO（Global Object）对象
  2. 变量声明提升，变量名作为GO对象的属性，属性值为undefined
  3. 函数声明整体提升，函数名作为GO对象的属性，属性值为函数体
- 函数预编译
  1. 创建AO（Activation Object）对象
  2. 变量声明提升，变量名以及形式参数作为AO对象的属性，属性值为undefined
  3. 形参实参相统一
  4. 函数声明整体提升，函数名作为AO对象的属性，属性值为函数体



## 原型

- 原型链是一种对象链

- ```javascript
  function Person() {};
  var person = new Person();
  console.log(person.__proto__ === Object.prototype);	//结果为true，二者为同一个对象
  										 //表明实例对象的__proto__属性指向的是构造其的构造函数的prototype（对象）
  ```

- 每构造出一个函数（函数声明或匿名函数表达式）或者是内建的构造函数，其\_\_proto\_\_指向的是Function.prototype，表明任何函数都是由Function()构造，即

  ```javascript
  var F = function () {};
  console.log(F.__proto__ === Function.prototype); //结果为true
  ```



## 定位

absolute：相对于非static定位的父级元素定位，若无非static定位的父级元素，则相对于初始块容器定位。这个初始块容器有着和浏览器视口一样的尺寸，并且`<html>`元素也被包含在这个容器里面。简单来说，绝对定位元素会被放在`<html>`元素的外面，并且根据浏览器视口来定位



## this指向

1. 通过对象调用函数时，this指向对象
2. 直接调用函数时，this指向全局对象
3. 通过new关键字调用函数时，this指向新创建的对象
4. 通过apply、call、bind调用函数，this指向thisArg参数指定的数据
5. DOM事件处理函数，this指向事件源



## 严格模式

1. es5严格模式指es3和es5产生冲突的部分就使用es5，否则使用es3。分为：
   1. 全局严格模式
   2. 局部函数内严格模式
2. 语法：`"use strict"`
3. 全新es5规范：
   1. 不支持`with()`、`arguments.callee`、`function.caller`
   2. 变量赋值前必须声明
   3. 局部this必须被赋值（绑定指定对象），例如：局部（某函数体内部）下直接调用某函数时this不再指向window而是指向undefined
   4. 拒绝重复属性和参数



## 事件循环

- JS运行的环境称为**宿主环境**，浏览器宿主环境中包括5个线程：
  1. JS执行线程（JS引擎）：负责执行执行栈（Call Stack）的最顶部代码
  2. GUI渲染线程（浏览器渲染线程）：负责渲染页面
  3. 事件触发线程：负责监听各种事件
  4. 定时器触发线程：负责计时
  5. 网络（异步HTTP请求）线程：负责网络通信
- **执行栈（Call Stack）**：一种数据结构，用于存放各种函数的执行环境，任何一个函数执行之前，它的相关信息（执行上下文）会加入到执行栈
  - 函数调用之前，创建执行环境，然后加入到执行栈；函数调用之后，销毁执行环境
  - JS引擎永远都是执行执行栈的最顶部
- **异步函数**：即被创建后不会立即执行，而是要等待到某个特定时机才执行。例如事件处理函数。异步函数的执行时机，会被宿主环境控制
- 当浏览器宿主环境中的线程发生了某些事情，且线程发现该事情有处理程序，则会将处理程序添加到一个叫事件队列（Event Queue）的内存。当JS引擎发现执行栈已没有任何内容后，便会将事件队列中的第一个函数添加到执行栈中执行
- 事件队列在不同的宿主环境中有所差异，大部分宿主环境会将事件队列细分。浏览器实现中，事件队列分为以下两种：
  - **宏任务（队列）MacroTask**：计时器结束的回调、事件回调、HTTP回调等绝大部分异步函数会被加入宏队列
  - **微任务（队列）MicroTask**：MutationObserver（用于监听某个DOM对象的变化），Promise产生的回调进入微队列
  - 当执行栈清空后，JS引擎首先会将微任务中的所有任务依次执行结束，然后执行宏任务
- **JS引擎对事件队列的取出执行方式，以及与宿主环境的配合，称为事件循环**
- 目前所学的进入事件队列的函数有以下几种：
  - `setTimeout/setInterval的回调`，进入宏任务（macro task）
  - Promise的`then函数回调`，进入微任务（micro task）
  - `requestAnimationFrame的回调`，进入宏任务（macro task）
  - `事件处理函数`，进入宏任务（macro task）



## 属性描述符

- 属性描述符（Property Descriptor）：是一个普通对象，用于描述一个属性的相关信息
- `Object.getOwnPropertyDescriptor(对象，属性名)`：可得到一个对象的某属性的属性描述符对象，有以下配置项：
  - `value`：属性值
  - `configurable`：该属性的描述符是否可被修改
  - `enumerable`：该属性是否可被枚举
  - `writable`：该属性是否可被修改
  - `get`：属性的访问器函数
  - `set`：属性的设置器函数
- `Object.getOwnPropertyDescriptors(对象)`：可得到某对象的所有属性描述符
- `Object.defineProperty(对象，属性名，描述符)`：在某对象上定义或修改一个新属性，可配置其属性描述符
- `Object.defineProperties(对象，多个属性的描述符)`：在某对象上定义或修改多个属性，可同时配置每一个的属性描述符
- 属性描述符中，若配置了get或set中的任一个，则该属性成为一个**存取器属性**。get和set配置均为函数，存取器属性在读取该属性时会运行相应的get方法并返回所得到的返回值，修改该属性时会运行相应的set方法
- 存取器属性的意义在于可以控制属性的读取和修改



## JS时间线

1. 创建Document对象，开始解析web页面。解析HTML元素和他们的文本内容后添加Element对象和Text节点到文档中。这个阶段document.readyState = 'loading'

2. 遇到link外部css，创建线程加载，并继续解析文档

3. 遇到script外部js，并且没有设置async、defer，浏览器加载，并阻塞，等待js加载完成并执行该脚本，然后继续解析文档

4. 遇到script外部js，并且设置有async、defer，浏览器创建线程加载，并继续解析文档。对于async属性的脚本，脚本加载完成后立即执行。（异步禁止使用document.write()）

5. 遇到img等，先正常解析dom结构，然后浏览器异步加载src，并继续解析文档

6. 当文档解析完成，document.readyState = 'interactive'

7. 文档解析完成后，所有设置有defer的脚本会按照顺序执行。（注意与async的不同,但同样禁止使用document.write()）

8. document对象触发DOMContentLoaded事件，这也标志着程序执行从同步脚本执行阶段，转化为事件驱动阶段

9. 当所有async的脚本加载完成并执行后、img等加载完成后，document.readyState = 'complete'，window对象触发load事件

10. 从此，异步响应方式处理用户输入、网络事件等




## label标签

```html
<label for="demo">username</label>
<input type="text" id="demo">
```

设置其上的for的属性值为某input的id属性值，可将二者相关联



## 属性和特性

- 属性包含特性，即特性是标签（DOM结构）原有的，属性可以是人为添加的
- 非特性的属性与标签的行间样式无映射关系，特性则有

```html
<input type="text" value="default" id="demo" creator="xq">

<script type="text/javascript">
	var input = document.getElementById("demo");
	input.creator;
    input.data; // 读取值都为undefined，表明是从对input这个dom对象进行读取操作，与行间样式并无					映射
    input.creator = "anyone"; // 给input这个dom对象的新增一个creator属性且值为"anyone"，								 但是不会映射到行间样式上修改creator="xq"
</script>
```

若要对非特性的属性进行读写操作，则应使用getAttribute和setAttribute方法，可相对应改变行间样式

- elem.style是一个类数组，对其中属性进行读写只可改变元素行间样式，读取不到页面级css以及外部引用css的值



## 图片预加载和懒加载

1. 监控滚轮事件
2. 不断判定当前div的位置
3. 当到达临界点时采取预加载
4. 预加载完成后将图片正式地添加到页面中

前三步可看作实现懒加载的各分解步骤



## 文档碎片-虚拟DOM

- 用文档碎片来操作dom并不提高效率，例如直接往一个ul中添加10个li，和先将10个li添加到一个文档碎片后再添加到ul中的效率实际上差不多
- 后期学习框架时（vue和react）会学习到虚拟dom来作为提高效率的常用方法



## 数组方法

1. **forEach**：对数组的每个元素执行一次给定的函数

2. **filter**：创建一个新数组，其包含通过所提供函数实现的测试的所有元素

3. **map**：创建一个新数组，其包含的每个元素是调用一次提供的函数后的返回值

4. **every**：测试一个数组内的所有元素是否都能通过某个指定函数的测试，它返回一个布尔值

5. **some**：测试数组中是不是至少有1个元素通过了被提供的函数测试，它返回一个布尔值

   上述方法的语法形式形如：**arr.filter(callback, thisArg)**		**callback(elem, index, self)**

   callback为自定义回调函数（用来测试数组的每个元素的函数)，其参数elem为数组正在处理的元素，index正在处理的元素在数组中的索引值，self为调用该方法的数组本身，thisArg为callback执行时this指向的值，省略则this指向window

6. **reduce**：对数组中的每个元素执行一个由您提供的reducer函数(升序执行)，将其结果汇总为单个返回值

7. **reduceRight**：和reduce类似，差别在于对数组元素的遍历顺序与之相反(降序执行)

   二者的语法形式形如：**arr.reduce(callback, initialValue)**		**callback(accumulator, currentValue, index, self)**

   accumulator累计器累计回调的返回值; 它是上一次调用回调函数时返回的累积值，初始时为initialValue(若有)；currentValue, index, self与上述类似；initialValue作为第一次调用回调函数时的第一个参数的值。 如果没有提供初始值，则将使用数组中的第一个元素。
