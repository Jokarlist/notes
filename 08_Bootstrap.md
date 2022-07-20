## 栅格系统

- 栅格系统是一个基于12列的布局，有5种默认的响应式层级，Sass变量和mixins，以及大量的预定于样式类，工作原理如下：

  - `.container`实现固定的宽度并居中呈现，`.container-fluid`实现全宽度，并和其它网格实现对齐（即使网页能够以100%的宽度呈现在所有的浏览器窗口或设备尺寸上）
  - 行（`.row`）是列（`.col-*`）的横向组合和父容器（它们有效组织在`.row`下），每列都有水平的padding值（称作`gutter`），用于控制它们之间的间隔（创建列与列之间的间隙），同时在负margin的行上抵消，从而实现列中的所有内容在视觉上都是左侧对齐的体验
  - 网页开发者的呈现内容必须放在列（`.col-*`）中，而且只有列可以是行的直接子元素，否则都是违法的
  - `.col-*`后面可以有不同的数字，如`.col-sm-4`或`.col-xl-12`，这些样式类后面的数字用于表明定义div空间想要占用列的数量，每行最多有12列。如果你想用三个等宽的列，则取12的三分之一，即`.col-sm-4`就是正确的
  - `.col-*`的`width`属性（即列宽）是用百分比来表现和定义的，所以它们总是流式的，其尺寸大小受父元素的定义影响（这正是弹性盒布局的特征，子元素的宽比和排列受父元素定义）
  - `.row`上带有`margin-left: -15px; margin-right: -15px`样式，你可以在`.row`上定义`.no-gutters`属性，从而消除这个属性，使页面不会额外宽出30px，即`<div class="row no-gutters"...`
  - 总共有5个栅格等级，每个响应式分界点隔出一个等级：特小`.col-*`、小`.col-sm-*`、中`.col-md-*`、大`.col-lg-*`、特大`.col-xl-*`
  - 栅格断点的媒体查询基于最小宽度，这意味着它们适用于该断点及其上方的所有断点（例如：`col-sm-4`适用于小`sm`，中`md`，大`lg`，特大`xl`的设备，但不适用于特小`xs`）
  - 用户不需要自行定义网格，可以直接使用系统预定义的栅格类（如`.col-4`）或采用[Sass mixins](https://getbootstrap.com/docs/4.6/layout/grid/#sass-mixins)来进行更多的语义标记满足开发需要

- **栅格选项**（`.container`前提下）：

  - `xl`为特大屏。屏幕宽度>=1200px，容器的宽度固定为1140px，一行可以设置12个列。屏幕宽度<1200px时，一行只能设置1列：

    ```html
    <div class="container">
    	<div class="row">
    		<div class="col-xl-3"></div>
    		<div class="col-xl-3"></div>
    		<div class="col-xl-3"></div>
    		<div class="col-xl-3"></div>
    	</div>
    </div>
    ```

  - `lg`为大屏。屏幕宽度>=992px，容器的宽度固定为960px，一行可以设置12个列。屏幕宽度<992px时，一行只能设置1列：

    ```html
    <div class="row">
    	<div class="col-lg-4"></div>
        <div class="col-lg-4"></div>
        <div class="col-lg-4"></div>
    </div>
    ```

  - `md`为中屏。屏幕宽度>=768px，容器的宽度固定为720px，一行可以设置12个列。屏幕宽度<768px时，一行只能设置1列：

    ```html
    <div class="row">
    	<div class="col-md-6"></div>
        <div class="col-md-6"></div>
    </div>
    ```

  - `sm`为小屏。屏幕宽度>=576px，容器的宽度固定为540px，一行可以设置12个列。屏幕宽度<576px时，一行只能设置1列：

    ```html
    <div class="row">
    	<div class="col-sm-3"></div>
        <div class="col-sm-3"></div>
        <div class="col-sm-3"></div>
        <div class="col-sm-3"></div>
    </div>
    ```

  - `xs`为特小屏。屏幕宽度<576px时，容器的宽度为auto，一行总是能设置12列：

    ```html
    <div class="row">
    	<div class="col-4"></div>
        <div class="col-4"></div>
        <div class="col-4"></div>
    </div>
    ```

- **等宽布局**：

  - 设置等宽列，平分宽度，通过类`.col`：

    ```html
    <div class="row">
    	<div class="col">等宽列1</div>
        <div class="col">等宽列2</div>
        <div class="col">等宽列3</div>
    </div>
    ```

- **等宽多行**：

  - 设置多行等宽列，在期望断开的地方添加一个类`.w-100`的div，能够让后面的列换行：

    ```html
    <div class="row">
    	<div class="col">等宽列1</div>
        <div class="col">等宽列2</div>
        <div class="w-100"></div>
        <div class="col">等宽列3</div>
        <div class="col">等宽列4</div>
    </div>
    ```

- **设置一列宽度，剩下宽度自动平分**

  - ```html
    <div class="row">
    	<div class="col-sm-7">在小屏幕下占7列</div>
        <div class="col">自动平分剩余的宽度</div>
        <div class="col">自动平分剩余的宽度</div>
        <div class="col">自动平分剩余的宽度</div>
    </div>
    ```

- **可变宽度的弹性空间**：

  - 根据内容的自然宽度调整列的宽度，使用类`.col-{breakpoint}-auto`：

    ```html
    <div class="row">
    	<div class="col-md-auto">在中等屏幕下由内容撑开宽度</div>
        <div class="col">自动平分剩余的宽度</div>
        <div class="col-lg-2">在大屏幕下占2列</div>
    </div>
    ```

- **所有尺寸下，都占同样的列数**：

  - 使用类`.col-*`：

    ```html
    <div class="row">
        <div class="col-8">所有尺寸下都占8列</div>
        <div class="col-4">所有尺寸下都占4列</div>
    </div>
    ```

- **混合布局**：

  - ```html
    <!--
    	1. 特大屏幕下一行显示6个div，一个div占2列
    	2. 大屏幕下一行显示4个div，一个div占3列
    	3. 中等屏幕下一行显示3个div，一个div占4列
    	4. 小屏幕下一行显示2个div，一个div占6列
    	5. 特小屏幕下一行显示1个div，一个div占12列
    -->
    <div class="row">
        <div class="col-xl-2 col-lg-3 col-md-4 col-md-6 col-12"></div>
        <div class="col-xl-2 col-lg-3 col-md-4 col-md-6 col-12"></div>
        <div class="col-xl-2 col-lg-3 col-md-4 col-md-6 col-12"></div>
        <div class="col-xl-2 col-lg-3 col-md-4 col-md-6 col-12"></div>
        <div class="col-xl-2 col-lg-3 col-md-4 col-md-6 col-12"></div>
        <div class="col-xl-2 col-lg-3 col-md-4 col-md-6 col-12"></div>
    </div>
    ```

- **对齐**：

  - *垂直对齐*：
    - 整行的对齐方式：
      - `align-items-start`：顶对齐
      - `align-items-center`：中间对齐
      - `align-items-end`：底对齐
    - 列的单独对齐方式：
      - `align-self-start`：顶对齐
      - `align-self-center`：中间对齐
      - `align-self-end`：底对齐

  - *水平对齐*：
    - `justify-content-start`：左对齐
    - `justify-content-center`：中间对齐
    - `justify-content-end`：底对齐
    - `justify-content-around`：分散居中对齐（每个元素的两侧的间距是相等的）
    - `justify-content-between`：左右两端对齐（元素之间的间距自动平分）

- **列排序**：

  - 使用类`.order-{breakpoint}-*`：

    ```html
    <div class="row">
        <div class="col">第1列</div>
        <div class="col order-2">第2列</div>
        <div class="col order-3">第3列</div>
    </div>
    <div class="row mt-5">
        <div class="col">第1列</div>
        <!-- 只有当屏幕尺寸>=1200px时，才会进行排序 -->
        <div class="col order-xl-3">第2列</div>
        <div class="col order-xl-2">第3列</div>
    </div>
    <div class="row mt-5">
        <div class="col">第1列</div>
        <!-- order-first代表排在第一位，order-last表示排在最后一位 -->
        <div class="col order-first">第2列</div>
        <div class="col order-last">第3列</div>
        <div class="col">第4列</div>
    </div>
    ```

  - 3.x版本使用`.col-{breakpoint}-push-*`、`.col-{breakpoint}-pull-*`来排序

- **列偏移**：

  - 使用类`.offset-{breakpoint}-*`：

    ```html
    <div class="row">
        <div class="col-md-4">第1列</div>
        <div class="col-md-4 offset-md-4">往右偏移4列</div>
    </div>
    <div class="row mt-5">
        <div class="col-md-3 offset-md-3">第1列</div>
        <div class="col-md-3 offset-md-3">第2列</div>
    </div>
    <div class="row mt-5">
        <div class="col-sm-5 col-md-6">小屏占5列，中屏占6列</div>
        <div class="col-sm-5 offset-sm-3 col-md-6 offset-md-5">小屏偏移3列，中屏偏移5列</div>
    </div>
    ```

- **间距**：

  - 使用margin工具可以让列之间产生间距：

    - `mr-{breakpoint}-auto`：使列右侧隔离出距离
    - `ml-{breakpoint}-auto`：使列左侧隔离出距离

    ```html
    <div class="row">
        <div class="col-md-4">第1列</div>
        <div class="col-md-4 ml-auto">第2列，位于行最右侧</div>
    </div>
    <div class="row mt-5">
        <div class="col-md-3 ml-md-auto">在中屏下，离左侧距离自动计算</div>
        <div class="col-md-3 ml-md-auto">在中屏下，离左侧距离自动计算</div>
    </div>
    <div class="row mt-5">
        <div class="col-auto mr-auto">宽度由内容撑开</div>
        <div class="col-auto">宽度由内容撑开，位于行最右侧</div>
    </div>
    ```

- **嵌套**：

  - 每一个列中可以继续添加行，此行中的列会以父级行的宽度为基准，分为12个列：

    ```html
    <div class="row">
        <div class="col-sm-9">
        	父级的第1列
            <div class="row">
            	<div class="col-sm-8 col-6">子级的第1列，小屏下占8列，特小屏下占6列</div>
                <div class="col-sm-4 col-6">子级的第2列，小屏下占4列，特小屏下占6列</div>
            </div>
        </div>
        <div class="col-sm-3">父级的第2列</div>
    </div>
    ```



## 重置、排版、代码

- **重置**：
  - *页面默认值*：
    - 全局性的将每一个元素的`box-sizing`属性设置为`border-box`
    - `<html>`根元素不声明`font-size`，但被假定为16px大小，然后在此基础上采用`font-size: 1rem`的比例应用于`<body>`上，使媒体查询能够轻松的实现缩放，最大程序保障用户偏好和易于访问
    - `<body>`被赋予一个全局性的`font-family`、`line-height`及`text-align`，其下面的诸多表单元素继承了这些属性，以防止字体大小错位冲突
    - `<body>`的`background-color`默认值设置为`#fff`
  - *标题和段落*：
    - 所有的标题和段落元素的`margin-top`被移除，标题元素添加`margin-bottom: .5rem`，段落元素添加`margin-bottom: 1rem`
  - *列表*：
    - 所有列表元素的`margin-top`移除，添加`margin-bottom: 1rem`，但嵌套的子列表元素没有`margin-bottom`
    - 描述列表元素也更新了margin，`<dd>`重置`margin-left: 0`且添加`margin-bottom: .5rem`，`<dt>`设置`font-size: bold`
  - *预格式化文本*：
    - `<pre>`移除了`margin-top`属性，并将`rem`作为其`margin-bottom`的单位
  - *表格*：
    - 合并了边框`border-collapse: collapse`，格式化了`<caption>`，且确保`text-align`属性一致
    - 其它与`border`，`padding`相关的改变详见于`.table`类章节
  - *表单*：
    - `<fieldset>`去除了`border`，`padding`，`margin`属性，所以它们可以轻松地用作单一的输入框或者输入框组的放入容器中使用
    - `<legend>`和fieldset字段集一样，也已被重新设计过，显示为不同种类的标题
    - `<label>`添加了 `display: inline-block`属性，所以可被添加margin样式
    - `<input>`、 `<select>`、 `<textarea>`、 `<button>`基本本来都被规范化处理了，但 Reboot 移除了它们的`margin`，并且设置了`inline-height: inherit`
    - `<textarea>`被修改为只能竖直方向上调整大小，因为水平方向上调整大小经常会“破坏”页面布局
  - *其它杂项*：
    - `<blockquote>`应用块默认的`margin: 1em 40px`被重置为`margin: 0 0 1rem`，使其与其它元素更一致
    - `<abbr>`内联元素接受基本的样式，使其在段落文本中突出
    - `<summary>`上的`cursor`默认为`text`，被重置为`pointer`
- **排版**：
  - 
