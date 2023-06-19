## 注意事项

- Web API 的大多数都不可用，比如`window`、`document`等

  - 通用的只有==console==、==定时器==

- 虽然没有`window`，但是有等同于`window`的[global]()对象

  - ES 新特新中也可以使用[globalThis]()，这两是相等的

- 想要使用==ES6==模块语法，需要在`package.json`里添加`"type":"module"`配置项

  - 但是这样就无法使用`require(path)`方法，会报错
  - 并且没有`__dirname`全局变量了，需要根据控制台==当前==目录，使用`import.meta.url`作为替代

- > 流程
  >
  > <center>express开启服务、设置路由规则。决定如何返回静/动态资源、如何处理请求</center>
  >
  > <center>↓</center>
  >
  > <center>Mongodb/MySQL开启服务、在服务器负责存储数据</center>
  >
  > <center>↓</center>
  >
  > <center>Mongoose等工具包封装了操作数据库的命令、用代码形式便捷的操作数据库数据</center>
  >
  > <center>↓</center>
  >
  > <center>在Mongoose等工具包方法的回调中用express以接口需要的形式给页面返回数据</center>

## Buffer 缓冲器

- 类似于==数组==的`对象`，是用于处理==二进制数据==的内存空间

- ==大小固定==

- 可直接对计算机==内存==进行==操作==

- 每个元素大小为==1byte==(字节)，而一个元素由==8==个==bit==组成

  - 其实是 8 个==二进制==转换成 1 个==16 进制==显示
  - 转换字符/数组时，是将每个==字符==或数组中==每个元素==在==码表==中的十进制转换成 16 进制
    - 对于数组中的元素是==对象==或==字符串==的会表示为 00

- 创建

  - `Buffer.alloc(num)`：表示创建==num 字节==的 Buffer

    - 每一个二进制位都会归零

  - `Buffer.allocUnsafe(num)`：使用不安全的方式创建

    - 因为会使用已经被==使用过==的内存空间，且**不会**清空旧数据，就有可能==存在旧数据==

    - 性能更快，因为不用清零

  - `Buffer.from(params)`：将==数组==、==字符串==转换成 Buffer

- `b.toString()`：将 Buffer 按照==utf-8==编码方式转换为==字符串==

- 读写

  - 通过数组下标的方式读取，内容为==十进制==表示方式
  - 通过数组下标的方式修改，只能写十进制
    - **注意**，==最大==只能写 255，因为 8 个二进制数最大只能表示 255，多的会溢出，遵循==高位数字舍弃==原则

## fs 模块

- **不能**使用`import`方式导入模块

- ==相对路径==参照的是==命令行工作目录==，**不是**JS 所在目录
  - 要解决这个问题使用`__dirname`变量，它会保存 JS==文件夹==位置的==绝对路径==（不包含文件），如`__dirname + './index.js'`
    - `__filename`是==文件==的==绝对路径==
- [writeFile]()：文件写入
  - `fs.writeFile('写入路径','写入内容',回调函数)`
    - 写入路径下没有对应文件会新建
    - writeFile 方法写入的内容会==覆盖==源文件内容
    - 回调函数内形参是==失败==后的==对象==，==成功==则返回==nul==l
    - 还有第三个配置项`fs.writeFile('写入路径','写入内容',{flag:'a'},回调函数)`，配置 flag 值为`a`表示==追加==内容==而不是覆盖==
    - 写入内容**只能**是==字符串==！如果收到的是二进制文件，是不能写入文件的！
  - [writeFileSync]()：文件同步写入
    - ==没有回调函数==
    - ==同步==执行，等文件写入完后才会执行后续的代码
  - **只能**在==已有路径目录==下创建文件！
- [appendFile]()：文件追加写入
  - `fs.appendFile('路径','内容',回调函数)`
  - ==不会覆盖==文件内容，而是==追加==内容
  - [appendFileSync]()
  - 在需要换行的字符串前使用`\r\n`==换行==
  - 写入路径下没有对应文件会新建
- [createWriteStream]()：文件流式写入

  ```js
  let ws = fs.createWriteStream('文件路径')
  ws.write('内容')
  ws.write('内容2')
  ws.close() 关闭通道 可选 因为脚本执行完毕资源也会被回收 通道会自动关闭
  ```

  - 类似于 websocket==推流==的方式，即当前执行脚本和文件之间==连接不断开==，有需要就写人，适合==高频次==写入或者==大文件==写入

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
    - data 是文件状态==对象==
    - data 方法`data.isFile()`，返回 true/false，表示是否是文件，是文件夹的话会返回 false
    - data 方法`data.isDirectory()`，返回 true/false，表示是否是文件夹

## path 模块

- 用于==拼接==、规范路径格式，`resolve(多个参数)`，从第一个参数往后拼接

  - ==URL 对象==里的==pathname==只有`/index.css`单斜线，会省略`../`和`./`，因此 path 方法处理时会==重新定位到根目录==

  ```js
  const path = require('path')
  path.resolve(__dirname,'index.js') => D:\node\index.js
  path.resolve(__dirname,'./index.js') => D:\node\index.js
  path.resolve(__dirname,'index.js','index2.js') => D:\node\index.js\index2.js
  path.resolve(__dirname,'/index.js') => D:\index.js 注意此处是从根目录开始
  ```

- ==地址拼接==

  - `__dirname + '/../xxx' + pathname`当前==执行 js 的文件夹为**根目录**==，`/../`表示返回**根目录**的**上一级**，下的`xxx`文件夹下的`pathname`路径
    - 不能用 path 方法把这个路径整合，会变成`D:\xxx\pathname`

## http 模块

