## 使用方式

### 简化版

- [get请求]()：`axios.get(url).then(res=>{})`
- [post请求]()：`axios.post(url, {传送数据}).then(res=>{})`
- put请求同post

### 完整写法

- [get请求]()：

```js
axios({
	method:'get',
	url:''
}).then(res=>{})
```
- [post及put请求]()：

```js
axios({
	method:'post',
	url:'',
    data:{}
}).then(res=>{})
```

