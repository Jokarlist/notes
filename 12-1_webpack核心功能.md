## 如何在浏览器端实现模块化

- **遇到问题**：

  - 效率问题：精细的模块划分带来了更多的JS文件，更多的JS文件带来了更多的请求，降低了页面访问效率
  - 兼容性问题：浏览器目前仅支持ES6的模块化标准，并且还存在兼容性问题
  - 工具问题：浏览器不支持npm下载的第三方包
  - 以上问题只是前端工程化的一个缩影，当开发一个具有一定规模的程序，会遇到各种各样的非业务问题，涵盖：执行效率、兼容性、代码的可维护性和可拓展性、团队协作、测试等等。这些非工程问题虽与业务无关，但若是处理不好会严重地影响开发进度

  - 上述问题在node端就没有像在浏览器端这么严重，是因为在node端运行的js文件在本地，node可以有接口直接读取，效率比浏览器远程请求文件的效率高得多

- **根本原因**：在浏览器端，开发时态和运行时态的侧重点不一样：
  - 开发时态（devtime）：
    1. 模块划分越细越好
    2. 支持多种模块化标准
    3. 支持npm或其它包管理器下载的模块
    4. 能够解决其它工程化的问题
  - 运行时态（runtime）：
    1. 文件越少越好
    2. 文件体积越小越好
    3. 代码内容越乱越好
    4. 所有浏览器都要兼容
    5. 能够解决其它运行时的问题，主要是执行效率问题
- **解决办法**：
  - 既然开发时态和运行时态面临的局面有巨大的差异，因此需要有一个工具，这个工具能够让开发者专心的在开发时态写代码，然后利用这个工具将开发时态编写的代码转换为运行时态需要的东西
  - 这样的工具称为构建工具，常见的有：
    - webpack
    - grunt
    - gulp
    - browserify
    - fis
    - 其它



## webpack的安装和使用

- **webpack简介**：
  - webpack是基于模块化的打包（构建）工具，它把一切视为模块。它通过一个开发时态的入口模块为起点，分析出所有的依赖关系，然后经过一系列的过程（压缩、合并），最终生成运行时态的文件
  - webpack的特点：
    - 为前端工程化而生：webpack致力于解决前端工程化，特别是浏览器端工程化中遇到的问题，让开发者集中注意力编写业务代码，而把工程化过程中的问题全部交给webpack来处理
    - 简单易用：支持零配置，可以不用写任何一行额外的代码就使用webpack
    - 强大的生态：webpack是非常灵活、可以扩展的，webpack本身的功能并不多，但它提供了一些可以扩展其功能的机制，使得一些第三方库可以融于到webpack中
    - 基于nodejs：由于webpack在构建的过程中需要读取文件，因此它是运行在node环境中的
    - 基于模块化：webpack在构建过程中要分析依赖关系，方式是通过模块化导入语句进行分析的，它支持各种模块化标准，包括但不限于CommonJS、ES6 Module

- **webpack的安装**：

  - webpack通过npm安装，它提供了两个包：
    - webpack：核心包，包含了webpack构建过程中要用到的所有api
    - webpack-cli：提供一个简单的cli命令，它调用了webpack核心包的api，来完成构建过程
  - 安装方式：
    - 全局安装：可以全局使用webpack命令，但是无法为不同项目对应不同的webpack版本
    - 本地安装：推荐方式，每个项目都使用自己的webpack版本进行构建

- **webpack使用**：

  - 默认情况下，webpack会以 `./src/index.js` 作为入口文件分析依赖关系，打包到 `./dist/main.js` 中

  - 通过 `--mode` 选项可以控制webpack的打包的运行环境

    ```sh
    # 生产环境下的打包
    webpack --mode=production
    # 开发环境下的打包
    webpack --mode=development
    ```



## 模块化兼容性

- 由于webpack同时支持CommonJS和ES6 module，因此需要理解它们互操作时webpack是如何处理的