```js
const http = require('http')
// createServer接收一个函数作为参数 所以可以使用express()返回的函数对象作为参数
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
  - `request.method`：属性。获取请求方法，如 post、get
  - `request.url`：属性。获取==端口号后==的内容，如`/api?name=xxx`
    - **但是**使用不便，一般不会用这种方法提取参数，而是使用[url 模块]()，详情见后
  - `request.on('data', chunk =>{解析})`：读取收到的==请求体==数据。request 是==流数据==，所以要绑定==data 事件==一片一片取数据

    - **注意**此处只能提取到==post==方式发送的参数，**无法获取**拼接到地址栏的参数，如`?xxx1=123&xxx2=456`
    - `chunk`是==Buffer==类型数据
    - 请求体数据`toString()`后得到的结果

      ![image-20230509172920446](C:/Users/admin/AppData/Roaming/Typora/typora-user-images/image-20230509172920446.png)

  - `request.on('end', () => {})`：读取完成事件。在回调里执行返回页面结果
- [response]()：创建服务传入函数的第二个参数，用于设置==返回结果==

  - `response.write(内容)`：设置==响应体==，可以==多次==调用，内容会==拼接==在一起
    - 如果`response.end`也有内容，则也会拼接在一起
    - 如果不写`response.end`设置的响应内容==不会返回==给页面，必须要有`end`！
  - `response.end(内容)`：设置==响应体==，**并**==断开通信==，**但是**中文显示在页面上会是乱码，需要设置==字符集==

    - `response.end()`只能存在**一个**，不然会报错

    - 不写 end，会导致请求一直是==待处理==状态，从而占用资源，以致==其他请求==无法返回结果

    - ※end 执行完不会终止，会继续向后执行

  - `end`和`write`均可以接收==Buffer==类型的数据！因此可以将[fs 模块]()读取的==html 文件==数据传入，从而根据==路径名==返回对应的页面
    - 当页面解析返回的 html 文件，其内`<img>`、`<link>`等引入了外部文件，就又会发起请求，**并且**是==异步==请求获取外部文件
    - 通过解析==URL 对象.[pathname]()==，可以区分是带路由的请求还是加载==外部资源==文件，如，获取`./index.css`文件时发送的请求是`http.../index.css`，而路由的请求是`http.../login?name=xxx`
  - `response.setHeader(键,值)`：设置==响应头==
    - `setHeader('content-type','text/html;charset=utf-8')`，告诉浏览器==返回内容==是 HTML，内容对应的==字符集==是 UTF-8
    - `setHeader('test',[1, 2, 3])`，第二个参数传入==数组==可以创建==多个同名==请求头
  - `response.statusCode = 404`：**设置**==响应状态码==，但是并不影响请求返回结果
  - `response.statusMessage = 'xxx'`：设置==响应状态描述==，比如==Not Found==就是状态描述

- [listen(端口号，回调函数)]()：监听端口、创建服务

  - ==80==是==http 协议==默认端口号，==https 协议==默认端口号是==443==，默认端口号不会在地址栏显示

## url 模块(旧版)

- 用于解析地址栏 url，一般用于提取、解析==query==参数

  ```js
  const url = require('url') 引入url内置模块
  ...
  const server = http.createServer((request,response)=>{
      let res = url.parse(request.url,true) 解析请求行url
      let keyword = res.query.xxx
  })
  ```

- `url.parse(url字符串,true/false)`：解析 url 字符串，返回==对象==

  - 第一个参数是请求行的 url，可以是完整形式`http:...`，也可以是端口号后内容`request.url`
  - 第二个参数是==是否将 query 属性解析为对象==，可选 true/false
    - 解析后的结果`{ name:'xxx',password:'123' }`

## util模块

- 用于将回调函数风格的方法转变成Promise形式

- **只能**传入==错误优先==风格的回调函数，即`fn(params1, params2, ..., (err,data)=>{})`

- 应用

  ```js
  const fs = require('fs')
  const util = require('util')
  // 封装成新的函数方法
  let newFsRead = util.promisify(fs.readFile)
  newFsRead('./src/text.txt').then(data=>{})
  ```

- 内部实现逻辑

  ```js
  // 传入一个函数作为参数 返回一个函数
  function promisify(fn) {
      // 返回的函数可接收多个参数 params是数组
      return (...params) => {
          return new Promise((success, fail) => {
              // 将参数数组展开 补全最后一个回调函数参数
              fn(...params, (err, data) => {
                  if (err) fail(err)
                  success('ok')
              })
          })
      }
  }
  // 实验函数
  function testFn(params1, params2, callback1, callback2) {
      console.log(params1)
      console.log(params2)
      // 关键点 如果有多个回调函数作为参数 必须 携带最后一个回调函数作为参数传入
      callback1(params1 + params2, callback2)
  }
  // 加工旧方法 返回一个新的函数
  let newTestFn = promisify(testFn)
  // 新方法传参 只传最后一个回调函数之前的
  newTestFn(5, 2, (result, callback) => {
      let t = null
      if (result > 5) {
          t = 'error'
      }
      // 关键点 执行最后一个回调函数 并按错误在前 数据在后顺序传入参数
      callback(t, result)
  }).then(res => {
      console.log(res)
  }).catch( res => {
      console.log(res)
  })

## URL API

- 与 HTML 页面的 URL 通用
- 与旧版的区别
  - ==无需引入模块==
  - 默认将 url 携带参数整合成对象
  - 必须传入完整 url 地址，包括域名、协议、端口，或者在第二个参数中补全域名协议端口号
    - `let url = new URL(request.url,'http://127.0.0.1')`
    - 或者`let url = new URL('http://127.0.0.1' + request.url)`
    - 可以==省略端口号==
  - ==query 参数==不是**对象**，需要用==get 方法==获取里面的内容
    - `url.searchParams.get('name')`
- URL 对象里的`pathname`属性会省略相对路径前的`./`或`../`
- `url.hostname`：IP 域名。不包括 http 协议及端口号

## mime 资源类型

- 结构`[type]/[subType]`

  - 如`text/html`、`text/css`、`image/jpeg`，`application/json`
  - 在响应头`Content-Type`表明类型，浏览器会根据类型决定如何处理资源
  - 对于未知类型，设置`application/octet-stream`类型，浏览器遇到该类型，会对响应体进行独立存储，即==下载==

