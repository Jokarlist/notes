## input新增属性

- **placeholder**：当表单控件为空时，控件中显示的内容
- **type**：
  - `date`：输入日期的控件（年、月、日，不包括时间）
  - `time`：用于输入时间的控件，不包括时区
  - `week`：用于输入以年和周数组成的日期，不带时区
  - `month`：输入年和月的控件，没有时区
  - `datetime-local`：输入日期和时间的控件，不包括时区
  - `number`：用于输入数字的控件，有验证（即只能输入数字）
  - `email`：编辑邮箱地址的区域
  - `color`：用于指定颜色的控件
  - `range`：此控件用于输入不需要精确的数字。控件是一个范围组件，默认值为正中间的值
  - `search`：用于搜索字符串的单行文字区域。输入文本中的换行会被自动去除
  - `url`：用于输入 URL 的控件，有验证
  - 以上type类型值对各个浏览器的兼容性各有不相同，生产环境下不能随便使用



## contenteditable属性

- 全局属性，表示元素是否可被用户编辑，取值为true和false。 如果可以，浏览器会修改元素的部件以允许编辑
- 如果没有设置该属性，其默认值继承自父元素
- 元素内的部件可编辑代表HTML结构可编辑，即可以删除HTML文本与标签



## draggable属性

- 全局属性，用于标识元素是否允许使用拖放操作API拖动，取值为true和false，默认值 为 auto ，表示使用浏览器定义的默认行为

- 默认情况下，只有已选中的文本、图片（img标签）、链接（a标签）可以拖动

- 拖拽的生命周期：
  1. 拖拽开始：

     `dragstart`：当用户开始拖动元素（click按下抬起时，mouseup时）或选择文本时触发此事件

  2. 拖拽进行中：

     `drag`：拖动元素或选择文本时触发此事件

  3. 拖拽结束：

     `dragend`：当拖动操作结束时（释放鼠标按钮或按下退出键），会触发此事件

- 拖拽的组成：
  1. 被拖拽的物体
  2. 目标区域（目标元素）：
     1. `dragente`r：当拖动的元素或选择文本输入有效的放置目标时，会触发此事件
     2. `dragover`：当将元素或文本选择拖动到有效放置目标（每几百毫秒）上时，会触发此事件
     3. `dragleave`：当拖动的元素或选择文本输入有效的放置目标时，会触发此事件
     4. `drop`：当在有效放置目标上放置元素或选择文本时触发此事件

- 所有的标签元素，当拖拽周期结束时，默认事件是回到原处。事件是由行为触发，一个行为可能会触发多个事件，当在目标区域里结束拖拽事件时，会触发dragover（默认触发）和drop事件，所以要在dragover内使用e.preventDefault()阻止回到原处的默认事件才得以触发drop事件



## dataTransfer属性

- drag event下的属性对象，兼容性不好，生产环境下基本不使用
  - `e.dataTransfer.effectAllowed`：提供所有可用的操作类型。必须是  `none`, `copy`, `copyLink`, `copyMove`, `link`, `linkMove`, `move`, `all` or `uninitialized` 之一。设置在dragstart事件中
  - `e.dataTransfer.dropEffect`：获取当前选定的拖放操作类型或者设置的为一个新的类型。值必须为  `none`, `copy`, `link` 或 `move`。设置在drop事件中



## 语义化标签

- `<header></header>`
- `<footer></footer>`
- `<nav></nav>`
- `<article></article>`
- `<section></section>`
- `<aside></aside>`



## canvas画线

1. 获取canvas元素的dom对象令之为cnv
2. 通过`cnv.getContext("2d")`返回canvas的上下文ctx
3. `ctx.moveTo()`是 Canvas 2D API 将一个新的子路径的起始点移动到(x，y)坐标的方法
4. `ctx.lineTo()`是 Canvas 2D API 使用直线连接子路径的终点到x，y坐标的方法（并不会真正地绘制）
5. `ctx.stroke()`是 Canvas 2D API 使用非零环绕规则，根据当前的画线样式，绘制当前或已经存在的路径的方法
6. `ctx.closePath()`是 Canvas 2D API 将笔点返回到**当前**子路径起始点的方法。它尝试从当前点到起始点绘制一条直线。 如果图形已经是封闭的或者只有一个点，那么此方法不会做任何操作
7. `ctx.fill()`是 Canvas 2D API 根据当前的填充样式，填充当前或已存在的路径的方法
8. `ctx.lineWidth`是 Canvas 2D API 设置线段厚度的属性（即线段的宽度），值为数字，0、负数、`Infinity`和 `NaN`会被忽略
9. `ctx.beginPath()`是 Canvas 2D API 通过清空子路径列表开始一个新路径的方法。 当你想创建一个新的路径时，调用此方法



