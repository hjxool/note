UDP是基于非连接，即发送出去后不知道对方有没有收到

TCP是基于连接，即先建立连接再通信最后断开

------

### 三次握手：

客户端发送syn包请求连接，服务端回复syn+ack包表示可以连接，客户端在收到回复后才发送ack包正式建立关系（因为syn包可能会滞留，导致重复建立同样的连接，所以要三次沟通再建立）

### 四次挥手：

客户端发送fin包，请求断开，服务端收到后回复fin+ack包，表示可以，然后把最后的数据发送完后再回一个fin，表示我发送完了可以断开，客户端收到以后回复ack告诉服务端我已经断开了，并进入等待断开的状态，如果服务端收到并断开连接一段时间后，客户端才放心的彻底断开连接

![img](https://upload-images.jianshu.io/upload_images/6322775-739bb1a0c88d5f91.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

适用场景

------

SSE与websocket不同，SSE基于HTTP协议，所以后端无需额外修改，而websocket有独立协议；

### SSE用例：

​    let source = new EventSource('url')；

​    EventSource对象第二个参数 `{withCredentials:true}` 可以应对访问跨域接口时，发送cookie；

​    onopen用于连接建立时触发回调函数

​    onmessage用于客户端收到服务器数据后触发

​    source.close()用于关闭连接

------

### websocket用例：

  let **ws_link** = new WebSocket(ws_url)

  let **message** = { key1:value1，key2：value2 }

  **ws_link.**send(**JSON.stringify**（**message**）)