- ==获取对应类型==

  ```js
  let mimes = {
  	html: "text/html",
  	css: "text/css",
  	js: "text/javascript",
  	png: "image/png",
  	jpg: "image/jpeg",
  	gif: "image/gif",
  	mp4: "video/mp4",
  	mp3: "audio/mpeg",
  	json: "application/json",
  };
  http.createServer((request, respone) => {
  	let { pathname } = new URL(request.url, "http://127.0.0.1");
  	let file_path = __dirname + pathname;
  	fs.readFile(file_path, (err, data) => {
  		if (err) {
  			response.end();
  			return;
  		}
  		let t = pathname.split(".")[1];
  		获取对应类型;
  		let type = mimes[t];
  		if (type) {
  			response.setHeader("content-type", type);
  		} else {
  			没匹配到进行下载;
  			response.setHeader("content-type", "application/octet-stream");
  		}
  		response.end(data);
  	});
  });
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

  - `require`本身就会运行一遍文件

    ```js
    // 如果需要的是文件执行后得到的对象
    // index.js文件
    let dataType = new mg.Schema({...});
    let model = mg.model('test2', dataType);
    module.exports = model;
    // index2.js文件
    const model = require('./index.js')
    // 在某事件回调中
    model.create(...).then(...)

    // 如果需要的是函数，选择时机执行
    // index.js文件
    module.exports = function (success, error) {
    	mg.connect(...);
    	mg.connection.once('open', () => {
    		console.log('连接成功');
    		success();
    	});
    	mg.connection.on('error', () => {
    		if (typeof error !== 'function') {
    			error = () => {
    				console.log('连接失败');
    			};
    		}
    		error();
    	});
    	mg.connection.on('close', () => {
    		console.log('连接关闭');
    	});
    };
    // index2.js文件
    const connect = require('./index.js')
    connect(()=>{...})

    ```

  - 对于自己创建的模块，**不能**省略==相对路径==

    - `js`、`json`文件可以省略后缀
    - 当导入没写后缀的模块时，会==优先搜索`js`==后缀的文件，如：同一目录下有`module.js`和`module`文件夹，导入`require('./module')`会优先导入`module.js`

  - 导入==文件夹==时，会搜索文件夹下`package.json`里的属性`"main":"./xxx.js"`所对应的文件

    - 如果==main 属性==不存在，则会尝试导入`index.js`和`index.json`

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

    - require 实际在内部执行了一次[fs 模块]()文件读取，再转字符串

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

  - require**只能**导入==JSON==和==JS==文件

    - 导入 txt 等格式文件会以 JS 格式去运行
    - 导入==JSON==文件，返回的就是 JSON 文件保存的对象，可以直接进行取值操作
    - 导入==JS==文件，会先运行一遍，再返回`module.exports`的值。如果 JS 文件没有给`module.exports`赋值，则返回`{}`

## npm

- ==初始化==
  - 以一个文件夹目录启动命令行，执行`npm init`
  - 本质是将当前文件夹==初始化==为`包`，在目标文件夹下生成`package.json`文件
  - 每个`包`都必须要有`package.json`
  - `包`名默认是==文件夹名==，不能是==中文==！所以文件夹最好不要用中文
  - 意义：创建==项目==必须的操作，因为直接 npm 安装，`package.json`里只有`dependencies`属性
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
  - 普通(==局部==)安装
    - 命令：`npm i 包名`
    - 会一层层向上级目录查找`node_modules`文件夹，因此不必在根目录下执行安装命令，在子文件夹下安装即可
    - 执行安装的工具包
      - 第一种方法：`path\node_modules\.bin\指令 -v`
      - 第二种方法：
        - 配置 package.json 文件中`script`属性别名，`"name":"nodemon -v"`
        - 再执行`npm run name`
  - ==全局==安装
    - 命令：`npm i -g 包名 `
    - 任何工作目录下均可执行安装
    - 无需`require`函数，而是通过命令行指令去调用，如：`nodemon`，安装完后自动运行，需要调用指令时`nodemon xxx`去执行
    - 查看全局安装位置：`npm root -g`
    - 执行安装的工具包：`指令 -v`
  - 切换安装版本：`npm i 包名@版本号`
  - 删除依赖
    - 局部：`npm remove 包名`或`npm r 包名`
    - 全局：`npm remove -g 包名`
  - 配置别名
    - `package.json`文件中有个`scripts`属性，在其中添加==自定义名==，如`"server": "node 执行路径及额外参数"`，即可使用`npm run server`运行，省去了冗余的命令参数
    - 也可以在`script`属性中配置`start: "node 执行路径"`，同样可以使用`npm run start`运行，不过 start 可以省略 run`npm start`
    - `npm run`会自动向上级目录查找`package.json`
  - NodeJs==版本管理==软件：[nvm]()
    - 显示可下载`Node.js`版本：`nvm list available`
    - 已安装版本：`nvm list`
    - 安装某一指定版本：`nvm install 16.11.12`
    - 安装最新版本：`nvm install latest`
    - ==删除==某版本：`nvm uninstall 16.11.12`
    - ==**切换**==版本：`nvm use 16.11.12`
- Tips
  - ==nodemon==工具的使用，用`nodemon ./xx.js`代替`node ./xx.js`
  - 开发环境中的`node_modules`文件夹，是不会上传服务器的，主要是为了去除冗余的安装包文件。所以协同开发时，都是下载`package.json`和`package-lock.json`文件，根据里面的`dependence`属性重新在本地安装依赖包，而`npm i`命令可以快捷安装 json 里所有依赖

## express 框架

- 也是一个==工具包==，封装了多个功能，便于开发 HTTP 服务

  - 导入的工具包是**函数**，使用前需创建==函数对象==，`const app = express()`，`app`其实是一个函数，所以可以传入`http或https.createServer`

- 与 node 原生 http 模块区别

  ```js
  // http模块
  const http = require("http");
  const server = http.createServer((req, res) => {
  	let url = new URL(req.url, "http://127.0.0.1");
  	switch (url.pathname) {
  		case "/":
  			res.end(页面);
  			break;
  		case "/login":
  			req.on("data", (chunk) => {
  				解析;
  			});
  			req.on("end", () => {
  				res.end("数据");
  			});
  			break;
  	}
  });
  server.listen(30, () => {
  	console.log("服务已启动");
  });
  
  // express
  const express = require("express");
  const app = express();
  app.use(express.json());
  app.use(express.urlencoded({ extended: false }));
  app.get("/", (req, res) => {
  	res.send(页面);
  });
  app.post("/login", (req, res) => {
  	res.send("收到数据" + req.body + "对象");
  });
  
  // http模块也可以使用express创建的应用对象
  const server = http.createServer(app)
  ```

### express==路由==

- 替代了 http 模块开启的服务里，对`pathname`进行的复杂分类跳转。例：`app.get('/home',(req,res)=>{...})`，表示：接收请求类型为 get、且`pathname`为/home 的请求
- 格式：`app.<method>(path,callback)`

  - `<method>`
    - `post`、`get`：表示匹配 post、get 请求
    - `all`：表示匹配==任意==请求类型
  - `path`

    - 形如`'/home'`形式
    - `'*'`：表示匹配==任意路径==

  - `callback`
    - 回调函数，有两个参数`(request,result)=>{...}`，`request`表示请求报文对象，`result`表示响应报文对象

- Tips
  - 一般登陆首页是没写路径的，因此服务端收到的路径是`/`，所以要监听`app.get('/',callback)`，跳转到主页
  - 匹配是按照代码中从上至下的书写顺序，如`app.all('*',callback)`写在前，那之后所有的匹配规则都不会触发，全被`all('*')`接收，所以要注意匹配的优先顺序

### 获取==请求报文==参数

- 兼容==http 模块==的属性方法
- express 操作
  - `req.path`：等同于`new URL`里的`pathname`属性，会省略==query 参数==，只返回路径
  - `req.query`：返回对象。以键值对的形式保存 query 参数
  - `req.ip`：获取 ip
  - `req.get('key')`：获取==请求头==。
    - 只能通过 get 获取，因为请求头存在==Symbol==值为键名的对象里

### 获取==路由==参数

- 在==路径==中使用占位符来**匹配**获取对应参数值

  - `:命名`：占位符格式

  - 除了占位符以外的其他字符会做==格式==匹配，不匹配的字符串集合到最后一个占位符

    ```js
    页面请求: //www.jd.com/abc.7.8.html
    
    http: 匹配示例;
    app.get("/:a.html", callback); //得到{a:'abc.7.8'}
    app.get("/:a.:b"); //得到{a:'abc', b:'7.8.html'}
    app.get("/:a.:b.html"); //得到{a:'abc', b:'7.8'}
    ```

- `request.params`属性集合了所有占位符存储的==路由参数==，没有占位符，`params`返回值为空

### ==响应==设置

- 兼容 http 模块方法
- express 响应设置可以==链式==调用。例：`res.status(500).set('aaa','123').send(xxx)`
- `res.status(500)`：设置==状态码==。等同于 http 模块的`res.status = 500`
- `res.set('aaa','123')`：设置==响应头==。等于 http 模块`res.setHeader`
- `res.send('中文xxx')`：设置==响应体==。等同于 http 模块`res.end`，会==覆盖页面==
  - **但是**send 方法会自动添加`text/html;charset=utf-8`响应头，因此不需要设置响应头也可正常显示中文
  - 同`res.end`一样，只能有一个
  - `send`后不能写`res.download`等方法
  - 同 http 模块的`res.end`，会继续执行后续代码
- `res.redirect(url,状态码)`：==重定向==
  - 将==**当前**请求==转向新的 url，并不会修改当前页面
  - `url`可以是`./path`、`/path`，也可以是完整的 url`http...`
  - **只能**有一个`redirect`，同`res.end`一样
  - 会继续执行后续代码
- `res.download(path,重命名,失败回调)`：==下载==响应。后面两个参数可以省略
  - `path`只能是本地/服务器资源路径，==不能是 url==！
  - 和`res.send`放在一起执行会==失效==，因为`res.download`执行完会断开请求连接
  - 只有新开页签发送的==get 请求才会触发下载==，如果是页面内元素、事件触发则不会自动下载
    - 方式1：前端页面header中携带密匙，后端返回一个新的url，前端接收到后在新页签中打开访问，后端对访问该url的请求使用`res.download`响应下载
    - 方式2：前端页面在新页签中将token放在地址栏发送请求，后端对该请求使用`res.download`响应下载
- ※`res.json(任何类型数据)`
  - ==接口==响应，返回 JSON 数据，不覆盖原页面，由==发起请求==的回调接收响应结果
  - 同样执行完会断开连接
  - 会自动设置==响应头==类型为`application/json`
  - 如果传读取的文件内容，依然是 Buffer 类型数据，即使`toString`转换成字符串，也会有`\n`等字符，因此不适合传文件
  - 会继续执行后续代码
- `res.sendFile(绝对路径)`：响应==文件==内容
  - 只能是**绝对**路径！因此需要用[path.resolve]()或者`__dirname + '/path'`
  - 响应的文件内容是==原格式==，如果在地址栏发送请求，响应 HTML 文件会==直接运行==！
  - 响应的文件视浏览器是否支持在线预览，不同的浏览器可能会直接打开或者跳转下载
    - 因为浏览器默认自动解析添加`Content-Disposition:video/mp4`等响应头，识别不了的会添加`Content-Disposition:attachment`，从而进行下载
    - **但**如果手动添加响应头`res.set('Content-Disposition','attachment')`，则**所有**响应文件均会**下载**
    - 哪怕是`res.send('字符串')`也会当做文件下载
- Tips
  - 当访问`http(s)://ip:port`时
    - 一种是可以`res.redirect('./index.html')`==重定向==，让`app.use(express.static(__diranme))`静态资源获取服务器上的首页
    - 另一种是`res.sendFile(__dirname + '/index.html')`直接==返回==首页==文件==
    - 如果首页文件名为`index.html`，则可以省略上述方法，因为没有 path 路径时默认访问`http(s)://ip:port/index.html`，静态资源中间件会直接返回文件