- **同模块化标准**：

  - 如果导出和导入使用的是同一种模块化标准，打包后的效果和之前学习的模块化没有任何差异：

    <img src="https://gitee.com/dev-edu/frontend-webpack-particular/raw/master/1.%20webpack%E6%A0%B8%E5%BF%83%E5%8A%9F%E8%83%BD/1-3.%20%E6%A8%A1%E5%9D%97%E5%8C%96%E5%85%BC%E5%AE%B9%E6%80%A7/assets/2020-01-07-07-50-09.png" style="zoom: 55%">
    <img src="https://gitee.com/dev-edu/frontend-webpack-particular/raw/master/1.%20webpack%E6%A0%B8%E5%BF%83%E5%8A%9F%E8%83%BD/1-3.%20%E6%A8%A1%E5%9D%97%E5%8C%96%E5%85%BC%E5%AE%B9%E6%80%A7/assets/2020-01-07-07-53-45.png" style="zoom: 55%">

- **不同模块化标准**：

  - 不同的模块化标准，webpack按照如下的方式处理：

    <img src="https://gitee.com/dev-edu/frontend-webpack-particular/raw/master/1.%20webpack%E6%A0%B8%E5%BF%83%E5%8A%9F%E8%83%BD/1-3.%20%E6%A8%A1%E5%9D%97%E5%8C%96%E5%85%BC%E5%AE%B9%E6%80%A7/assets/2020-01-07-07-54-25.png" style="zoom: 55%">

    <img src="https://gitee.com/dev-edu/frontend-webpack-particular/raw/master/1.%20webpack%E6%A0%B8%E5%BF%83%E5%8A%9F%E8%83%BD/1-3.%20%E6%A8%A1%E5%9D%97%E5%8C%96%E5%85%BC%E5%AE%B9%E6%80%A7/assets/2020-01-07-07-55-54.png" style="zoom: 55%">



## 编译结果分析

- 将入口文件index.js和模块文件a.js用webpack编译所得结果main.js文件的基本实现：

  ```js
  ((modules) => {
    // 用于缓存模块的导出结果
    const moduleCache = {};
  
    function require(moduleID) {
      // 判断是否该模块已有缓存
      if (moduleCache[moduleID]) {
        return moduleCache[moduleID];
      }
      // 用于存放该模块的运行结果
      const module = {
        exports: {},
      };
      // 运行该模块
      modules[moduleID](module, module.exports, require);
      // 缓存该模块运行的结果
      moduleCache[moduleID] = module.exports;
  
      return module.exports;
    }
  
    // 运行require函数相当于运行一个模块，得到模块导出结果
    require("./src/index.js");
  })({
    "./src/a.js": function (module, exports) {
      /* console.log("module a");
      module.exports = "a"; */
  
      // 用eval函数来执行可以让浏览器提供一个新的环境执行其中的代码，若代码出
      // 错则方便调试
      eval(
        'console.log("module a");\nmodule.exports = "a";\n //# sourceURL=./src/a.js'
      );
    },
    "./src/index.js": function (module, exports, require) {
      /* console.log("module index");
  
      const a = require("./src/a.js");
  
      console.log(a); */
  
      eval(
        'console.log("module index");\nconst a = require("./src/a.js");\nconsole.log(a);\n //# sourceURL=./src/index.js'
      );
    },
  });
  ```




## 配置文件

- 默认情况下，webpack会读取 `webpack.config.js` 文件作为配置文件，也可以通过CLI参数 `--config` 来指定使用某个配置文件
- 配置文件中通过CommonJS模块化导出一个对象，对象中的各种属性对应不同的webpack配置
- 注意：配置文件中的代码，必须是能在node环境下运行的有效代码
- 基本配置：
  - `mode`：编译模式。字符串，取值为development或production，指定编译结果代码运行的环境，会影响webpack对编译结果代码格式的处理
  - `entry`：入口。字符串，指定入口文件
  - `output`：出口。对象，指定编译结果文件



