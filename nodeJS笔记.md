## 注意事项

- Web API的大多数都不可用，比如`window`、`document`等
  - 通用的只有==console==、==定时器==
- 虽然没有`window`，但是有等同于`window`的[global]()对象
  - ES新特新中也可以使用[globalThis]()，这两是相等的

## Buffer缓冲器

- 类似于==数组==的`对象`，是用于处理==二进制数据==的内存空间

- ==大小固定==

- 可直接对计算机==内存==进行==操作==

- 每个元素大小为==1byte==(字节)，而一个元素由==8==个==bit==组成

  - 其实是8个==二进制==转换成1个==16进制==显示
  - 转换字符/数组时，是将每个==字符==或数组中==每个元素==在==码表==中的十进制转换成16进制
    - 对于数组中的元素是==对象==或==字符串==的会表示为00

- 创建

  - `Buffer.alloc(num)`：表示创建==num字节==的Buffer

    - 每一个二进制位都会归零

  - `Buffer.allocUnsafe(num)`：使用不安全的方式创建

    - 因为会使用已经被==使用过==的内存空间，且**不会**清空旧数据，就有可能==存在旧数据==

    - 性能更快，因为不用清零

  - `Buffer.from(params)`：将==数组==、==字符串==转换成Buffer

- `b.toString()`：将Buffer按照==utf-8==编码方式转换为==字符串==

- 读写

  - 通过数组下标的方式读取，内容为==十进制==表示方式
  - 通过数组下标的方式修改，只能写十进制
    - **注意**，==最大==只能写255，因为8个二进制数最大只能表示255，多的会溢出，遵循==高位数字舍弃==原则

## fs模块

- **不能**使用`import`方式导入模块

- ==相对路径==参照的是==命令行工作目录==，**不是**JS所在目录
  - 要解决这个问题使用`__dirname`变量，它会保存JS==文件夹==位置的==绝对路径==（不包含文件），如`__dirname + './index.js'`
    - `__filename`是==文件==的==绝对路径==
  
- [writeFile]()：文件写入
  - `fs.writeFile('写入路径','写入内容',回调函数)`
    - 写入路径下没有对应文件会新建
    - writeFile方法写入的内容会==覆盖==源文件内容
    - 回调函数内形参是==失败==后的==对象==，==成功==则返回==nul==l
    - 还有第三个配置项`fs.writeFile('写入路径','写入内容',{flag:'a'},回调函数)`，配置flag值为`a`表示==追加==内容==而不是覆盖==
  - [writeFileSync]()：文件同步写入
    - ==没有回调函数==
    - ==同步==执行，等文件写入完后才会执行后续的代码
  - **只能**在==已有路径目录==下创建文件！
  
- [appendFile]()：文件追加写入
  - `fs.appendFile('路径','内容',回调函数)`
  - ==不会覆盖==文件内容，而是==追加==内容
  - [appendFileSync]()
  - 在需要换行的字符串前使用`\r\n`==换行==

- [createWriteStream]()：文件流式写入

  ```js
  let ws = fs.createWriteStream('文件路径')
  ws.write('内容')
  ws.write('内容2')
  ws.close() 关闭通道 可选 因为脚本执行完毕资源也会被回收 通道会自动关闭
  ```

  - 类似于websocket==推流==的方式，即当前执行脚本和文件之间==连接不断开==，有需要就写人，适合==高频次==写入或者==大文件==写入

  - 同样会==覆盖==源文件内容

- [readFile]()：文件读取

  - `fs.readFile('文件路径',(err,data)=>{})`
    - 有两个参数，第一个依然是错误参数，第二个是读取==成功==的内容
    - 读取到的值是==Buffer==，转换成可读字符串需要`toString`
  - [readFileSync]()：文件同步读取
    - `let data = fs.readFileSync('路径')`
    - 因为是同步，所以没有回调函数，而是直接==返回值==，`data`依然是==Buffer==