### ==中间件==函数

```js
function middleware(req,res,next){
    接收三个参数，分别是 请求报文对象、响应报文对象、next函数
    next函数执行后会运行后续的 路由回调函数 或 中间件回调函数
}
```

- 是个回调函数，用于过滤、预处理请求数据

- ==全局==中间件：在所有路由之前执行

  - **必须**要将`app.use`方法写在所有路由规则前，写在`app.<method>('path')`后会先执行路由匹配。由此可见执行顺序是从上到下，谁写在前面先执行谁。
  - **并且**将`app.use`写到路由后也不会执行！除非`app.use`前的路由一个都没匹配上才会执行==中间件函数==
  - `app.use(中间件函数)`：专门解析中间件函数的方法
    - `use`方法设置的是==**全局**==中间件
    - 会将 http 请求交给中间件
    - 可以传两个参数`app.use('前缀', 路由对象)`，第一个参数是路由匹配规则前缀，第二个是路由对象

- ==路由==中间件：在路由匹配规则触发后执行，并先于`callback`回调前执行

  - 使用方法`app.<method>(path, 中间件函数, callback)`
  - 同样会传入上述三个参数

- ==静态资源==中间件

  ```js
  http模块读取静态资源文件
  const http = require('http')
  const fs = require('fs')
  const server = http.createServer((req,res)=>{
      let {pathname} = new URL(`http://127.0.0.1${req.url}`)
      if(pathname === '/xxx'){
          ...
      } else if(pathname === 'path'){
  
      } else {
          需要一大串ifelse解析路由和静态资源读取
          fs.readFile(`.${pathname}`,(err,data)=>{
              if(err){
                  res.end()
                  return
              }
              res.end(data)
          })
      }
  })
  
  express框架读取静态资源
  const express = require('express')
  const app = express()
  app.use(express.static(__dirname + '/存放静态资源的文件夹路径')) //如 __dirname+'/img'
  app.<method>('path',callback)
  ```

  - 不过一般而言 HTML 页面上的静态资源都是相对路径，以当前 html 文件所在文件夹为==**dirname==，路径一般为`./img/xxx.png`等，因此请求 url 为`http://127.0.0.1:port/img/xxx.png`，这种情况下静态资源路径应写为`express.static(**dirname)`即可，因为静态资源路径写为`express.static(\_\_dirname+'/img')`，请求url必须为`http://127.0.0.1:port/xxx.png`，不能加`/img`，相当于[express.static]()方法已经帮你把存放静态资源的文件夹路径拼上了
  - [use]()方法都是==全局中间件==，因此要写在路由前
  - `express.static`方法会自动为静态资源加上对应响应头类型和字符集
  - Tips
    - ==小技巧==：当处理类似`https://www.baidu.com`这种无路径、端口的请求时，可以用`res.redirect('./index.html')`重定向到导航页的静态文件，就会触发`express.static`方法

### 获取==请求体==数据

- `express`自身也有解析请求体的方法

  - `app.use(express.json()/urlencoded()/text()/raw())`：同`body-parser`一样有四钟方法解析请求体
  - `app.use(express.urlencoded({ extended:false }))`：必写方法。同`body-parser`一样必须要有这个配置项才能解析请求体

- [body-parser]()将请求体数据解析成==对象==形式

  - `body-parser`**只有**`JSON(application/json)`、`表单(application/x-www-form)`、`字符串(text/plain)`、`二进制数据(application/octet-stream)`准备了解析模块。分别为`json()`、`urlencoded()`、`text()`、`raw()`，对==文件==类型的数据没有方法处理

  ```js
  const parser = require('body-parser')
  // 解析 JSON 格式请求体的中间件
  const jsonp = parser.json()
  // 解析 查询字符串 格式请求体的中间件 如 name=123&password=111
  const queryp = parser.urlencoder({extended:false}) // 固定写法
  // 建议请求体中间件用作路由中间件
  app.<method>('path', queryp或jsonp, (req,res)=>{
      // 因为是同一个请求对象 请求体中间件执行完后会往 req对象 身上添加 body 属性
      res.send(req.body) // {name:123, password:111}
  })
  ```

- `body-parser`不能处理文件的==二进制文件流==，因此需要引入[formidable]()工具

  - `formidable`函数中`uploadDir`配置项不能写**不存在**的文件夹！只能选已有的文件夹下保存
  - 形参`files`是对象，存储着文件的详细信息，因为上传的文件会重新命名，所以有`newFilename`和`oldFilename`属性

  ```js
  const formidable = require("formidable");
  app.post("/test", (req, res) => {
  	// 创建formidable对象
  	let form = formidable({
  		mutiples: true,
  		uploadDir: __dirname + "/src", //配置项 上传文件保存目录
  		keepExtensions: true, //配置项 是否保留文件后缀
  	});
  	form.parse(req, (err, fields, files) => {
  		// fields对象内存储着 除了文件之外 字段
  		// files对象内存储着 文件数据 没传文件则为 空对象
  		if (err) {
  			next(err);
  			return;
  		}
  	});
  });
  ```