## devtool配置

- **source map 源码地图**：
  - 因为前端工程化后运行的是对源代码进行合并、压缩、转换等操作后的代码，不利于阅读，当遇到问题时调试就会非常麻烦，则有一门技术便是能方便开发者对源代码中的错误进行调试，即source map
  - source map实际上一个配置文件，配置中不仅记录了所有源代码的内容，还记录了和转换后的代码的对应关系
- **最佳实践**：
  1. source map 应在开发环境中使用，作为一种调试手段
  2. source map 不应该在生产环境中使用，source map 的文件一般较大，不仅会导致额外的网络传输，还容易暴露原始代码。即使要在生产环境中使用source map用于调试真实的代码运行问题，也要做出一些处理规避网络运输和代码暴露的举措
- **webpack中的source map**：
  - 使用 webpack 编译后的代码难以调试，可以通过配置文件中的 `devtool` 配置字段来优化调试体验



## 编译过程

- webpack编译（构建、打包）过程分三个步骤：

  1. **初始化**：

     - 此阶段，webpack会将`CLI参数、配置文件、默认配置`进行融合，形成一个最终的配置对象
     - 对配置的处理过程是依托一个第三方库`yargs`完成的

  2. **编译**：

     1. 创建chunk：

        - chunk是webpack在内部构建过程中的一个概念，译为块，它表示通过某个入口找到的所有依赖的统称

        - 根据入口模块（默认为`./src/index.js`）创建一个chunk

          <img src="https://gitee.com/dev-edu/frontend-webpack-particular/raw/master/1.%20webpack%E6%A0%B8%E5%BF%83%E5%8A%9F%E8%83%BD/1-8.%20%E7%BC%96%E8%AF%91%E8%BF%87%E7%A8%8B/assets/2020-01-09-11-54-08.png" style="zoom: 60%">

        - 每个chunk都有至少两个属性：
          - name：默认为main
          - id：唯一编号。开发环境下和name相同，生产环境下是一个从0开始的数字

     2. 构建所有依赖模块：


     <img src="https://gitee.com/dev-edu/frontend-webpack-particular/raw/master/1.%20webpack%E6%A0%B8%E5%BF%83%E5%8A%9F%E8%83%BD/1-8.%20%E7%BC%96%E8%AF%91%E8%BF%87%E7%A8%8B/assets/2020-01-09-12-32-38.png">

     - 过程简图：

     <img src="https://gitee.com/dev-edu/frontend-webpack-particular/raw/master/1.%20webpack%E6%A0%B8%E5%BF%83%E5%8A%9F%E8%83%BD/1-8.%20%E7%BC%96%E8%AF%91%E8%BF%87%E7%A8%8B/assets/2020-01-09-12-35-05.png" style="zoom: 65%">

     - 该步骤完成之后，chunk会产生一个模块列表，列表中包括了模块id和模块转换后的代码

     - 产生chunk assets：

        - webpack会根据配置为chunk生成一个资源列表，即 `chunk assets` ，资源列表可以理解为是用于存储最终生成文件的文件名和文件内容的清单

          <img src="https://gitee.com/dev-edu/frontend-webpack-particular/raw/master/1.%20webpack%E6%A0%B8%E5%BF%83%E5%8A%9F%E8%83%BD/1-8.%20%E7%BC%96%E8%AF%91%E8%BF%87%E7%A8%8B/assets/2020-01-09-12-39-16.png">

        - chunk hash 是根据chunk assets的内容生成的一个hash字符串，hash是一种算法，有很多种形式，特点是将一个任意长度的字符串转换为一个固定长度的字符串，且可以保证原始内容不变，产生的hash字符串就不变

        - 过程简图：

          <img src="https://gitee.com/dev-edu/frontend-webpack-particular/raw/master/1.%20webpack%E6%A0%B8%E5%BF%83%E5%8A%9F%E8%83%BD/1-8.%20%E7%BC%96%E8%AF%91%E8%BF%87%E7%A8%8B/assets/2020-01-09-12-43-52.png">

     - 合并chunk assets：

        - 将多个chunk的assets合并到一起，并产生一个总的hash：

          <img src="https://gitee.com/dev-edu/frontend-webpack-particular/raw/master/1.%20webpack%E6%A0%B8%E5%BF%83%E5%8A%9F%E8%83%BD/1-8.%20%E7%BC%96%E8%AF%91%E8%BF%87%E7%A8%8B/assets/2020-01-09-12-47-43.png" style="zoom: 60%">

  3. **输出**：

     - webpack利用node中的fs模块，根据编译产生的总的assets，生成相应的文件

       <img src="https://gitee.com/dev-edu/frontend-webpack-particular/raw/master/1.%20webpack%E6%A0%B8%E5%BF%83%E5%8A%9F%E8%83%BD/1-8.%20%E7%BC%96%E8%AF%91%E8%BF%87%E7%A8%8B/assets/2020-01-09-12-54-34.png" style="zoom: 65%">