## canvas画矩形

1. `ctx.rect()`是 Canvas 2D API 创建矩形路径的方法，矩形的起点位置是 *(x, y)* ，尺寸为 *width* 和 *height*。矩形的4个点通过直线连接，子路径作为闭合的标记，所以你可以填充或者描边矩形
2. `ctx.strokeRect()`是 Canvas 2D API 在 canvas 中，使用当前的绘画样式，描绘一个起点在 *(x, y)* 、宽度为 *w* 、高度为 *h* 的矩形的方法
3. `ctx.fillRect()`是Canvas 2D API 绘制填充矩形的方法。当前渲染上下文中的`fillStyle` 属性决定了对这个矩形对的填充样式



## canvas画圆

`ctx.arc(x, y, radius, startAngle, endAngle, anticlockwise)`是 Canvas 2D API 绘制圆弧路径的方法。 圆弧路径的圆心在 *(x, y)* 位置，半径为 *radius* ，根据*anticlockwise* （默认为顺时针）指定的方向从 *startAngle* 开始绘制，到 *endAngle* 结束



## canvas画圆角矩形

`ctx.arcTo(x, y, radius, startAngle, endAngle, anticlockwise)`是 Canvas 2D API 根据控制点和半径绘制圆弧路径，使用当前的描点(前一个moveTo或lineTo等函数的止点)。根据当前描点与给定的控制点1连接的直线，和控制点1与控制点2连接的直线，作为使用指定半径的圆的**切线**，画出两条切线之间的弧线路径



## canvas贝塞尔曲线

1. `ctx.quadraticCurveTo(cpx, cpy, x, y)`是 Canvas 2D API 新增二次贝塞尔曲线路径的方法。它需要2个点。 第一个点是控制点，第二个点是终点。 起始点是当前路径最新的点，当创建二次贝赛尔曲线之前，可以使用 `moveTo()` 方法进行改变
2. `ctx.bezierCurveTo(cp1x, cp1y, cp2x, cp2y, x, y)`是 Canvas 2D API 绘制三次贝赛尔曲线路径的方法。 该方法需要三个点。 第一、第二个点是控制点，第三个点是结束点。起始点是当前路径的最后一个点，绘制贝赛尔曲线前，可以通过调用 `moveTo()` 进行修改



## canvas坐标平移旋转与缩放

1. `ctx.translate(x, y)`：将 canvas 按原始 x点的水平方向、原始的 y点垂直方向进行平移变换
2. `ctx.rotate()`：是 Canvas 2D API 在变换矩阵中增加旋转的方法（即改变坐标系角度位置）。角度变量表示一个顺时针旋转角度，并且用弧度表示
3. `ctx.scale()`：是 Canvas 2D API 根据 x 水平方向和 y 垂直方向，为canvas 单位添加缩放变换的方法



## canvas的save和restore

1. `ctx.save()`：是 Canvas 2D API 通过将当前状态放入栈中，保存 canvas 全部状态的方法
2. `ctx.restore()`：是 Canvas 2D API 通过在绘图状态栈中弹出顶端的状态，将 canvas 恢复到最近的保存状态的方法



## canvas背景填充

1. `ctx.fillStyle`：是Canvas 2D API 使用内部方式描述颜色和样式的属性。默认值是 `#000` （黑色）
2. `ctx.createPattern(img, repetition)`：是 Canvas 2D API 使用指定的图像`CanvasImageSource`创建模式的方法。 它通过repetition参数在指定的方向上重复元图像。此方法返回一个`CanvasPattern`对象



## canvas线性渐变