### 防盗链

- ==请求头==中的`referer`字段会显示==发送请求的网站==，可以根据这个字段决定是否返回内容，这即是防盗链

- **注意**==第一次==访问页面**没有**`referer`字段，之后页面中的静态资源以及触发的事件请求才有

  ```js
  app.use((req, res, next) => {
  	let url = req.get("referer");
  	if (url) {
  		// 第一种方法 正则
  		let reg = /^https?:\/\/(?<a>[0-9\.a-zA-Z]+)(:[0-9]+)?\//g;
  		req.cus_params = reg.exec(url).groups.a;
  		// 第二种方法 URL API
  		let obj = new URL(url);
  		req.cus_params = obj.hostname;
  		if (req.cus_params !== "127.0.0.1") {
  			res.status(404).send("<h1>404 NOT FOUND</h1>");
  			return;
  		}
  	}
  	next();
  });
  app.use(express.static(__dirname));
  app.post("/login", (req, res) => {
  	res.send(req.cus_params);
  });
  ```

### 路由模块化

- 使用前需创建==路由对象==，`const router = express.Router()`
- ==路由==对象跟==应用==对象`app`一样，使用同样的方法，**只不过**需要模块化暴露出去，并在其他地方==导入==`app.use(router)`使用

- `app.use('/user',router)`use 可以设置==路由前缀==，即引用的路由都会加上`/user`等前缀，在路由文件的匹配规则中就不用写`/user`了

- 示例

  ```js
  模块1.js
  const express = require('express')
  const router = express.Router()
  router.get('/home',(req,res)=>{...}) // 因为有前缀 相当于匹配get('/excel/home',callback)
  module.exports = router
  
  模块2.js
  const express = require('express')
  const router = express.Router()
  // 不同模块间也可以互相导入
  const router2 = require('./模块1.js')
  router.use('/excel', router2) // 加前缀
  router.post('/login',(req,res)=>{...})
  module.exports = router
  ```

### express 中使用 ejs 模板

- 使用 ejs 模块渲染 html 结构有两种方式

  - 读取`.html`文件生成字符串，放入 ejs 函数渲染

    ```js
    const express = require("express");
    const app = express();
    const ejs = require("ejs");
    let content = "内容";
    let title = "标题";
    const fs = require("fs");
    let fileString = fs.readFileSync("./index.html").toString();
    let result = ejs(fileString, { title, content });
    app.get("/", (req, res) => {
    	res.send(result); // 注意 此处用的是express的send
    });
    app.listen("80", () => {});
    ```

  - 在`.ejs`文件内写 html 结构，并用 ejs 在响应设置添加的方法渲染

    ```js
    const express = require("express");
    const app = express();
    const ejs = require("ejs");
    app.set("view engine", "ejs");
    app.set("views", __dirname);
    app.get("/", (req, res) => {
    	let text = "hahah";
    	res.render("ejsPageName", { title: text }); //注意,此处使用render,且不需要写文件后缀
    });
    ```

- `app.set(key,value)`：express==应用对象==的 set 方法，用于设置==系统配置==

  - `app.set('view engine', 'ejs')`，`view engine`和`views`是固定写法
  - `app.set('views', path模块.resolve(__dirname, '/HTML模板存储文件夹'))`：指定`res.render()`渲染页面文件存放位置

- 用例

  - js 页面

    ```js
    const express = require("express");
    const app = express();
    app.set("view engine", "ejs");
    app.set("views", `${__dirname}`);
    app.get("/", (req, res) => {
    	let text = "hahahah";
    	res.render("ejsPageName", { title: text });
    });
    ```

  - ejsPageName.ejs 页面

    ```ejs
    <!-- html结构 -->
    <h2><%= title %></h2>
    ```

- Tips

  - 模板中写的变量==必须==传值进去

  - `.ejs`需要换行的地方==必须用==`<% %>`==包裹==

    ```ejs
    <% array.forEach(item => { %> <!-- 注意此处只有半个括号 -->
    	<li><%= item %></li>
    <% }) %>
    ```

  - 模板中可以写 js 语句，如：`<%= title || 'title为false' %>`

## Mongodb

- 三个重要概念
  - ==数据库(database)==：数据仓库，数据库==服务==下可以创建多个数据库，用以存放多个==集合==
    - 一个项目使用一个数据库
    - 一台电脑安装一个 mongodb，这台电脑就是一个==节点==
  - ==集合(collection)==：类似==数组==，集合中存放多个==文档==
    - 一个集合存储==同一类型==的数据
  - ==文档(document)==：文档是数据库中最小单位，类似==对象==。文档中的属性称之为==字段==
- 安装数据库注意事项
  - 通过安装程序安装的`mongodb`不需要命令行启动服务，而是在window系统下的`服务`功能中启动
  - 以下展示`.zip`压缩包形式启动服务
    - 在安装或解压目录下的`bin`目录打开 cmd，执行`mongod --dbpath=..\data\test`指定数据库路径为`..\data\test`(以 bin 文件目录为起点同级的 data 下的 test)目录，并启动==mongodb 服务==，注意不是==HTTP 服务==，在浏览器端无法直接访问，因为是 HTTP==协议==向 mongodb 服务发起请求
      - 为了能连接 Mongodb==服务==，还需要启动 Mongodb==客户端==，以客户端连接服务
        - 客户端启动命令`mongo`，**注意**不是`mongod`，有两个`.exe`文件

      - ==服务==开启后**不能**关闭，否则==客户端==访问不到
      - Mongodb 默认存放数据位置`C:\data\db`，如果用默认目录可以执行`mongod`直接启动服务，但使用自定义目录，就需要每次执行`mongod --dbpath=..\data\test`来启动服务

    - 命令行`mongod`指令其实是执行`bin`目录下的`mongod.exe`，可能会提示缺少`xxx.dll`文件，下载后放在`c:\Windows\System32`下即可
    - 配置环境变量，便于命令启动
      - 复制 mongodb 的 bin 文件夹目录，在系统——高级系统设置——环境变量——Path 中新建粘贴文件夹目录
      - 以后启动客户端就可以直接`mongo`，无需在`bin`目录下
        - 但是因为是自定义数据存放目录，因此启动服务还需要在`bin`目录下执行

    - **不要**在服务窗口选中文本，会导致服务暂停，客户端操作无法返回结果
      - 解决方法：服务端窗口按下回车