- **总过程**：

  <img src="https://gitee.com/dev-edu/frontend-webpack-particular/raw/master/1.%20webpack%E6%A0%B8%E5%BF%83%E5%8A%9F%E8%83%BD/1-8.%20%E7%BC%96%E8%AF%91%E8%BF%87%E7%A8%8B/assets/2020-01-09-15-51-07.png">

- **涉及术语**：
  - module：模块，分割的代码单元。webpack中的模块可以是任何内容的文件，不仅限于JS
  - chunk：webpack内部构建模块的块。一个chunk中包含多个模块，这些模块是从入口模块通过依赖分析得来的
  - bundle：chunk构建好模块后会生成chunk的资源列表，列表中的每一项就是一个bundle，可以认为bundle就是最终生成的文件
  - hash：最终的资源列表所有内容联合生成的hash值
  - chunk hash：chunk生成的资源列表内容联合生成的hash值
  - chunk name：chunk的名称，如果没有配置则使用main
  - id：通常指chunk的唯一编号，如果在开发环境下构建，和chunk name相同；如果是生产环境下构建，则使用一个从0开始的数字进行编号



## 入口和出口

- **前置知识（关于node）**：
  - `./`的不同含义：
    1. 模块化代码中，例如require("./xxx")，`./`表示当前js文件所在的目录
    2. 在路径处理中，`./`表示node运行目录（即启用终端键入node命令的那个目录）
  - `__dirname`：所有情况下，都表示当前运行的js文件所在的目录，是一个绝对路径
  - `path.resolve()`：该方法将一系列的路径或路径片段解析为绝对路径

- **出口**：

  - 出口是针对资源列表的文件名或路径的配置
  - 出口是通过配置文件中的`output`字段来配置

- **入口**：

  - 入口真正配置的是chunk

  - 入口通过配置文件中的`entry`字段来配置

    ```js
    const path = require("path");
    
    module.exports = {
        mode: "development",
        entry: {
        	// 为chunk main和chunk a分别配置入口
            // 属性名：chunk的名称，属性值：入口模块（启动模块）
            main: "./src/index.js",
            a: ["./src/a.js", "./src/index.js"] // 启动模块有两个
        },
        output: {
            // path表示资源放置的文件夹，必须配置一个绝对路径，默认值为dist（此处简略，实际为绝对路径）
            path: path.resolve(__dirname, "target"),
            filename: "[name].[chunkhash:5].js" // 配置最终构建生成的文件的文件名规则，下述
        },
        devtool: "source-map"
    };
    ```