1. `ctx.createLinearGradient(x0, y0, x1, y1)`：创建一个沿参数坐标指定的直线的渐变

   该方法返回一个线性`CanvasGradient`对象。想要应用这个渐变，需要把这个返回值赋值给`fillStyle`或者 `strokeStyle`

2. `gradient.addColorStop()`：添加一个由**`偏移值`**和**`颜色值`**指定的断点到渐变。偏移值为0到1之间的值



## canvas辐射渐变

1. `ctx.createRadialGradient(x0, y0, r0, x1, y1, r1)`：是 Canvas 2D API 根据参数确定两个圆的坐标，绘制放射性渐变的方法



## canvas的阴影

1. `ctx.shadowColor`：是 Canvas 2D API 描述阴影颜色的属性
2. `ctx.shadowBlur`：是 Canvas 2D API 描述模糊效果程度的属性； 它既不对应像素值也不受当前转换矩阵的影响。 默认值是 0
3. `ctx.shadowOffsetX`：是 Canvas 2D API 描述阴影水平偏移距离的属性
4. `ctx.shadowOffsetY`：是 Canvas 2D API 描述阴影垂直偏移距离的属性



## canvas渲染文字

1. `ctx.font`：是 Canvas 2D API 描述绘制文字时，当前字体样式的属性。 使用和 CSS font 规范相同的字符串值
2. `ctx.strokeText(text, x, y [, maxWidth])`：是 Canvas 2D API 在给定的 *(x, y)* 位置绘制文本的方法。如果提供了表示最大值的第四个参数，文本将会缩放适应宽度
3. `ctx.fillText(text, x, y, [maxWidth])`：是 Canvas 2D API 在 *(x, y)*位置填充文本的方法。如果选项的第四个参数提供了最大宽度，文本会进行缩放以适应最大宽度



## canvas线端样式

1. `ctx.lineCap`：是 Canvas 2D API 指定如何绘制每一条线段末端的属性。有3个可能的值，分别是：`butt`, `round` and `square`。默认值是 `butt`
2. `ctx.lineJoin`：是 Canvas 2D API 用来设置2个长度不为0的相连部分（线段，圆弧，曲线）如何连接在一起的属性（长度为0的变形部分，其指定的末端和控制点在同一位置，会被忽略）
3. `ctx.miterLimit`：是 Canvas 2D API 设置斜接面限制比例的属性



## svg画线与矩形

1. `<svg>`：如果svg不是根元素，`svg` 元素可以用于在当前文档（比如说，一个HTML文档）内嵌套一个独立的svg片段 。 这个独立片段拥有独立的视口和坐标系统
2. `<line>`：是一个SVG基本形状，用来创建一条连接两个点的线
3. `<rect>`：是SVG的一个基本形状，用来创建矩形，基于一个角位置以及它的宽和高。它还可以用来创建圆角矩形



## svg画圆、椭圆、折线

1. `<circle>`：是一个SVG的基本形状，用来创建圆,基于一个圆心和一个半径

2. `<ellipse>`：是一个SVG基本形状，用来创建一个椭圆，基于一个中心坐标以及它们的`x`半径和`y`半径

3. `<polyline>`：是SVG的一个基本形状，用来创建一系列直线连接多个点

   上述都默认会闭合且填充，可自定义样式进行改变



## svg画多边形和文本

1. `<polygon>`：定义了一个由一组首尾相连的直线线段构成的闭合多边形形状。最后一点连接到第一点
2. `<text>`：定义了一个由文字组成的图形。注意：我们可以将渐变、图案、剪切路径、遮罩或者滤镜应用到text上



## svg透明度与线条样式

1. `stroke-opacity`：指定了当前对象的轮廓的不透明度
2. `fill-opacity`：指定了填色的不透明度或当前对象的内容物的不透明度
3. `stroke-linecap`：制定了，在开放子路径被设置描边的情况下，用于开放自路径两端的形状
4. `stroke-linejoin`：指明路径的转角处使用的形状或者绘制的基础形状



## svg的path标签

1. `<path>`：是用来定义形状的通用元素

   属性`d`定义了一个路径，路径指令有Moveto（M，m），Lineto（L，l），Horizontal（H），Vertical（V）ClosePath（z）对大小写敏感，一个大写的命令指明它的参数是绝对位置，而小写的命令指明相对于当前位置的点