- [createReadStream]()：文件流式读取

  ```js
  let rs = fs.createReadStream('路径') 可读取视频等文件
  rs.on('data',chunk => { 必须写 data 绑定的是 data事件 不是 自定义事件名
      事件在 读取完一块 后执行回调
      chunk是读取到的 Buffer数据 不是什么数据都能用toString转 比如读取视频 就无法转换
  })
  rs.on('end',()=>{ 没有参数 同 createWriteStream 可以不绑定end事件 脚本执行完会自动释放
      console.log('读取完成')
  })
  ```

  - 将文件一块一块读取，读取效率高

- ==文件复制==

  ```js
  第一种方法——读写最快、占用内存最小，因为每次读取64KB就写入，不需要将整个文件缓存进内存
  const rs = fs.createReadStream('xxx')
  const ws = fs.createWriteStream('xxx2')
  rs.on('data',chunk=>{
      ws.write(chunk)
  })
  rs.on('end',()=>{
      console.log('写入完成')
  })
  第二种方法——整体读取、写入
  let data = fs.readFileSync('xxx')
  fs.writeFileSync('xxx2',data)
  第三种方法——最简单、用的少
  rs.pipe(ws)
  ```

- [rename]()：文件重命名(也可根据路径移动文件)

  - `fs.rename('源文件路径','重命名路径',回调函数)`
  - [renameSync]()：文件同步重命名
  - **只能**移动文件到==存在的目录下==，不能同时创建文件夹并移动

- [rm]()：文件删除

  - `fs.rm('路径',err=>{})`
  - 同样有[rmSync]()
  - 要删除多层级目录和文件，需要配置项`fs.rm('路径',{recursive:true},err=>{})`，==recursive==，即可递归删除==多级目录和文件==

- [mkdir]()：创建文件夹

  - `fs.mkdir('路径',err=>{})`
  - 当创建路径中存在多个层级不存在时，即需要递归创建路径中有不存在的文件夹，需要第二个参数`fs.mkdir('./a/b/c',{recursive:true},err=>{})`，==recursive==，意为递归，设为==true==即可

- [readdir]()：读取文件夹内文件目录

  - `fs.readdir('路径',(err,data)=>{})`
  - 同样有同步方法[readdirSync]()

- [stat]()：获取文件状态信息(比如：创建时间、大小等)

  - `fs.stat('路径'(err,data)=>{})`
    - data是文件状态==对象==
    - data方法`data.isFile()`，返回true/false，表示是否是文件，是文件夹的话会返回false
    - data方法`data.isDirectory()`，返回true/false，表示是否是文件夹

## path模块

- 用于==拼接==、规范路径格式，`resolve(多个参数)`，从第一个参数往后拼接

  - ==URL对象==里的==pathname==只有`/index.css`单斜线，会省略`../`和`./`，因此path方法处理时会==重新定位到根目录==

  ```js
  const path = require('path')
  path.resolve(__dirname,'index.js') => D:\node\index.js
  path.resolve(__dirname,'./index.js') => D:\node\index.js
  path.resolve(__dirname,'index.js','index2.js') => D:\node\index.js\index2.js
  path.resolve(__dirname,'/index.js') => D:\index.js 注意此处是从根目录开始
  ```

- ==地址拼接==

  - `__dirname + '/../xxx' + pathname`当前==执行js的文件夹为**根目录**==，`/../`表示返回**根目录**的**上一级**，下的`xxx`文件夹下的`pathname`路径
    - 不能用path方法把这个路径整合，会变成`D:\xxx\pathname`


## http模块

```js
const http = require('http')

const server = http.createServer((request, response)=>{ 回调函数 在 *每次*收到请求后都会调用
    第一个参数 request => 浏览器发来的 请求报文 对象，包括 请求行、请求头、请求体
    第二个参数 response => 服务器的 响应报文 对象，可以设置 返回结果
})

listen方法 监听端口、创建服务
server.listen(端口号,()=>{ 回调函数在 服务启动成功 后被调用
    console.log('服务已启动')
})
```

- ==重启==服务==修改==才能==生效==

- [request]()：创建服务传入函数的第一个参数，用于==解析请求报文==

  - `request.method`：属性。获取请求方法，如post、get
  - `request.url`：属性。获取==端口号后==的内容，如`/api?name=xxx`
    - **但是**使用不便，一般不会用这种方法提取参数，而是使用[url模块]()，详情见后
  - `request.on('data', chunk =>{解析})`：读取收到的==请求体==数据。request是==流数据==，所以要绑定==data事件==一片一片取数据
    - **注意**此处只能提取到==post==方式发送的参数，**无法获取**拼接到地址栏的参数，如`?xxx1=123&xxx2=456`
  - `request.on('end', () => {})`：读取完成事件。在回调里执行返回页面结果