- 命令行交互

  - 数据库命令

    - 显示==所有==数据库：`show dbs`
    - 切换到指定数据库：`use 数据库名`
      - 如果不存在，则自动创建
    - 显示当前所在的数据库：`db`
    - 删除当前数据库：`use 库名`——`db.dropDatabase()`
      - 删除当前库后，再使用`db`查看当前库，会发现还是当前库，并且依然可以通过创建集合等方法填入内容，再次`show dbs`就又可以看到被删的库

  - 集合命令

    - 创建集合：`db.createCollection('集合名')`
    - 显示==当前==数据库中所有集合：`show collections`
    - 删除==某个==集合：`db.集合名.drop()`
    - 重命名集合：`db.集合名.renameCollection('newName')`

  - 文档命令

    - 插入文档：`db.集合名.insert(文档对象)`
    - 查询文档：`db.集合名.find(查询条件)`
      - `_id`是 mongodb 自动生成的唯一编号，用来唯一标识文档
      - 查询条件可以是文档中某一字段，但**必须**是对象的形式，如`db.test1.find({age:20})`
        - 字段必须完全匹配，不支持模糊搜索
        - 可以多字段同时查询
    - 更新文档：

      - 用新内容全部替换：`db.集合名.update(查询条件， 新的文档)`
      - 修改对应字段：`db.集合名.update({name:'xxx'}, {$set:{age:19}})`
      - 当有多个查询结果符合条件时，修改更新失败

    - 删除文档：`db.集合名.remove(查询条件)`
      - 当有多个查询结果符合条件，一起删除

- Tips

  - 与`MySQL`的区别
    - `Mongodb`属于==非关系型==数据库，`MySQL`属于==关系型==数据库
      - ==关系型==数据库特点是以**行**和**列**的方式存储数据，类似 Excel，一系列行和列组成**表**，一组**表**组成**数据库**，每一列的==字段名==都是==固定==的
      - ==非关系型==数据库的特点是类 JSON，没有行和列，以文档为最小单元，每个文档中==字段名==可以==不固定==
      - ==关系型==数据库中**表**对应==非关系型==数据库中**集合**，**一组**表或集合组成一个数据库
    - `MySQL`必须用==SQL 语句==操作，而`Mongodb`不只用 SQL 语句，还可以用其他语言操作
    - `MySQL`适合**读写**操作==密集==的场景，`Mongodb`适合灵活更改==数据类型==场景

## Mongoose

- 是一个工具包使用`npm i mongoose`安装，作用是便于用==代码操作==Mongodb 数据库

  - 绕过`mongo`客户端命令行操作数据库，而是通过`mongoose`直连`mongodb`服务用代码进行操作

- 导入 mongoose：`const mongoose = require('mongoose')`

- 连接 mongodb 服务：`mongoose.connect('协议名://IP地址:端口号/数据库名')`

  - 协议名`mongodb`
  - 端口号默认`27017`
  - 数据库名不存在会自动创建
  - **注意**本地连接时，IP 地址不能用`localhost`只能用`127.0.0.1`

- 设置事件回调

  - 连接成功回调：`mongoose.connection.on('open',()=>{})`
    - 连接成功会执行一系列内置操作，使用`on`监听会导致掉线重连时，重复触发回调及内置操作，官方推荐使用`mongoose.connection.once('open',callback)`，只会触发一次
  - 连接失败回调：`mongoose.connection.on('error',()=>{})`
    - 连接失败有一个==超时==的过程，不会立即返回结果
  - 连接关闭回调：`mongoose.connection.on('close',()=>{})`
    - 关闭连接：`mongoose.disconnect()`

- 连接数据库后在集合中==添加==文档

  ```js
  const mongoose = require("mongoose");

  // 连接Mongodb服务中指定数据库
  mongoose.connect("mongodb://127.0.0.1:27017/customdb");

  mongoose.connection.once("open", () => {
  	// 首先创建文档结构对象 其实就是定义数据类型
  	let dataType = new mongoose.Schema({
  		name: String,
  		price: String,
  		num: Number,
  	});

  	// 然后创建对象模型 其实就是所连接数据库下指定要操作哪个集合，并限制文档字段数据类型
  	// ※没有对应集合则会新建
  	let obj = mongoose.model("集合名", dataType);

  	// ※注意Mongoose@6.0以后的版本不支持create中传入回调函数 而是返回一个Promise对象
  	obj
  		.create({
  			name: "菠萝",
  			price: "30",
  			num: 2,
  		})
  		.then(
  			(data) => {
  				console.log("创建成功" + data);
  			},
  			(err) => {
  				console.log("创建失败" + err);
  			}
  		);
  });
  mongoose.connection.on("error", () => {
  	console.log("连接失败");
  });
  mongoose.connection.on("close", () => {
  	console.log("连接关闭");
  });
  ```

- ==删除==文档

  ```js
  mongoose.connection.once("open", () => {
  	let dataType = new mongoose.Schema({
  		name: String,
  		price: String,
  		num: Number,
  	});
  	let obj = mongoose.model("集合名", dataType);
  	// ※注意Mongoose@6.0以后的版本不支持delete中传入回调函数 而是返回一个Promise对象
  	// 删除一条 可以有多个符合查询条件的 但只会删除第一条匹配到的
  	obj.deleteOne({ name: "菠萝" }).then(
  		(data) => {
  			console.log("删除成功" + data);
  		},
  		(err) => {
  			console.log("删除失败" + err);
  		}
  	);
  	// 删除多条
  	obj.deleteMany({ price: "30" }).then(
  		(data) => {
  			console.log("删除成功" + data);
  		},
  		(err) => {
  			console.log("删除失败" + err);
  		}
  	);
  });
  ```

- ==更新==文档

  ```js
  mongoose.connection.once('open',()=>{
      let dataType = new mongoose.Schema({
          name: String,
  		price: String,
  		num: Number,
      })
      let obj = mongoose.model('集合名', dataType)
      // ※注意Mongoose@6.0以后的版本不支持update中传入回调函数 而是返回一个Promise对象
      // 更新一条 传入两个参数 第一个参数是查询条件 第二个参数是更新内容
      obj.updateOne({name:'菠萝'},{price:'10'}).then( data =>{
          console.log('更新成功' + data)
      }, err =>{
          console.log('更新失败' + err)
      })
      // 更新多条
      obj.updateMany({price:'10'},{name:'荔枝'})then( data =>{
          console.log('更新成功' + data)
      }, err =>{
          console.log('更新失败' + err)
      })
  })
  ```

- ==读取==文档

  - 注意：读取结果都是==数组==

  ```js
  mg.connection.once("open", () => {
  	let datatype = new mg.Schema({
  		name: String,
  		price: String,
  		num: Number,
  	});
  	let obj = mg.model("test2", datatype);
  	// ※读取不到也不会读取失败 只会返回空数组[] 旧版本返回null
  	// 读取单条
  	obj.findOne({ name: "茄子2" }).then(
  		(data) => {
  			mg.disconnect();
  			console.log("读取成功", data);
  		},
  		(err) => {
  			mg.disconnect();
  			console.log("读取失败");
  		}
  	);
  	// 通过id读取
  	obj.findById("646dddcc94cf30cef7076975").then((data) => {
  		mg.disconnect();
  		console.log("读取成功" + data);
  	});
  	// 批量查询
  	obj.find({ name: "test2" }).then((data) => {
  		mg.disconnect();
  		console.log("读取成功" + data);
  	});
  	// 读取所有
  	obj.find().then((data) => {
  		mg.disconnect();
  		console.log("读取成功" + data);
  	});
  	// 读取筛选 读取到符合条件的记录只显示需要的字段内容
  	// select 接收对象作为参数 对象内要读取的字段值为1
  	// 省略或值设置为0 表示不显示
  	obj
  		.find()
  		.select({ price: 1, name: 1 })
  		.then((data) => {
  			console.log("旧版本需要exec执行回调，将find里的回调方法放到exec中执行，新版返回的是Promise对象，因此不需要回调");
  		});
  	// 读取数据排序
  	// select等方法可以链式调用
  	obj
  		.find()
  		.select({ price: 1 })
  		.sort({ price: 1 })
  		.then((data) => {
  			console.log("升序1 降序-1");
  		});
  	// 数据截取
  	// skip跳过num条 limit取前num条 数据
  	obj
  		.find()
  		.sort({ price: -1 })
  		.skip(3)
  		.limit(3)
  		.then((data) => {
  			console.log("price从降序排列，取4~6位置(skip跳过3条，limit只取前3条)的记录");
  			console.log("常用于分页");
  		});
  });
  ```

