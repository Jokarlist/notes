## clean-webpack-plugin 清除输出目录

- 一个用于清除构建（build）文件夹的webpack插件

- 默认情况下，这个插件会在webpack每一次重新构建之后清除webpack的output.path路径指示的文件夹（默认是dist文件夹）下的所有文件，以及所有没使用上的webpack资源

- 原理（猜想）：在触发emit钩子函数的时候，通过fs模块删除output.path路径指示的文件夹

- 使用：

  ```js
  const { CleanWebpackPlugin } = require("clean-webpack-plugin");
  
  module.exports = {
      output: {
          filename: "[name].[chunkhash:5].js"
      },
  	plugins: [
          new CleanWebpackPlugin()
      ]
  };
  ```

  

## html-webpack-plugin 自动生成页面

- 一个为webpack构建后的源代码提供引用其的html文件的插件

- 原理（猜想）：在触发emit钩子函数的时候，利用fs模块生成一个html页面文件，在文件内容的合适位置上添加一个script元素，元素的src路径从webpack最终输出所参照的资源列表中获取资源的名字，也即文件输出路径

- 使用（例子）：

  ```js
  const HtmlWebpackPlugin = require('html-webpack-plugin');
  
  module.exports = {
      entry: {	// 配置了两个chunk，则下面插件的引用可以引用两次，以创建两个不同的插件实例即生成两
          		// 个页面；也可以只引用一次，则生成的页面会引入两个chunk最终输出的两个js文件
          home: "./src/index.js",
          a: "./src/a.js"
      },
      output: {	// 最终构建生成的文件的文件名规则，也可以使用如下的文件夹规则
          filename: "scripts/[name].[chunkhash:5].js"
      },
      plugins: [
          new CleanWebpackPlugin(),
          new HtmlWebpackPlugin({
              template: "./public/index.html",	// 可指定文件作为生成的页面的模板文件，一般
              									// 放置在public文件夹下
              filename: "home.html",	// 生成页面的名字，默认为index.html
              chunks: ["home"]	// 生成页面依据的chunk
          }),
          new HtmlWebpackPlugin({
              template: "./public/index.html",
              filename: "a.html",
              chunks: ["a"]
          })
      ]
  }
  ```
  
  

## copy-webpack-plugin 复制静态资源

- 复制已存在的独立文件或文件夹到构建（build）文件夹

- 使用：

  ```js
  const CopyPlugin = require("copy-webpack-plugin");
  
  module.exports = {
  	plugins: [
  		new CopyPlugin({
              patterns: [
                  {from: "./public", to: "./"}	// 将public文件夹下的文件全部复制到构建文件
                  								// 夹，且to的相对路径也是构建文件夹
              ]
          })       
      ]
  }
  
  // 复制过去的文件若是与构建生成文件存在重复的文件名，则该插件会自动处理，不将该冲突文件复制过去
  // 该特性在最新版本已被废弃使用
  ```
  
  

## webpack-dev-server 开发中服务器

- 在开发阶段，打包、运行、调试过程过于繁琐，回顾一下操作流程：
  1. 编写代码
  2. 控制台运行命令完成打包
  3. 打开页面查看效果
  4. 继续编写代码，回到步骤2

- 并且，我们往往希望把最终打包的代码和页面部署到服务器上，来模拟真实环境。为解决这些问题，webpack官方制作了一个单独的库：`webpack-dev-server`，既不是plugin也不是loader

- 使用：
  1. npm安装
  2. 执行 `webpack-dev-server` 命令

- `webpack-dev-server`命令几乎支持所有的webpack命令参数，如--config、--env等，当执行`webpack-dev-server`命令时，相当于进行以下操作：

  1. 内部执行webpack命令，传递命令参数
  2. 开启watch监控
  3. 注册hooks：类似plugin，webpack-dev-server会向webpack中注册一些钩子函数，主要功能：
     1. 将资源列表（assets）保存起来
     2. 禁止webpack输出文件
  4. 用express开启一个服务器，监听某个端口，当有请求到达后，根据请求的路径，给予相应的资源

