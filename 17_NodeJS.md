### 全局对象

#### *setTimeout*

#### *setInterval*

#### *setImmediate*

- 类似于 setTimeout 0

#### *console*

#### *__dirname*

- 获取当前模块所在目录路径
- 并非 global 属性

#### *__filename*

- 获取当前模块的文件路径
- 并非 global 属性

#### *Buffer*

- 类型化数组
- 继承自 Uint8Array

#### *process*

- `cwd()`：
  - 返回当前 nodejs 进程的工作目录
  - 绝对路径
- `exit()`：
  - 强制退出当前 node 进程
  - 可传入退出码，0表示成功退出，默认为0
- `argv`：
  - String[]
  - 获取命令行中的所有参数
- `platform`：
  - 获取当前的操作系统
- `kill(pid)`：
  - 根据进程 id 杀死进程
- `env`：
  - 获取环境变量对象



### Node 的模块化细节

#### 模块的查找

1. 绝对路径：
   - 根据绝对路径直接加载模块
2. 相对路径（./ 或 ../）：
   - 相对于当前模块
   - 转换为绝对路径，并加载模块
3. 相对路径：
   - 检查是否是同名的内置模块，如：fs、path等
   - 检查当前目录中的 node_modules
   - 检查上级目录中的 node_modules 
   - 最终都会转换为绝对路径，并加载模块
4. 关于后缀名：
   - 若不提供后缀名，则 node 自动补全
   - js、json、node、mjs
5. 关于文件名：
   - 若仅提供目录，不提供文件名，则自动寻找该目录下的 index.js 文件
   - 可通过修改 package.json 中的 main 字段来指定上面所寻找的文件，也即包的入口文件

#### module 对象

记录当前模块的信息

#### require 函数

导入模块

#### 注意

*当执行一个模块或使用 require 导入一个模块时，会将模块放置在一个函数环境中执行*



### Node 中的 ES 模块化

- 目前，Node 中的 ES 模块化仍然处于试验阶段
- 模块要么采用 commonjs 的形式，要么采用 ES 的形式，混用会报错
- 当使用 ES 模块化运行时，必须添加 --experimental-modules 标记



### 基本内置模块

#### *os*

- `EOL`
- `arch()`
- `cpus()`（常用）：返回一个对象数组，内容是每个CPU核心的信息
- `freemem()`
- `homedir()`
- `hostname()`
- `tmpdir()`（常用）：返回操作系统默认的缓存文件夹的路

#### *path*