- ==条件==控制

  - 运算符：`mongodb`不能使用`<>=!==`等运算符，而是用替代符号
    - `>`用`$gt`
    - `<`用`$lt`
    - `>=`用`$gte`
    - `<=`用`$lte`
    - `!==`用`$ne`
    - 使用示例：`db.test.find({ price: { $gt: 10 } })`，表示价格比 10 大的记录
  - 逻辑运算
    - 逻辑==或==：`$or`
      - 示例：`db.test.find({ $or:[{price:10}, {price:30}] })`，价格 10 或 30 的
    - 逻辑==与==：`$and`
      - 示例：`db.test.find({ $and:[{price:{$gt:10}, {price:{$lt:30}}] })`，价格==大于==10==小于==30 的
  - 正则匹配
    - 示例：`db.test.find({name:/[A-Z0-9]+/})`

- ==文档==结构可选字段类型

  - 字符串：`String`

  - 数字：`Number`

  - 布尔值：`Boolean`

  - 数组：`Array`，也可以用`[]`

  - 日期：`Date`

  - Buffer 对象：`Buffer`

  - 任意类型：`mongoose.Schema.Types.Mixed`

  - 对象 ID：`mongoose.Schema.Types.ObjectId`

    - 主要用于==关联表==，也就是==外键==，通过文档中这个字段来==查找其他表==，以联合搜索内容

  - 高精度数字：`mongoose.Schema.Types.Decimal128`

  - Tips
    - 写入文档的字段名和文档结构中==不符==，则会==忽视==
    - 写入文档==字段类型==与文档结构==不符==，则会==报错==

- ==字段值==验证，mongoose 内置功能

  ```json
  name:{
      type: String, // 指定类型
      required: true, // 设置为必填
  },
  price:{
      type: String,
      default: '10', // 设置默认值 此处default值类型和type不一致会被忽略
  },
  options:{
      type: String,
      // 设置的值 必须 是数组中的 比如options:'test2'
      // enum中值类型和type 必须 一致
      enum: ['test1','test2']
  },
  name:{
      type: String,
      // 设置为独一无二 重复会报错
      // 注意 要使用此项必须一开始就用 不能在旧集合中使用此项 会失效
      unique: true,
  }
  ```

- ==条件==控制

  - mongodb 不能使用`<>=!=`等运算符，需要使用符号代替
  - 运算符：例`{price:{$gt:10}}`price 大于 10 的记录
    - `>`：`$gt`
    - `<`：`$lt`
    - `>=`：`$gte`
    - `<=`：`$lte`
    - `!==`：`$ne`
  - 逻辑运算
    - `$or`逻辑或：`{$or:[{price:{$lt:10}}, {price:{$gt:30}}]}`price 小于 10**或大**于 30 的记录
    - `$and`逻辑与：`{$and:[{price:{$gt:10}}, {price:{$lt:30}}]}`price 大于 10**且**小于 30 的记录
  - 正则匹配：`{price:/[1-3]{1}[0-9]{1}/}`

- Tips
  - mongoose 除了`_id`字段会自动往文档里添加`id`字段

## 接口

- [RESTful API]()

  - 一种特殊风格的接口

  - URL 的路径表示==资源==，路径中不能有==动词==，例如`create`，`delete`，`update`等这些都不能有

  - 操作资源要与`HTTP 请求方法`对应

  - 操作结果要与`HTTP 响应状态码`对应

  - 请求类型

    - 其实是语义化的规范，里面逻辑还是自己写

    | 请求类型 | 返回                                       |
    | -------- | ------------------------------------------ |
    | GET      | 资源信息                                   |
    | POST     | 新增并返回新的资源信息                     |
    | PUT      | 更新并返回新的资源信息(完整更新替换)       |
    | DELETE   | 删除资源返回空文档                         |
    | PATCH    | 更新并返回新的资源信息(局部更新同名属性值) |

- [json-server]()工具包

  - 用于快速搭建`RESTful API`服务
  - 安装：`npm i -g json-server`