- [response]()：创建服务传入函数的第二个参数，用于设置==返回结果==

  - `response.write(内容)`：设置==响应体==，可以==多次==调用，内容会拼接在一起
    - 如果`response.end`也有内容，则也会拼接在一起

  - `response.end(内容)`：设置==响应体==，**并**==断开通信==，**但是**中文显示在页面上会是乱码，需要设置==字符集==
    - `response.end()`只能存在**一个**，不然会报错

    - 不写end，会导致请求一直是==待处理==状态，从而占用资源，以致==其他请求==无法返回结果

  - `end`和`write`均可以接收==Buffer==类型的数据！因此可以将[fs模块]()读取的==html文件==数据传入，从而根据==路径名==返回对应的页面
    - 当页面解析返回的html文件，其内`<img>`、`<link>`等引入了外部文件，就又会发起请求，**并且**是==异步==请求获取外部文件
    - 通过解析==URL对象.[pathname]()==，可以区分是带路由的请求还是加载==外部资源==文件，如，获取`./index.css`文件时发送的请求是`http.../index.css`，而路由的请求是`http.../login?name=xxx`
  - `response.setHeader(键,值)`：设置==响应头==
    - `setHeader('content-type','text/html;charset=utf-8')`，告诉浏览器==返回内容==是HTML，内容对应的==字符集==是UTF-8
    - `setHeader('test',[1, 2, 3])`，第二个参数传入==数组==可以创建==多个同名==请求头
  - `response.statusCode = 404`：**设置**==响应状态码==，但是并不影响请求返回结果
  - `response.statusMessage = 'xxx'`：设置==响应状态描述==，比如==Not Found==就是状态描述

- [listen(端口号，回调函数)]()：监听端口、创建服务

  - ==80==是==http协议==默认端口号，==https协议==默认端口号是==443==，默认端口号不会在地址栏显示

## url模块(旧版)

- 用于解析地址栏url，一般用于提取、解析==query==参数

  ```js
  const url = require('url') 引入url内置模块
  ...
  const server = http.createServer((request,response)=>{
      let res = url.parse(request.url,true) 解析请求行url
      let keyword = res.query.xxx
  })
  ```

- `url.parse(url字符串,true/false)`：解析url字符串，返回==对象==

  - 第一个参数是请求行的url，可以是完整形式`http:...`，也可以是端口号后内容`request.url`
  - 第二个参数是==是否将query属性解析为对象==，可选true/false
    - 解析后的结果`{ name:'xxx',password:'123' }`

## URL API

- 与HTML页面的URL通用
- 与旧版的区别
  - ==无需引入模块==
  - 默认将url携带参数整合成对象
  - 必须传入完整url地址，包括域名、协议、端口，或者在第二个参数中补全域名协议端口号
    - `let url = new URL(request.url,'http://127.0.0.1')`
    - 或者`let url = new URL('http://127.0.0.1' + request.url)`
    - 可以==省略端口号==
  - ==query参数==不是**对象**，需要用==get方法==获取里面的内容
    - `url.searchParams.get('name')`
- URL对象里的`pathname`属性会省略相对路径前的`./`或`../`

## mime资源类型

- 结构`[type]/[subType]`

  - 如`text/html`、`text/css`、`image/jpeg`，`application/json`
  - 在响应头`Content-Type`表明类型，浏览器会根据类型决定如何处理资源
  - 对于未知类型，设置`application/octet-stream`类型，浏览器遇到该类型，会对响应体进行独立存储，即==下载==