2. path画弧：路径指令Arcto（A）格式：A rx ry xAxisRotate LargeArcFlag SweepFlag x y

   - rx和ry分别是x和y方向的半径
   - LargeArcFlag的值用来确定是要画小弧（0）还是画大弧（1）
   - SweepFlag的值用来确定弧是顺时针方向（1）还是逆时针方向（0）
   - x和y是目的地的坐标



## svg线性渐变

1. `<defs>`：SVG 允许我们定义以后需要重复使用的图形元素。 建议把所有需要再次使用的引用元素定义在defs元素里面
2. `<linearGradient>`：用来定义线性渐变，用于图形元素的填充或描边
3. 图形元素的填充用`fill`属性的`url(#id)`



## svg高斯模糊

1. `<filter>`：是作为原子滤镜操作的容器。它不能直接呈现。可以利用目标SVG元素上的`filter`属性引用一个滤镜
2. `<feGaussianBlur>`：该滤镜对输入图像进行高斯模糊，属性`in`标识输入的原语，值为SourceGraphic表示图形元素自身将作为`<filter>`原语的原始输入；属性`stdDeviation`中指定的数量定义了钟形



## svg虚线及简单动画

1. `stroke-dasharray`：控制用来描边的点划线的图案范式，数与数之间用逗号或者空白隔开，指定短划线和缺口的长度
2. `stroke-dashoffset`：指定了dash模式到路径开始的距离



## svg的viewbox（比例尺）

`viewBox`：值是一个包含4个参数的列表 `min-x`, `min-y`, `width` and `height`， 以空格或者逗号分隔开， 在用户空间中指定一个矩形区域映射到给定的元素



## video和audio

1. `<video>`：用于在HTML或者XHTML文档中嵌入媒体播放器，用于支持文档内的视频播放
2. `<audio>`：用于在文档中嵌入音频内容



## video

1. play()：该方法会尝试播放媒体。这个方法返回一个 `Promise`，当媒体成功开始播放时被解决（resolved）。当播放因为任何原因失败时，这个 promise 被拒绝（rejected）
2. pause()：方法可用来暂停媒体的播放，如果媒体已经处于暂停状态，该方法没有效果
3. paused属性：告诉视频是否正在暂停，返回true表示暂停中，false表示未暂停
4. duration属性：以秒为单位给出媒体的长度，如果没有媒体数据可用，则为零
5. currentTime属性：会以秒为单位返回当前媒体元素的播放时间。设置这个属性会改变媒体元素当前播放位置
6. playbackRate属性：设置媒体文件播放时的速率。这用于实现让用户控制快放、慢放等。 正常播放速率乘以该值表示当前的播放速率，所以1.0表示一个正常的播放速率，设为负值**不可以**实现倒播
7. volume属性：可设置媒体播放时的音量。取值为 0 到 1 的双精度值。0 为静音，1 为音量最大时的值



## geolocation-api

h5获取地理位置的方法，是使用网络获取粗略位置：

```js
window.navigator.geolocation.getCurrentPosition(success, error, options);
// success: 成功得到位置信息时的回调函数，使用Position对象作为唯一的参数
// error: 获取位置信息失败时的回调函数，使用PositionError对象作为唯一的参数，这是一个可选项
// options: 一个可选的PositionOptions对象

// 只适用于https协议和file协议，http协议不安全所以不能获取
```



## deviceorientation接口

DeviceOrientationEvent提供给网页开发者当设备（指手机，平板等移动设备）在浏览页面时物理旋转的信息

- `DeviceOrientationEvent.alpha`：一个表示设备绕z轴旋转的角度（范围在0-360之间）的数字
- `DeviceOrientationEvent.beta`：一个表示设备绕x轴旋转（范围在－180到180之间）的数字，从前到后的方向为正方向
- `DeviceOrientationEvent.gamma`：一个表示设备绕y轴旋转（范围在－90到90之间）的数字，从左向右为正方向



## devicemotion接口

`DeviceMotionEvent` 为web开发者提供了关于设备的位置和方向的改变速度的信息