- **规则**：
  - `name`：使用chunkname
  - `hash`：使用总的资源的hash，使用其通常是为了解决缓存问题。因为浏览器请求一次文件后会缓存，下一次若继续需要请求同名文件，会直接使用缓存的文件，则会造成文件内容发生更改而文件名未变化的情况下所预期的浏览器页面变化不对
  - `chunkhash`：使用单个chunk的hash，同样为了解决缓存问题，只不过不会让所有chunk的hash都改变，而是会只改变内容确切变化了的chunk的hash
  - `id`：使用chunkid，不推荐



## 入口和出口的最佳实践

- **一个页面一个js**：

  - <img src="https://gitee.com/dev-edu/frontend-webpack-particular/raw/master/1.%20webpack%E6%A0%B8%E5%BF%83%E5%8A%9F%E8%83%BD/1-10.%20%E5%85%A5%E5%8F%A3%E5%92%8C%E5%87%BA%E5%8F%A3%E7%9A%84%E6%9C%80%E4%BD%B3%E5%AE%9E%E8%B7%B5/assets/2020-01-10-12-00-28.png" style="zoom: 60%">

  - 源码结构：

    ```sh
    |—— src
        |—— pageA	页面A的代码目录
            |—— index.js 页面A的启动模块
            |—— ...
        |—— pageB	页面B的代码目录
            |—— index.js 页面B的启动模块
            |—— ...
        |—— pageC	页面C的代码目录
            |—— main1.js 页面C的启动模块1 例如：主功能
            |—— main2.js 页面C的启动模块2 例如：实现访问统计的额外功能
            |—— ...
        |—— common	公共代码目录
            |—— ...
    ```

  - webpack配置：

    ```js
    module.exports = {
        entry:{
            pageA: "./src/pageA/index.js",
            pageB: "./src/pageB/index.js",
            pageC: ["./src/pageC/main1.js", "./src/pageC/main2.js"]
        },
        output:{
            filename:"[name].[chunkhash:5].js"
        }
    };
    ```

  - 这种方式适用于页面之间的功能差异巨大、公共代码较少的情况，这种情况下打包出来的最终代码不会有太多重复

  - 缺点是公共代码较多时会增加额外的网络传输，但不会影响代码的可维护性（因为维护的代码是源代码，最终运行的代码是构建后的代码）

- **一个页面多个js**：

  - <img src="https://gitee.com/dev-edu/frontend-webpack-particular/raw/master/1.%20webpack%E6%A0%B8%E5%BF%83%E5%8A%9F%E8%83%BD/1-10.%20%E5%85%A5%E5%8F%A3%E5%92%8C%E5%87%BA%E5%8F%A3%E7%9A%84%E6%9C%80%E4%BD%B3%E5%AE%9E%E8%B7%B5/assets/2020-01-10-12-38-03.png" style="zoom: 60%">

  - 源码结构：

    ```sh
    |—— src
        |—— pageA	页面A的代码目录
            |—— index.js 页面A的启动模块
            |—— ...
        |—— pageB	页面B的代码目录
            |—— index.js 页面B的启动模块
            |—— ...
        |—— statistics	用于统计访问人数功能目录，独立于页面A、B
            |—— index.js 启动模块
            |—— ...
        |—— common	公共代码目录
            |—— ...
    ```

  - webpack配置：

    ```js
    module.exports = {
        entry:{
            pageA: "./src/pageA/index.js",
            pageB: "./src/pageB/index.js",
            statistics: "./src/statistics/index.js"
        },
        output:{
            filename:"[name].[chunkhash:5].js"
        }
    };
    ```

  - 这种方式适用于页面之间有一些**独立**、相同的功能，专门使用一个chunk抽离这部分JS有利于浏览器更好的缓存这部分内容