- ==获取对应类型==

  ```js
  let mimes = {
      html:'text/html',
      css:'text/css',
      js:'text/javascript',
      png:'image/png',
      jpg:'image/jpeg',
      gif:'image/gif',
      mp4:'video/mp4',
      mp3:'audio/mpeg',
      json:'application/json'
  }
  http.createServer((request,respone)=>{
      let {pathname} = new URL(request.url,'http://127.0.0.1')
      let file_path = __dirname + pathname
      fs.readFile(file_path,(err,data)=>{
          if(err) {
              response.end()
              return
          }
          let t = pathname.split('.')[1]
          获取对应类型
          let type = mimes[t]
          if(type){
              response.setHeader('content-type',type)
          }else{
              没匹配到进行下载
              response.setHeader('content-type','application/octet-stream')
          }
          response.end(data)
      })
  })
  ```

## 模块化

- ==暴露==数据的方式

  - `module.exports`是==固定==写法

  - `exports`和`module.exports`有隐性的关系，`exports = module.exports = {}`，所以`exports.name = xxx`暴露出来的是`{name:xxx}`，且不能用`export = xxx`的方式去暴露

    ```js
    第一种方式——直接赋值变量
    function fn(){
        console.log('hhhh')
    }
    module.exports = fn
    
    第二种方式——用对象整体暴露
    function fn(){}
    let a = '123'
    module.exports = {fn,a} 简写形式 等同于 {fn:fn,a:a}
    
    第三种方式——exports.name
    function fn(){}
    let a = '455'
    exports.fn = fn 等同于 module.exports={fn,a}
    exports.a = a
    ```

- ==导入==模块的方式

  - `require`返回的是`module.exports`的==值==

  - 对于自己创建的模块，**不能**省略==相对路径==

    - `js`、`json`文件可以省略后缀
    - 当导入没写后缀的模块时，会==优先搜索`js`==后缀的文件，如：同一目录下有`module.js`和`module`文件夹，导入`require('./module')`会优先导入`module.js`

  - 导入==文件夹==时，会搜索文件夹下`package.json`里的属性`"main":"./xxx.js"`所对应的文件

    - 如果==main属性==不存在，则会尝试导入`index.js`和`index.json`

  - 导入==内置==模块，无需路径或相对路径，直接写模块名称

    ```js
    第一种方式——导入文件
    const f = require('模块文件路径')
    f()
    
    第二种方式——导入文件夹
    const f = require('./文件夹')
    f()
    
    第三种方式——导入内置模块
    const fs = require('fs')
    ```

  - ==require==原理

    ```js
    伪代码
    function require(file_path){
        拼接绝对路径
        let absolute_path = path.resolve(__dirname,file_path)
        检测缓存
        if(catchs[absolute_path]){
            return catchs[absolute_path]
        }
        如果没有缓存 读取文件代码 从Buffer转换到字符串
        let code = fs.readFileSync(absolute_path).toString()
        自执行读取的代码
        let module = {}
        let exports = module.exports = {}
        (function (module, exports, __dirname, __filename, require){
            code 内部对传入的 module.exports进行了赋值
        })(module, exports, __dirname, __filename, require)
        将module.exports的 值 写入缓存
        return catchs[absolute_path] = module.exports
    }
    ```

## npm

- ==初始化==
  - 以一个文件夹目录启动命令行，执行`npm init`
  - 本质是将当前文件夹==初始化==为`包`，在目标文件夹下生成`package.json`文件
  - 每个`包`都必须要有`package.json`
  - `包`名默认是==文件夹名==，不能是==中文==！所以文件夹最好不要用中文
- [搜索工具包](https://www.npmjs.com)

- ==导入==工具包
  - [require]()函数：`const xx = require('包名')`
  - 导入的包会作为自己所创建的包的依赖包
  - 导入包的流程
    - 会在==当前==文件夹下的`node_modules`寻找同名==文件夹==
    - 没找到则，往==上级目录==下的`node_modules`中寻找，直到磁盘==根目录==
- 生产环境和开发环境
  - 开发依赖：只在==开发阶段==使用的依赖包
    - 命令：`npm -D 包名`
    - 包信息存在`package.json`中==devDependencies==属性
  - 生产依赖：==开发阶段==和最终==上线==都用得到的依赖包
    - 命令：`npm -S 包名`
    - `-S`是默认选项
    - 包信息存在`package.json`中==dependencies==属性
  - ==全局==安装
    - 命令：`npm i -g 包名 `
    - 任何工作目录下均可执行
