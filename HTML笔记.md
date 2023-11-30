## html的加载顺序

- 解析html结构 -> 加载外部脚本和样式表文件 -> 解析并执行脚本代码 -> 构造html dom模型 -> 加载图片等外部文件 -> 页面加载完毕。

-  html文件从上至下读取，如果`<head>`中外部引入文件读取慢，则会发生==js阻塞==，只有等前一个文件/服务器读取完才能执行后面的操作

  解决办法：

  - 使用Vue的 v-cloak指令`<div v-cloak>{{msg}}</div>`，这个指令保持在元素上直到关联实例结束编译
  - 使用Vue的v-html指令 `<div v-html='msg'></div>`
  - 使用Vue的 v-text指令 `<div v-text='msg'></div>`
  - 使用`<template>`标签将需要渲染的 内容包起来
    - `<template>`标签是一种用于保存客户端内容的机制，该内容在页面加载时不被渲染，但可以在运行时使用JavaScript进行实例化。

## HTML全局属性(所有标签都可以使用)

- `title`：为元素标签添加鼠标**悬停信息**

- `data-name="value"`：搭配 `getAttribute('name')`方法 来获得标签上的自定义 `data-*` 属性
  - JS中通过 `dataset.后缀名` 设置标签节点上的`data-*`属性

  - 或者通过`dom.setAttribute('data-*'，值)`

  - 或者通过JQuery中 `data('后缀名')`方法设置

- `draggable`：使标签元素可拖动
  - 链接和图像默认是可拖动的

- 其他的还有style和class等

## ul、li标签

- `<ul>`：`<ul>`默认有`padding`和`margin`属性，将这两者设为0即可
  - 列表默认垂直排列，可以在`<ul>`样式设置==flex布局==来横向排列
- `<li>`：前带有小圆点，用`list-style`CSS属性设置清除

## table表格

- `<tr>`表示行
- `<td>`表示一行中的列
  - `<td>`标签属性[colspan]()：合并列。例：`<td colspan="2">`当前列占2格
  - [rowspan]()：合并行。例：`<td rowspan="2">`当前行占2格
  - [vertical-align]()：CSS属性。只有在td中才能居中所有子元素，对于其他标签只会作用于文字

- `<th>`和`<td>`不同的一点是`<th>`带加粗

- 行、列合并

  ![img](https://upload-images.jianshu.io/upload_images/6322775-bf4beb046ff9b398.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## form表单

- `<form method="post/get等" action="接口url">`

- `<form>`标签属性`action="url"`：将包裹范围内带name属性的元素值通过地址栏方式提交
  
  - 提交的数据格式`url?name1=xxx&name2=xxx`
  
- 点击`<input type="submit"`才会提交
  - `<button>`：默认`<button type="submit">`，只要有`type="submit"`就可以提交

- `<input name="mima">`有`name`才会将**值**提交

- `<input>`、`<select>`等表单控件可以使用`<xx disabled>`属性禁止点击交互

- `<lable for="控件id">`是为了改进可用性

  - 点击`<label>`标签文字效果跟点击组件一样
  - `for`的值要跟对应控件id相同

  ```html
  <form>
      <label for="hhh">点我触发对应id控件</label>
      <input type="radio" id="hhh" />
  </form>

## video标签

- `<video src>`标签内的`<source src>`表示多个片段，按先后顺序拼接，如果只有一个`<source>`就可以`<video src="..." type="...">`，有多个片段则

  ```html
  <video autoplay controls>
      <source src="..." type="..."></source>
      <source src="..." type="..."></source>
  	...
  </video>

## input标签

- ==value属性==即使在HTML写死，在`<input/>`==输入==时获取到的也是==当前输入值==，而不是`value`值
- `text-align`：CSS样式。可以调整输入内容位置
  - 文字位置决定了文字输入方向
  - 如`text-align: right;`使光标移动到最右边，内容从后往前延伸
- `readonly`：标签属性。输入框变成只读，可选中但不能输入
- 当`<input type="file"/>`设置样式`display：none`隐藏显示
  - 仍然可以触发`dom.click() `方法，以及`change`事件
  - `<input type="file"/>`元素虽然存储着所选文件，但是==只读==，**只能**通过==清空==`inputDom.value = null或者''`来==重置选择文件==
  - `dom.files`不具有数组常用的那些方法，只能用for循环遍历取出里面的文件对象，添加到自定义数组中，展示和上传文件时使用自定义数组
- 虽然不同`type`时获取`<input/>`节点取的字段不同，但是`<input/>`节点上有所有不同`type`的字段
- `<input type="checkbox/radio">`值存在dom节点的`checked`属性中
- `<input type="text">`值存在dom节点的`value`属性中

## HTML文档结构

- `<!Doctype HTML>`就是==文档声明==，用来告诉浏览器当前网页版本
- `<html lang="zh-CN">`：屏蔽chrome浏览器默认提示翻译弹窗
- `<head>`
  - `<meta>`表示声明==字符集==，只有用正确的字符集去==解码==，才不会出现乱码
  - ==content==属性：设备被搜索时的关键词(百度搜索、爬虫可用)。如`<meta content="购物,前端">`
  - `<head>`引入`<script>`不好用，因为当script引入的js需要操作节点时，`<body>`中的结构还没有读取到
    - 如果需要在`<head>`中使用`<script>`，需要用`window.onload = () => {原script中代码}`

## 超链接

- 使用`id`属性给元素打上标记，再用`<a href="#ID">`就可以==跳转==到对应的==元素位置==
  - 可以用`click()`方法触发超链接的`href`，即使超链接==不显示也可以触发==，可以隐藏超链接，用替换的样式

## 样式标签

- ==上标、下标==

  ![img](https://upload-images.jianshu.io/upload_images/6322775-c743b6a11f347c74.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- `<pre/>`：放入预先格式化的文本。==保留空格==和==换行符==

## 标签命名规则

- HTML**属性名**对大小写不敏感，会统一转换成小写，在js中是驼峰式的写法在HTML中写成短横线分隔

  ![img](https://upload-images.jianshu.io/upload_images/6322775-f07c8f6f5807509c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## script标签

- 浏览器解析html文件时，如果遇到`<script>`，便会停下对html结构的解析，转而处理脚本，脚本执行完才会继续解析html结构
- 属性`defer`(延迟)
  - 浏览器不会优先处理`<script defer>`内的脚本，而是在html结构解析完成后才去执行脚本，且会暂时阻止`DOM事件`的执行，等到加载好的脚本执行完才会执行事件回调
- 属性`async`(异步)
  - 脚本和html结构是并行加载，执行脚本时机不确定，所以可能会获取不到html结构中定义的元素，因为此时元素还没被解析

## iframe标签

- 默认有边框，去除边框`frameborder="0"`
- 默认有滚动条，去除滚动条`scrolling="no"`