- **单页应用**：

  - 所谓单页应用，是指整个网站（或网站的某一个功能块）只有一个页面，页面中的内容全部靠JS创建和控制。 vue和react都是实现单页应用的利器

    <img src="https://gitee.com/dev-edu/frontend-webpack-particular/raw/master/1.%20webpack%E6%A0%B8%E5%BF%83%E5%8A%9F%E8%83%BD/1-10.%20%E5%85%A5%E5%8F%A3%E5%92%8C%E5%87%BA%E5%8F%A3%E7%9A%84%E6%9C%80%E4%BD%B3%E5%AE%9E%E8%B7%B5/assets/2020-01-10-12-44-13.png" style="zoom: 60%">

  - 源码结构：

    ```sh
    |—— src
        |—— subFunc	子功能目录
            |—— ...
        |—— subFunc	子功能目录
            |—— ...
        |—— common	公共代码目录
            |—— ...
        |—— index.js
    ```

  - webpack配置：

    ```js
    module.exports = {
        entry: "./src/index.js",
        output:{
            filename:"index.[hash:5].js"
       }
    };
    ```




## loader

- loader本质上是一个函数，它的作用是将某个源码字符串转换成另一个源码字符串返回

  <img src="https://gitee.com/dev-edu/frontend-webpack-particular/raw/master/1.%20webpack%E6%A0%B8%E5%BF%83%E5%8A%9F%E8%83%BD/1-11.%20loader/assets/2020-01-13-10-39-24.png" style="zoom: 60%">

  函数中的this作为上下文会被webpack填充，并且其中包含一些实用的方法

- loader函数将在模块解析的过程中被调用，已得到最终的源码

- chunk中解析模块的更详细过程：

  <img src="https://gitee.com/dev-edu/frontend-webpack-particular/raw/master/1.%20webpack%E6%A0%B8%E5%BF%83%E5%8A%9F%E8%83%BD/1-11.%20loader/assets/2020-01-13-09-35-44.png" style="zoom: 60%">

- 处理loaders流程：

  <img src="https://gitee.com/dev-edu/frontend-webpack-particular/raw/master/1.%20webpack%E6%A0%B8%E5%BF%83%E5%8A%9F%E8%83%BD/1-11.%20loader/assets/2020-01-13-10-29-54.png" style="zoom: 60%">

- loader配置：

  - 建立相关的loaders文件，然后在webpack配置文件中配置module字段，引入相关的loader模块

  - 完整配置：

    ```js
    module.exports = {
        mode: "development",
        module: {	// 针对模块的配置，目前版本只有两个配置字段，rules和noParse
            rules: [	// loader模块匹配规则，可以存在多个规则
                // 每个规则是一个对象
                {	// 规则1
                    test: /index\.js$/, // 正则表达式，匹配模块的路径
                    use: [	// 匹配的模块所需要应用的加载器loader
                        {	// 其中一个loader
                            loader: "模块路径", // loader模块的路径，该字符串会被放置到
                            				   // require函数中，因此要注意路径描述
                            // 若options字段中传递的额外参数数量少，也可以将其简写在loader字段
                            // 中，即：
                            // loader: "模块路径?额外参数名=额外参数值"
                            options: {	// 向对应的loader模块传递的额外参数，存储在loader模块
                                		// 中的this里面
                                changeVar: "变量"
                            }
                        }
                    ]
                },
                {	// 规则2
                    test: /\.js$/,
                    // 当use字段中配置的规则模块还有以下的简写法，将loader字段和options字段结合
                    // 作为一个字符串书写在use数组中
                    use: ["./loaders/loader3", "./loaders/loader4"]
                }
            ],
            noParse:	// 是否不要解析某个模块
        }
    }
    ```



## plugin

- **背景**：

  - loader的功能定位是转换代码，而一些其它的操作难以使用loader完成，比如：
    - 当webpack生成文件时，顺便多生成一个说明描述文件
    - 当webpack编译启动时，控制台输出一句话表示webpack启动了
    - 当xxx时，xxx
    
  - 类似这种的功能表现为需要把功能嵌入到webpack的编译流程中，而这种事情的实现是依托于plugin的

    <img src="https://gitee.com/dev-edu/frontend-webpack-particular/raw/master/1.%20webpack%E6%A0%B8%E5%BF%83%E5%8A%9F%E8%83%BD/1-14.%20plugin/assets/2020-01-15-12-45-16.png" style="zoom: 60%">

    