- 接口测试工具[apipost](https://www.apipost.cn/)

  - `none`表示没有请求体
  - `form-data`表示表单形式的数据
  - `x-www-form-urlencoded`表示`query`查询字符串
  - `raw`表示`json`等格式的原生请求体
  - 放入同一文件夹的接口可以设置通用`header`等

## 日期对象

- 存到数据库里的==时间==得是==日期对象==，因为后端内部调用接口时处理字符串形式的日期很麻烦，因此存成一个对象使用更方便
- 有两种方式
  - 一种`new Date()`传入日期时间字符串
  - 另一种使用[moment](http://momentjs.cn/)工具包转换成`Date`对象
    - `moment('2023-10-1').toDate()`

## 权限控制(用户身份识别)

- HTTP 协议无法区分请求来自哪里，即==无法区分用户==

- 常见的权限(会话)控制技术

  - cookie

    - HTTP 服务器发送到==浏览器保存==的一小块数据
    - 按==域名==划分保存
    - `key=value;`形式的键值对
    - 特性

      - 浏览器向服务器发送请求时，会==自动==将==当前域名下==可用的 cookie 设置在==请求头==中，发送给==服务器==，再由服务器发送`set-cookie: name=xxx;password=111`响应头，浏览器识别到这一响应头会自动将 cookie 存储到当前域名下
      - 没有 cookie 不会有`cookie: name=xxx`请求头
      - 当前域名指同一==IP==、==端口号==，都会携带 cookie
      - 对于没有设置生命时长的 cookie，==浏览器==关闭时会自动清除；设置了时长的，时间一到也会自动清除
      - 注意：==响应报文==中的`Max-Age=120`单位是==秒==

    - 应用

      ```js
      // express文件
      app.get("/login", (req, res) => {
      	// 设置cookie
      	res.cookie("name", "xxx");
      
      	// 设置生命时长 单位毫秒
      	res.cookie("name", "xxx", { maxAge: 2 * 60 * 1000 }); // 2分钟
      
      	// 清除cookie
      	// 只能一条条删
      	res.clearCookie("name");
      });
      // 读取cookie
      // 需要安装cookie-parser工具包
      let cookieparser = require("cookie-parser");
      app.use(cookieparser());
      app.get("/read", (req, res) => {
      	res.json(req.cookies);
      });
      ```

  - session

    - 保存在==服务器端==的数据

    - 运行流程

      1. 填写身份验证信息，校验通过后创建==session 对象==，将`session_id`的值通过`set-cookie`返回给浏览器
      2. 浏览器下次请求就会携带`cookie`，服务器通过`cookie`中的`session_id`确定身份

    - 应用

      ```js
      // 需要安装express-session、connect-mongo工具包
      const session = require("express-session");
      const cm = require("connect-mongo");
      
      const app = express();
      // 设置session中间件 传入配置对象 返回一个函数
      app.use(
      	session({
      		name: "sid", //设置cookie的name，默认值：connect.sid
      		secret: "key", //参与加密的字符串(签名/密钥)
      		saveUninitialized: false, //是否每次请求设置一个cookie存储session的id，为true则不用session也会创建空对象
      		resave: true, //是否每次请求重新保存session(延续生命周期)，如20分钟过期，只要操作间隔不大于20分钟就一直不会过期
      		store: cm.create({
      			mongoUrl: "mongodb://127.0.0.1:27017/customdb", //数据库连接配置
      		}),
      		cookie: {
      			httpOnly: true, //前端是否可通过JS操作cookie
      			maxAge: 60 * 1000, //控制 sessionID 的过期时间(包括浏览器cookie、数据库session过期时间)
      		},
      	})
      );
      // 路由
      app.get("/login", (req, res) => {
      	if (req.query.name === "admin" && req.query.password === "admin") {
      		// 设置 session 信息
      		req.session.username = "admin";
      		req.session.password = "admin";
      
      		res.send("登陆成功");
      	} else {
      		res.send("登陆失败");
      	}
      });
      // 读取 session
      app.get("/home", (req, res) => {
      	// 中间件已经根据浏览器请求中携带的cookie获取session id
      	// 并在数据库中查询将查询结果放入req对象中
      	if (req.session.username) {
      		res.send(`欢迎 ${req.session.username} 登录`);
      	} else {
      		res.send("你还没有登录");
      	}
      });
      // 销毁 session
      app.get("/loginout", (req, res) => {
      	req.session.destroy(() => {
      		res.send("退出登录");
      	});
      });
      ```

  - token

    - 由==服务端==返回给客户端的加密字符串，保存着==用户信息==

    - 与`cookie`不同的是，`token`是由客户端发送请求时手动添加到请求报文中，一般放在请求头

    - 特点

      - 数据存储在客户端，服务端压力更小
      - 更安全，数据加密避免伪造请求攻击
        - 即使`token`泄露，但==服务端==掌握的==加密字符串(钥匙)==不会泄露，就无法解析`token`获得加密信息
      - 拓展性强，可服务间共享、增加服务节点更简单

    - Tips

      - 记录`token`中携带的用户信息，便于取用
    
        ```js
        let checklogin = (req, res, next)=>{
            let token = req.get("token");
        	if (token && token !== "undefined") {
        		// 不要把空token传入校验会报错
        		jwt.verify(token, "myKey", (err, data) => {
        			if (err) {
        				console.log(err);
        				return res.json(json(null, "登陆已过期！"));
        			}
                    req.userInfo = data;//将用户信息保存起来
        			next();
        		});
        	} else {
        		res.json(json(null, "登陆已过期！"));
        	}
        }
        app.use(checklogin)
        // 路由规则
    
    - 应用
    
      ```js
      // 需要安装 jsonwebtoken 工具包
      const jwt = require("jsonwebtoken");
      
      // 创建 token
      // jwt.sign(数据, 加密字符串, 配置项)
      let token = jwt.sign(
      	{
      		username: "xxx",
      	},
      	"secretkey",
      	{
      		expiresIn: 60, //单位 秒
      	}
      );
      
      // 解析 token
      jwt.verify(token, "secretkey", (err, data) => {
      	if (err) {
      		// 使用加密字符串解析token不正确时触发
      		console.log(err);
      		return;
      	}
      	//解析得到得数据 {username:'xxx', iat: 注册时间戳, exp: 过期时间戳}
      	console.log(data);
      });
      ```

- `session`和`cookie`的区别

  - 存储位置
    - `cookie`：浏览器端
    - `session`：服务器端
  - 安全性
    - `cookie`是明文，安全性较低
    - `session`数据存于服务器相对安全，即时暴露也只有 cookie 中的`session_id`
  - 网络传输量
    - `cookie`设置内容过多会影响传输效率
    - `session`存在服务器，只通过`cookie`传`id`，不影响效率
  - 存储限制
    - 浏览器限制单个 cookie 保存数据不能超过==4K==，且==同域名==下存储条数也有限制

## 服务器部署运行(项目上线)

- 环境配置

  - 首先在服务器中安装`nodeJS`、`Mongodb`、`git`环境
  - 然后`npm i`安装`package.json`下的依赖包
  - 最后在命令行使用`node ./path/server.js`启动服务

- 在==云服务器本地==访问服务使用`127.0.0.1:port/page`访问页面，而==外网==使用`云服务器公网IP:port/page`访问页面

- 域名注册(以阿里云为例)

  - 选`产品`中的`域名注册`，搜索像注册使用的域名看是否可以购买
  - 进入`域名`-`控制台`，如果是国内使用，需要在`ICP备案`按流程登记等待审核
  - ==审核过==的域名点击`解析`，将域名和==云服务器公网IP==做一个映射
    - tips：像`www.baidu.com`这类都是域名，不是IP地址，浏览器其实不知道把请求发给谁，所以会去`DNS服务器`去查询当前域名对应的`IP地址`
  - 点击`添加记录`，`记录类型`选择`指向一个IPV4地址`，`主机记录`设置`www`等前缀，`记录值`填写`服务器公网IP`

- 配置HTTPS证书

  - 只能在==服务器==端操作，本地服务无法操作

  - https可以==加密HTTP报文==，响应报文由客户端解码，请求报文由服务器解码

  - [工具官网](https://certbot.eff.org/)

  - 操作流程

    1. [下载工具](https://dl.eff.org/certbot-beta-installer-win_amd64.exe)安装

    2. 管理员运行命令`certbot certonly --standalone`

       - 证书获取会占用`80`端口，注意不要跟服务器启的服务冲突
       - 获取证书需要输入邮箱、==域名==
         - 域名**必须**是==指向当前服务器IP==的
       - 证书会存储在本地

    3. 导入并使用`https`模块**创建服务**

       ```js
       const https = require('https')
       const fs = require('fs')
       // 导入express
       const express = require('express')
       const app = express()
       // 创建https服务 应用express对象
       https.createServer({
           // 证书存放路径 注意因为是反斜线需要转义\
           key: fs.readFileSync('C:\\Certbot\\live\\www.custom.com\\privkey.pem','utf8'),
           cert: fs.readFileSync('C:\\Certbot\\live\\www.custom.com\\cert.pem','utf8'),
           ca: fs.readFileSync('C:\\Certbot\\live\\www.custom.com\\chain.pem','utf8'),
       },app).listen(443,()=>{
           // https默认端口号443
           console.log('443端口监听中')
       })
       // 设置路由规则
       app.use(中间件)
       const router = require('./api/router.js')
       app.use('/api',router)
       app.get('/', callback)
       ```
    
    4. 证书更新
    
       - 有效期==三个月==
       - 一般更新：`certbot renew`
       - 强制更新：`certbot --force-renewal`