- 常用配置：

  - port：配置监听端口

  - proxy：配置代理，常用于跨域访问

  - stats：配置控制台输出内容

    ```js
    module.exports = {
    	devServer: {
    		port: 8080,
    		open: true,
    		proxy: {
                "/api": "http://open.duyiedu.com"
                // 请求地址与正则表达式/api相匹配时，将协议、域名、端口段的url更改为
                // 上述所配置的，即webpack自动去请求一个更改后的地址，因其基于node为
                // 服务器端，不存在跨域问题，所以可以解决浏览器的跨域访问问题
                // 如此配置，只改变url，请求头的内容不发生改变，若接收请求的服务器对请
                // 求头有特殊的要求，则可采用下述写法来配置新请求头
                "/api": {
    				target: "http://open.duyiedu.com",
                	changeOrigin: true // 更改请求头中的host和origin字段
            	}
            },
        	stats: {
    			modules: false,
                colors: true
            }
    	}
    }
    ```

    

## file-loader url-loader 普通文件处理

- file-loader：生成所依赖的文件到输出目录，文件名也即页面引入该文件时所使用的路径，由loader的配置产生，模块最终会将其导出

  ```js
  // file-loader
  function loader(source) {
      // source：文件内容（图片内容的话即用buffer表示）
      // 1. 生成一个具有相同文件内容的文件到输出目录
      // 2. 返回一段代码 export default "文件名"
  }
  ```

- url-loader：将所依赖的文件转换为一个base64格式的字符串并最终导出

  ```js
  // url-loader
  function loader(source) {
      // source：文件内容（图片内容的话即用buffer表示）
      // 1. 根据buffer生成一个base64编码字符串
      // 2. 返回一段代码 export default "base64编码"
  }
  ```

- 配置参照webpack loader的配置，具体的options字段配置查看各自的文档
  - url-loader的配置中options字段中的limit配置进行base64编码的最大限定字节数，若超过了所设定的最大值则内部将其交给file-loader进行处理

## 解决路径问题

- 在使用file-loader或url-loader时，可能会遇到一个有趣的路径问题，例如，通过webpack打包的目录结构如下：

  ```sh
  dist
      |—— img
          |—— a.png  # file-loader生成的文件，export default "img/a.png"，则
       			   # main.js读取不到这个文件
      |—— scripts
          |—— main.js	# html-webpack-plugin生成的文件，下同
      |—— pages
          |—— index.html # <script src="../scripts/main.js" ></script>
  ```

- 这种问题发生的根本原因：模块中的路径来自于某个loader或plugin，当产生路径时，loader或plugin只有相对dist目录的路径，并不知道该路径将在哪个资源（因为该资源可能还未生成）中使用，从而无法确定最终正确的路径

- 针对这种情况，需要依靠webpack的配置 `pulicPath` 解决：

  - `pulicPath`本质上是一个字符串，打包前的代码中使用 `__webpack_pulic_path__` 可获取，其会在webpack内部转换为存放在打包后的 `__webpack_require__.p` 中
  - 一般配置的值会是 `/` ，配置之后一些需要用到其的插件会读取它，然后一般会将其与该插件生成的文件名（即文件路径）字符串拼接在一起，就构成一个绝对路径
  - 而页面在引用这个资源时，除协议、域名、端口部分的url不变以外，后面的部分则变成这个绝对路径，便可以解决这个路径问题
  - 该配置字段也可以不用配置在webpack的配置下，可以配置在一些loader或plugin的配置中，具体是否可以配置参考其文档

## webpack内置插件

- 所有的webpack内置插件都作为webpack的静态属性存在，使用下面的方式创建一个插件对象

  ```js
  const webpack = require("webpack");
  
  new webpack.插件名(options);
  ```

- **DefinedPlugin**：

  - 全局常量定义插件：使用该插件通常定义一些常量值，例如：

    ```js
    new webpack.DefinedPlugin({
    	PI: `Math.PI`,	// PI = Math.PI
        VERSION: `"1.0.0"`,	// VERSION = "1.0.0"
        DOMAIN: JSON.stringify("gdut.edu")	// DOMAIN = ""gdut.edu""
    })
    ```

  - 这样配置之后，我们便可以在源码中直接使用插件中提供的常量，当webpack编译完成后，会自动替换为所配置的常量的值

- **BannerPlugin**：

  - 可以为每个chunk生成的文件头部添加一行注释，一般用于添加作者、公司、版权等信息

    ```js
    new webpack.BannerPlugin({
        banner: `
        	hash:[hash]
        	chunkhash:[chunkhash]
        	name:[name]
        	author:kuen heung
        	corporation: gdut
        `
    })
    ```

- **ProvidePlugin**：

  - 自动加载模块，而不必到处import或require

    ```js
    new webpack.ProvidePlugin({
    	$: "jquery",
    	_: "lodash"
    })
    ```

    然后便可以在任意源码中：

    ```js
    $("#item");
    _.drop([1, 2, 3], 2);
    ```

    