- plugin的本质是一个带有apply方法的对象：

  ```js
  const plugin = {
  	apply(compiler) {}
  };
  
  // 通常，习惯上会将该对象写成构造函数的模式
  class MyPlugin {
  	apply(compiler) {}
  }
  
  const plugin = new MyPlugin();
  ```

- 要将插件应用于webpack，需要把插件对象配置到webpack的plugins数组中，如下：

  ```js
  module.exports = {
  	plugins: [
          new MyPlugin();
      ]
  }
  ```

- apply函数会在初始化阶段，创建好compiler对象后运行。运行时将compiler对象作为参数传入

- compiler对象是在初始化阶段创建的，整个webpack打包期间只有一个compiler对象，后续完成打包工作的是compiler对象内部创建的compilation对象

  <img src="https://gitee.com/dev-edu/frontend-webpack-particular/raw/master/1.%20webpack%E6%A0%B8%E5%BF%83%E5%8A%9F%E8%83%BD/1-14.%20plugin/assets/2020-01-15-12-49-26.png" style="zoom: 60%">

  - compiler和compilation的区别：compilation由compiler生成，compiler在单次打包期间（执行了一次webpack打包命令）只会生成一次，compilation则在单次打包期间内，若源文件有变化会导致其重新生成，再进行打包流程