- `DeviceMotionEvent.acceleration`：提供了设备在X,Y,Z轴方向上加速度的对象。加速度的单位为 m/s^2^
- `DeviceMotionEvent.rotationRate`：提供了设备在 alpha，beta， gamma轴方向上旋转的速率的对象。旋转速率的单位为 °/s



## requestAnimationFrame接口

- 告诉浏览器希望执行一个动画，并且要求浏览器在下次重绘之前调用指定的回调函数更新动画。该方法需要传入一个回调函数作为参数，该回调函数会在浏览器下一次重绘之前执行
- 当你准备更新动画时你应该调用此方法。这将使浏览器在下一次重绘之前调用你传入给该方法的动画函数(即你的回调函数)。回调函数执行次数通常是每秒60次，但在大多数遵循W3C建议的浏览器中，回调函数执行次数通常与浏览器屏幕刷新次数相匹配
- 使用该接口的优点是：比起使用setTimeout或setInterval需要开发者自己定时，定时时间与屏幕刷新频率的不匹配而造成帧丢失，该接口可以自动地匹配屏幕刷新率，以准确地执行没一帧，呈现出来的动画效果更好
- 兼容性很差
- 语法：`window.requestAnimationFrame(callback)`：会返回一个整数，是回调列表中唯一的标识，可以传递给`window.cancelAnimationFrame()`以取消回调函数



## history接口

- BOM中的history对象提供了对浏览器的会话历史的访问。它暴露了很多有用的方法和属性，允许你在用户浏览历史中向前和向后跳转，且自h5开始提供了对history栈中内容的操作

- `history.pushState(state, title[, url])`：向当前浏览器会话的历史堆栈中添加一个状态。

  state：状态对象；title：大多数浏览器不支持，一般设置为null；url：可选项。指定新历史记录条目的url

- `history.replaceState(state, title[, url])`：修改当前历史记录实体

- 每当处于激活状态的历史记录条目发生变化时，`popstate`事件就会在对应window对象上触发。如果当前处于激活状态的历史记录条目是由`history.pushState()`方法创建，或者由`history.replaceState()`方法修改过的，则popstate事件对象的state属性包含了这个历史记录条目的state对象（即调用上述两个方法时传入的第一个state参数）的一个拷贝

- 调用`history.pushState()`或者`history.replaceState()`不会触发`popstate`事件。`popstate`事件只会在浏览器某些行为下触发，比如点击后退、前进按钮（或者在js中调用`history.back()、history.forward()、history.go()`方法），此外，a标签的锚点也会触发该事件

- 当url的片段标识符更改时，将触发`hashchange`事件（跟在＃符号后面的url部分，包括＃符号），但`pushState()` 不会造成 `hashchange` 事件调用，即使新的url和之前的url锚的数据不同。而在url的锚点数据被修改时则会触发`popstate`事件和`hashchange`事件



## worker接口

- 通过使用Web Workers，Web应用程序可以在独立于主线程的后台线程中，运行一个脚本操作。这样做的好处是可以在独立线程中执行费时的处理任务，从而允许主线程（通常是UI线程）不会因此被阻塞/放慢。Worker接口时Web Workers API的一部分

- worker不能操作dom，没有window对象，不能读取本地文件。可以使用ajax，可以进行计算

- 使用构造函数（例如`Worker()`）创建一个 worker 对象，构造函数接受一个js文件url，这个文件包含了将在 worker 线程中运行的代码。worker 将运行在与当前 window 不同的另一个全局上下文中，这个上下文由一个对象表示

  ```js
  const worker = new Worker("./worker.js");	// Worker创建的对象执行的url脚本必须遵守同源策略
  ```

- 主线程和 worker 线程相互之间使用 `postMessage()` 方法来发送信息, 并且通过 `onmessage` 这个 event handler来接收信息（传递的信息包含在 `Message` 这个事件的`data`属性内) 

  ```js
  worker.postMessage({ num: 1 });	// 将主线程的数据传递到worker线程中，worker线程通过监听并触发Message事件，得到的事件对象中的data属性即该传递的数据
  worker.onmessage = (e) => { console.log(e.data) }
  ```

- Worker 中也可以创建新的 Worker，当然，所有 Worker 必须与其创建者同源。blink内核还暂不支持
