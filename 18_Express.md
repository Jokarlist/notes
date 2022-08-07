### Express 的基本使用

```js
const http = require("http");
const express = require("express");
// 创建一个 express 应用。app 实际上是一个用于处理请求的函数
const app = express();
const server = http.createServer(app);
server.listen(2333, () => {});
```

或

```js
const express = require("express");
const app = express();
app.listen(2333, () => {});
```

```js
const express = require("express");
const app = express();
// 配置一个请求映射，若请求方法和请求路径均满足匹配条件，则请求交给处理函数进行处理
// app.请求方法(请求路径, 处理函数)
app.get("/news/:id", (req, res) => {
	// req 和 res 是被 express 封装过的对象，与 http.createServer 中的 handler 不一样
    // req 中有 headers、path、query、params等属性
    const { headers, path, query, params } = req;
    // res.send() 中最后自动调用 end();
    // res.send([1, 2, 3]);
    res.setHeaders("a", "123");
    res.send({ id: 1, name: Alex, age: 18 });
    // res.status() 定义响应状态码，且返回 res 对象，可链式操作
    res.status(302).headers("location", "https://www.baidu.com").end();
    // res.status(302).location("https://www.baidu.com").end();
    // res.redirect(302, "https://www.baidu.com");
});

// 匹配任何 get 请求
app.get("*", (req, res) => {});

const port = 2333;
app.listen(port, () => {});
```



### Express 中间件

#### 概念

当匹配到了请求后，交给第一个处理函数（中间件）处理，若需后续中间件处理，则需要手动的交付

```js
// 该回调函数就称作一个中间件
app.post("/news", (req, res, next) => {
    // 将请求交付下一个中间件处理
    next();
});
```



#### 中间件处理的细节

- 若后续已没有中间件，且express 发现响应还未结束（即未调用 *res.end()*），则 express 会响应 *404 状态*
- 若中间件处理过程发生了错误（如抛出错误），不会停止服务器，相当于调用 *next(错误对象)*，则其会寻找后续的错误处理中间件，若无则响应 *500 状态*



### 常用内置中间件

#### *express.static()*

发放静态资源，服务基于 [serve-static](https://www.npmjs.com/package/serve-static)

#### *express.urlencoded()*

对请求的 urlencoded 类型的载荷做解析处理，服务基于 [body-parser](https://www.npmjs.com/package/body-parser)

#### *express.json()*

对请求的 json类型的载荷做解析处理，服务基于 [body-parser](https://www.npmjs.com/package/body-parser)