- compiler对象提供了大量的钩子函数（hooks，可以理解为事件），plugin的开发者可以注册这些钩子函数，参与webpack编译和输出。在apply方法中注册钩子函数：

  ```js
  class MyPlugin {
  	apply(compiler) {
          compiler.hooks.事件名称.事件类型(name, compilation => {
  			// 事件处理函数
          });
      }
  }
  ```

  - 事件名称：即要监听的事件名也即钩子名，[查阅文档](https://www.webpackjs.com/api/compiler-hooks)
  - 事件类型：这一部分使用的是Tapable API，这个小型的库是一个专门用于钩子函数监听的库。它提供了一些事件类型：
    - `tap`：注册一个同步的钩子函数，函数运行完毕则表示事件处理结束
    - `tapAsync`：注册一个基于回调的异步的钩子函数，函数通过调用一个回调表示事件处理结束，事件处理函数传参compilation和callback
    - `tapPromise`：注册一个基于Promise的异步的钩子函数，函数通过返回的Promise进入已决状态表示事件处理结束
  - 事件处理函数：处理函数有一个事件参数`compilation`



## 区分环境

需要针对生产环境和开发环境分别书写webpack配置的时候，可以：

- 针对不同环境分别书写配置文件，使用时在webpack命令中指定所需的配置文件

- 考虑到webpack允许配置不仅可以是一个对象，还可以是一个函数，即：

  ```js
  module.exports = (env) => {
  	return {
          // 配置内容
      }
  }
  ```

  - 在开始构建时，webpack若发现配置是一个函数，则会调用该函数，将函数返回的对象作为配置内容。因此，开发者可以根据不同的环境返回不同的对象

  - 在webpack调用函数时，会向其传入一个参数env，该参数的值来自于webpack命令中给env指定的值，例如：

    ```sh
    npx webpack --env abc	# env: "abc"
    npx webpack --env.abc	# env: {abc: true}	webpack5不再支持这种写法
    npx webpack --env.abc=1	# env: {abc: 1}	webpack5不再支持这种写法
    npx webpack --env.abc=1 --env.bcd=2	# env: {abc: 1, bcd: 2}	webpack5不再支持这种写法
    ```

  - 如此，开发者便能在命令中指定环境，在配置文件的代码中进行判断，根据环境返回不同的配置结果



## 其它细节配置

- **context**：该配置会影响入口和loaders的解析，入口和loaders的相对路径会以context的配置作为基准路径，如此则配置会独立于CWD（current working directory 当前工作目录）

  ```js
  context: path.resolve(__dirname, "app");
  // 入口和loaders的相对路径为 当前工作目录/app
  ```

- **output**：

  - `library`：输出一个暴露入口点导出的库

    ```js
    output: {
    	library: "abc"
        // 这样打包后的结果中，会将自执行函数的执行结果暴露给全局变量abc
    }
    ```

  - `libraryTarget`：该配置可以更加精细的控制如何暴露入口包的导出结果。值有：

    - `var`：默认值。暴露给一个普通变量
    - `window`：暴露给window对象的一个属性
    - `this`：暴露给this的一个属性
    - `global`：暴露给global的一个属性
    - `commonjs`：暴露给exports的一个属性
    - 其它：https://webpack.docschina.org/configuration/output/#outputlibrarytarget

- **target**：设置打包结果最终要运行的环境。常用值有：

  - `web`：默认值。打包后的代码运行在web环境中
  - `node`：打包后的代码运行在node环境中
  - 其它：https://webpack.docschina.org/configuration/target/#target

- **module**：

  - `noParse`：不解析正则表达式匹配的模块，通常用它来忽略那些大型的单模块库（打包后的不存在依赖），以提高构建性能

    ```js
    module: {
    	noParse: /jquery/
    }
    ```

- **resolve**：resolve的相关配置主要用于控制模块解析过程

  - `modules`：

    ```js
    modules: ["node_modules"]	// 默认值
    ```

    当解析模块时，如遇到导入语句require("test")，webpack会从下面的位置寻找依赖的模块

    1. 当前目录下的 node_modules 目录
    2. 上级目录下的 node_modules 目录
    3. ...

  - `extensions`：

    ```js
    extensions: [".js". ".json"]	// 默认值
    ```

    当解析模块时，遇到无具体后缀的导入语句require("test")，会依次按extensions的配置来测试它的后缀名：test.js --> test.json

    注意：用于打包的源代码中出现的无具体后缀的导入语句require("./xxx")，是由webpack根据extensions字段的配置自动补全后缀的，而不是node

  - `alias`：

    ```js
    alias: {
    	"@": path.resolve(__dirname, "src"),
        "_": __dirname
    }
    ```

    导入语句中可以加入alias中配置的键名，例如require("@/abc.js")，webpack会将其看作是require(src的绝对路径+"abc.js")

    在大型系统中，源码结构往往比较深和复杂，别名配置可以让我们更加方便的导入依赖

- **externals**：

  ```js
  externals: {
  	jquery: "$",
      lodash: "_"
  }
  ```

  从最终的bundle中排除掉配置的源码，例如，入口模块为：

  ```js
  //index.js
  require("jquery");
  require("lodash");
  ```

  生成的bundle是：

  ```js
  (function () {
      ...
  })({
      "./src/index.js": function(module, exports, __webpack_require__){
          __webpack_require__("jquery");
          __webpack_require__("lodash");
      },
      "jquery": function(module, exports){
          //jquery的大量源码
      },
      "lodash": function(module, exports){
          //lodash的大量源码
      },
  });
  ```

  externals有了如上面的配置后，生成的bundle则变成：

  ```js
  (function(){
      ...
  })({
      "./src/index.js": function(module, exports, __webpack_require__){
          __webpack_require__("jquery");
          __webpack_require__("lodash");
      },
      "jquery": function(module, exports){
          module.exports = $;
      },
      "lodash": function(module, exports){
          module.exports = _;
      },
  });
  ```

  这比较适用于一些第三方库来自于外部CDN的情况，这样一来，即可以在页面中使用CDN，又让bundle的体积变得更小，还不影响源码的编写

- **stats**：控制的是构建过程中控制台的输出的统计内容