- `basename(path[,ext])`（常用）：返回所给路径的最后一部分，类似于 Unix 的 basename 指令
- `sep`：返回操作系统特定的路径片段分隔符。Windows 上是 `\`，POSIX 上是 `/`
- `delimiter`：返回操作系统特定的路径分隔符。Windows 上是 `;`，POSIX 上是 `:`
- `dirname(path)`（常用）：返回所给路径的目录名，类似于 Unix 的 dirname 指令
- `extname(path)`（常用）：返回所给路径的拓展名
- `join([...paths])`（常用）：将多个所给的路径片段拼接成一个路径（用操作系统特定的分隔符且会规格化路径）
- `normalize(path)`
- `relative(from, to)`
- `resolve([...paths])`（常用）：将一系列的路径或路径片段解析成一个绝对路径

#### *url*

- `URL(input[,base])`：全局构造函数。可将一个 URL 字符串解析成一个 URL 对象

#### *util*

- `callbackify()`
- `inherits()`
- `isDeepStrictEqual(val1, val2)`（常用）：深比较两个值
- `promisfy()`（常用）：将一个回调风格的函数转换为异步 Promise 风格的函数



### 文件 I/O

#### *fs*

- `readFile()`：读取一个文件
- `wrireFile()`：向文件写入内容。文件路径不存在时为新建文件；路径中包含不存在的目录时会报错
- `stat()`：获取文件或目录信息对象。内容包括：
  - `size`：占用字节
  - `atime`：上次访问时间（accessed）
  - `mtime`：上次文件内容被修改时间（modified）
  - `ctime`：上次文件状态被修改时间（changed）
  - `birthtime`：文件创建时间
  - `isDirectory()`：判断是否是目录
  - `isFile()`：判断是否是文件
- `readdir()`：获取目录中的文件和子目录。返回文件名数组
- `mkdir()`：创建目录
- `unlink()`：删除一个文件

#### *fs.promises*



### 文件流

#### Node 基础流类型

- 可读流 Readable：数据从源头流向内存
- 可写流 Writeable：数据从内存流向源头
- 双工流 Duplex：数据既可从源头流向内存又可从内存流向源头

#### 为什么需要流

- 其它介质和内存的数据规模不一致
- 其它介质和内存的数据处理能力不一致

#### 文件流的创建

##### *fs.createReadStream(path[,options])*

用途：创建一个**文件可读流**，用于读取文件内容

参数：

- `path`：读取的文件路径
- `options`：可选配置
  - `encoding`：编码方式
  - `start`：起始字节
  - `end`：结束字节
  - `highWaterMark`：每次读取数量
    - 若 encoding 有值，则该数量表示字符数
    - 若 encoding 为 null，则该数量表示字节数

返回值：Readable 的子类 ReadStream
- **事件**：`rs.on(事件名，处理函数)`
  - `open`：	
    - 文件打开事件
    - 文件被打开后触发
  - `error`：
    - 发生错误时触发
  - `close`：
    - 文件被关闭后触发
    - 可通过 `rs.close()` 手动关闭
    - 或文件读取完之后自动关闭，需要 `autoClose` 配置项配置为 true
  - `data`：
    - 读取到一部分数据后触发
    - 注册 data 事件后，流才会真正开始读取
    - 每次读取 highWaterMark 指定的数量
    - 回调函数中会附带读取到的数据
      - 若指定了编码，则读取到的数据会自动按照编码转换为字符串
      - 若没有指定编码，读取到的数据是Buffer
  - `end`：
    - 所有数据读取完毕后触发
- **方法**：
  - `rs.pause()`：暂停读取，会触发 pause 事件
  - `rs.resume()`：恢复读取，会触发 resume 事件

##### *fs.createWriteStream(path[,options])*

用途：创建一个**文件写入流**，用于写入文件内容

参数：

- `path`：写入的文件路径
- `options`：可选配置
  - `flags`：操作文件的方式
    - `w`：覆盖
    - `a`：追加
    - 其它
  - `encoding`：编码方式
  - `start`：起始字节
  - `highWaterMark`：每次最多写入的字节数

返回值：Writable 的子类 WriteStream
- **事件**：`ws.on(事件名，处理函数)`
  - `open`
  - `error`
  - `close`
  - `drain`：写入流满载荷清空后触发
- **方法**：
  - `ws.write(data)`：
    - 写入一组数据
    - data 可以是字符串或者Buffer
    - 返回一个 Boolean 值
      - true：写入通道没有被填满，接下来的数据可以直接写入，无须排队
      - false：写入通道目前已被填满，接下来的数据将进入写入队列。要特别注意背压问题，即太多的数据读入到内存中的写入队列，期望后续往写入流中添加，由此造成内存空间消耗过大
  - `ws.end([data])`：
    - 结束写入，将自动关闭文件。是否自动关闭取决于 autoClose 配置，其默认为 true
    - data可选，表示关闭前的最后一次写入数据

##### *rs.pipe(ws)*

- 将可读流连接到可写流
- 返回参数的值
- 该方法可解决背压问题



### net 模块

#### 简介

net 模块是一个通信模块，提供了一些异步网络接口，用于创建基于流的*网络通信 TCP* 和*进程间通信 IPC* 的服务器与客户端

#### 创建客户端

##### *net.createConnection(options[,connectListener])*

返回值：`socket`
- socket 是一个特殊的文件，在 node 中表现为一个双工流对象
- 通过向流写入内容发送数据，通过监听流的内容获取数据

#### 创建服务器

##### *net.createServer(\[options\][,connectListener])*

返回值：`server 对象`
- `server.listen(port)`：监听当前计算机中某个端口
- `server.on("listening", ()=>{})`：开始监听端口后触发的事件
- `server.on("connection", socket=>{})`：当某个连接到来时，触发该事件。事件的监听函数会获得一个 socket 对象



### http 模块

#### 简介

http 模块建立在 net 模块之上，使得开发者无须手动管理 socket，无须手动组装消息格式

#### 客户端

##### *http.request(url\[,options\][,callback])*

- 返回值：`http.ClientRequest`
- 发出请求：`ClientRequest` 对象
- 接收响应：`IncomingMessage` 对象（可读流）

#### 服务器

##### *http.createServer(\[options\][,requestListener])*

- 返回值：`http.Server`
- 接收请求（来自客户端）：`IncomingMessage` 对象（可读流）
- 发出响应：`ServerResponse` 对象



### https 模块

#### 本地产生证书（将自身作为权威机构发布证书）

1. 安装 OpenSSL

2. 生成 CA 私钥：

   ```sh
   openssl genrsa -des3 -out ca-pri-key.pem 1024
   # genrsa：密钥对生成算法
   # -des3：使用对称加密算法 des3 对私钥进一步加密。命令运行过程中会让用户输入密码，该密码将作为  des3 算法的key
   # -out ca-pri-key.pem：将加密后的私钥保存到当前目录的 ca-pri-key.pem 文件中。PEM：Privacy-Enhanced Mail
   # 1024：私钥的字节数
   ```

3. 生成 CA 公钥（证书请求）：

   ```sh
   openssl req -new -key ca-pri-key.pem -out ca-pub-key.pem
   # 通过私钥文件 ca-pri-key.pem 中的内容，生成对应的公钥，保存到 ca-pub-key.pem 中
   # 运行过程中要使用之前输入的密码来实现对私钥文件的解密
   # 其他输入信息：Country Name、Province Name、Local Name、Company Name、Unit Name、Common Name...
   ```

4. 生成 CA 证书：

   ```sh
   openssl x509 -req -in ca-pub-key.pem -signkey ca-pri-key.pem -out ca-cert.crt
   # 使用 X.509 证书标准，通过证书请求文件 ca-pub-key.pem 生成证书，并使用私钥 ca-pri-key.pem 加密，然后把证书保存到 ca-cert.crt 文件中
   ```

5. 生成服务器私钥：

   ```sh
   openssl genrsa -out server-key.pem 1024
   ```

6. 生成服务器公钥：

   ```sh
   openssl req -new -key server-key.pem -out server-scr.pem
   ```

7. 生成服务器证书：

   ```sh
   openssl x509 -req -CA ca-cert.crt -CAkey ca-pri-key.pem -CAcreateserial -in server-scr.pem -out server-cert.crt
   ```



#### 本地创建 https 连接

##### *https.createServer(\[options\][, requestListener])*

```js
const server = https.createServer(
  {
    key: fs.readFileSync(path.resolve(__dirname, "./server-key.pem")), // 私钥
    cert: fs.readFileSync(path.resolve(__dirname, "./server-cert.crt")) // 证书
  },
  handler
);
```



### node 生命周期

#### 事件循环

##### *timers*

- 存放计时器的回调函数队列

##### *pending callback*

##### *idle prepare*

##### *poll*

- 轮询队列
- 除了 timers、check 中的回调外，绝大部分异步操作的回调都会放入该队列。例如：文件读取、监听用户请求等
- 运作方式：
  - 若 poll 中有回调。依次执行回调直至清空队列
  - 若 poll 中无回调。等待其他队列中出现回调时，结束该阶段并进入下一阶段；若其他队列也没有回调，则持续等待，直至出现回调为止

##### *check*

- 检查阶段
- 使用 setImmediate 的回调直接进入此队列

##### *close callback*

##### *nextTick 和 Promise 队列*

- *事件循环中，每需要往前推进一步（即执行一个回调）之前，必须先清空 nextTick 和 promise 队列，且 nextTick 优先级高于 promise*



### EventEmitter

- node 事件管理的通用机制
- 内部维护多个事件队列
