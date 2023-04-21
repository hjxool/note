# HTML-1
1. `<h1></h1>`是标题标签
2. `<p></p>`段落
3. 图片由`<img src="1.jpg">`
![](https://upload-images.jianshu.io/upload_images/6322775-c3f030a6c69f12c0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
4. `<html>`是开始和结束标签，是根标签
5. `<head>` 标签用于定义文档的头部，它是所有头部元素的容器。头部元素有`<title>、<script>、 <style>、<link>、 <meta>`等标签
6. `<body>`内容是网页的主要内容,有`<h1>、<p>、<a>、<img>`等网页内容标签
7. `<title>`标签之间的是网页的标题信息，在浏览器的标题栏中显示
- ![](https://upload-images.jianshu.io/upload_images/6322775-7ae896c295f3e7f9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
8. HTML代码注释`< !--注释文字 -->`
9. `h1,h2`是标题**等级**
10. `<style>`写在`<head>`里面
- 在`<style>`中设置颜色等
- CSS也在`<style>`中设置
11. `<br>`相当于回车
- `&nbsp;`相当于空格
- 都没有**结束标签**
12. `<div>`用来划分独立逻辑
- 为了标识每个独立的逻辑部分，要使用`<div  id="版块名称">…</div>`ID来命名
- `id`为为**唯一标识**，即不能有多个相同的；`class`可以指定**多个类名**
13. 超链接
- `<a  href="目标网址"  title="鼠标滑过显示的文本">链接显示的文本</a>`
- 需要在新窗口打开时:`<a href="目标网址" target="_blank">click here!</a>`
14. 插入图片
- `<img src="图片地址" alt="下载失败时的替换文本" title = "提示文本">`
15. 表单交互
- `<form   method="传送方式"   action="服务器文件">`
16. 文本输入框
- type是password的话是不显示输入文字的
 ```
<form>
   <input type="text/password" name="名称" value="文本" />
</form>
 ```
17. 单选、复选框
- `<input   type="radio/checkbox"   value="值"    name="名称"   checked="checked"/>`
- ![同一组的单选按钮，name 取值要一致;多选name不能一样](https://upload-images.jianshu.io/upload_images/6322775-2db49d43aade2404.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
18. 下拉列表框
- ![selected="selected"表示初始选中](https://upload-images.jianshu.io/upload_images/6322775-553c5e2aa4a42bb3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
- 添加`multiple="multiple"`可以实现多选
19. 提交按钮
- ![只有当type值设置为submit时，按钮才有提交作用](https://upload-images.jianshu.io/upload_images/6322775-e9551a2583223d07.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
20. 重置按钮
- `<input type="reset" value="重置">`
- **type决定功能**
21. form表单中的**label标签**
- `<label for="控件id名称">`
- **注意**：**for 的值**应当与相关**控件的 id 值**一定要**相同**。
- 作用是：点击Label标签文本，就会触发控件
22. CSS改变字体颜色
- <span></span>括起来的字体用span{ }改变颜色
23. CSS语法
- ![](https://upload-images.jianshu.io/upload_images/6322775-343ac24aabf18e55.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
- **选择符：**指明样式的作用对象
- **声明：**“｛｝”中的的就是声明
24. 内联式CSS
- `<p style="color:red;font-size:12px">`**注意：**格式要加引号
25. 嵌入式CSS
- 写在`<style type="text/css"></style>`标签之间（在`<head></head>`里）
26. 外部式CSS
- 以**.css**为扩展名
- 在**<head>内**（不是在<style>标签内）使用<link>标签将css样式文件链接到HTML文件内
- `<link href="文件名" rel="stylesheet" type="text/css" />`
- `rel="stylesheet" type="text/css"` 是固定写法
27. 类选择器
- `.类选器名称{css样式代码;}`
- `<span></span>`本身没有用，但是想要为标签中的字体**单独**设置样式，就需要**“类”**来标记
28. ID选择器（不能有重复ID，不能同时设置多个样式）
- `<span id="名称"></span>`
- `#ID名称{css样式代码;}`
29. 只有子选择器可以加入红线框
30. 伪类选择符
- 鼠标滑过的位置会变色
- `标签名:hover{css样式代码;}`
31. **标签之间用空格连接表示层级关系**
32. **Label**标签
- **for**与表单的**Id**对应，作用是点击label相当于点击表单

# HTML-2

Tips：

- 为什么在`<head>`引入`<script>`不好用，因为当script引入的js需要操作节点时，`<body>`中的结构还没有读取到

- 所有容器的子元素，不设宽高的话宽度都是默认铺满的，只有高度需要自行设置，不是只有flex、grid布局才铺满
- js中**标签属性**是**大小写**的，在**html**页面要改为**短横线**分隔，但是**标签名**没有限制，用什么奇里古怪的命名方式都行
- “<input type='number'>”可以限制输入的值不能是字母或空格，你打字母空格打不上去
- “innerHTML = '字符串' ”，所有内容必须用引号包裹，如果里面是“html标签”，则会识别渲染
- html对“display：none”的元素时**不渲染**的，所以元素相关**属性值**均为**0或null**

---

> **href**是关联当前元素和引用资源之间的联系；
>
> **src**是引用资源，表示替换当前元素

> th和td不同的一点是th带加粗
>
> 页面想不跳转加#或者不写链接都可，但是#可以不刷新页面
>
> input标签：表单中的value值代表向服务器传递的数据；name是表单元素的名称，选项框都要有相同的；
>
> form标签：action表示向何处发送的地址
>
> select标签：由option标签组成，***size***属性表示的是显示行数；option中selected属性设置默认选项

![img](https://upload-images.jianshu.io/upload_images/6322775-bf4beb046ff9b398.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![img](https://upload-images.jianshu.io/upload_images/6322775-d9a21f2143555344.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

把要框选的内容用label包裹

> Auto Rename Tag 标签自动补全
>
> 地址可以用#，target不能用#作为属性
>
> Ctrl+/ 注释快捷键
>
> 只有“按钮”input属性可以用value显示文字
>
> button标签：title属性可以显示悬停文字

---

div独占一行，把标签中的内容分割成块，里面什么都能放；span不换行，文本级标签，不能放p、h、ul、dl、ol、div。

div是专门为css而生的，搭配“div+css”实现各种样式，实际开发中很少使用文本标签，都是容器标签。

容器标签（同时也是块级元素）：div ul li ol dl(定义列表)

div+css只会用到很少的标签：div a span img input ul(无序列表) ol(有序列表) dl(自定义列表) 

span是行内元素，不设为block无法设置宽高

span适用于文字，会自动根据文字长度和大小改变宽高，但是当父元素设置为**flex布局**时，会将未设**宽高**的元素**自动铺满**

------

<img>标签：如果想保证图片等比例缩放，只设置width和height中的一个，剩下的宽或高页面会自适应。

dl（定义列表，用的非常多）：dt(标题)是必须的，dd非必须

<ul>和<ol>都是用<li>，<dl>使用<dt>和<dd>


------

**用什么标签不是根据样子来决定，而是根据结构(语义)，dl和div都是常用结构**

------

表格是由行组成的(行是由列组成的)，而不是由行和列组成

------

Get和Post提交的区别：

Get会以**键值对**的形式将提交的数据追加在后面，以？间隔；

Post发送的数据则是不可见

![img](https://upload-images.jianshu.io/upload_images/6322775-84a55c3bb0152139.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

input属性

value在input中的作用：对于checkbox和radio来说是必须的，该值会发送至表单。对其他是默认显示内容。

placeholder跟value的不同：不会被提交表单，在获得焦点时自动消失；设置定位属性有效(position和left)，但是不能用外边距

------

用id属性的元素会在dom中自动注册，不需要getElement可以直接用id名调用节点方法

------

用在拖拽**目标**上的事件：

ondragstart - 用户开始拖动**元素**时触发

ondrag - **元素**正在拖动时触发

ondragend - 用户完成**元素**拖动后触发

用在拖拽**容器**上的事件：

ondragenter - 当被鼠标拖动的对象进入其**容器范围**内时**触发**此事件

ondragover - 当某被拖动的对象在另一对象**容器范围**内拖动时**触发**此事件

ondragleave - 当被鼠标拖动的对象离开其**容器范围**内时**触发**此事件

ondrop - 在一个拖动过程中，释放鼠标键时触发此事件

------

span标签是inline不是block，所以显示不出来图形样式

------

行内进行运算时需要加括号

------

HTML**属性名**对大小写不敏感，会统一转换成小写，在js中是驼峰式的写法在HTML中写成短横线分隔，或者统一都写成短横线

![img](https://upload-images.jianshu.io/upload_images/6322775-f07c8f6f5807509c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## html的加载顺序

- 解析html结构 -> 加载外部脚本和样式表文件 -> 解析并执行脚本代码 -> 构造html dom模型 -> 加载图片等外部文件 -> 页面加载完毕。

-  html文件从上至下读取，如果“head”中外部引入文件读取慢，则会发生“js阻塞”，只有等前一个文件/服务器读取完才能执行后面的操作

  解决办法：

  - 使用 v-cloak指令<div **v-cloak**>{{msg}}</div>；这个指令保持在元素上知道关联实例结束编译
  - 使用 v-html指令 <div v-html='msg'></div>
  - 使用 v-text指令 <div v-text='msg'></div>
  - 使用template标签将需要渲染的 html 包起来；template标签 是一种用于保存客户端内容的机制，该内容在页面加载时不被渲染，但可以在运行时使用JavaScript进行实例化。

## HTML全局属性(可以与所有元素一起使用)

- title：为元素标签添加鼠标**悬停信息**

- data-*：搭配 `getAttribute(' ')`方法 来获得标签上的自定义 `data-*` 属性；

  - 通过JS中 `dataset.后缀名` 设置/setAttribute('data-*'，值)

  - 或者通过JQ中 `data('后缀名')`方法 设置

- draggable：链接和图像默认是可拖动的

- 其他的还有style和class等

## 取值类型

  1、·**<input type="checkbox/radio">**·值存在dom节点的·**checked**·属性中

  2、·**<input type="text">**·值存在dom节点的·**value**·属性中

  3、·**<input type="text">**·值存在dom节点的·**files**·属性中，是个类似数组的·**只读**·类型，不具有数组常用的那些方法，只能用for循环遍历取出里面的对象，添加到自定义数组中，展示和上传文件时使用自定义数组

## 标签修改原样式

- [<ul>]()：`<ul>`默认有`padding`和`margin`属性，将这两者设为0即可
  - 列表默认垂直排列，可以在`<ul>`样式设置==flex布局==来横向排列
- [<li>]()：前带有小圆点，用`list-style`CSS属性设置清除

## 表格

- `<tr>`表示行
- `<td>`表示一行中的列
  - `<td>`标签属性[colspan]()：合并列。例：`<td colspan="2">`当前列占2格
  - [rowspan]()：合并行。例：`<td rowspan="2">`当前行占2格
  - [vertical-align]()：CSS属性。只有在td中才能居中所有子元素，对于其他标签只会作用于文字

## 表单

- `<form>`标签属性[action="url"]()：将包裹范围内带name属性的元素值通过地址栏方式提交
  - 提交的数据格式`url?name1=xxx&name2=xxx`
- 点击`<input type="submit"`才会提交
- `<input name="mima">`才会将值提交

## 标签细节

- [input]()：==value==即使在HTML写死，在==页面修改==时获取到的也是==当前输入值==
  - [text-align]()：CSS样式。可以调整输入内容位置
  - [readonly]()：标签属性。输入框变成只读，可选中但不能输入
  - ==type="file"==：display：none也可以触发[click]()方法
    - input身上的[change]()事件也可以响应
    - input节点的files虽然存储着所选文件，但是==只读==，**只能**通过==清空==`inputDom.value = null/''`来==重置选择文件==
  - 虽然不同type时获取input节点取的字段不同，但是input节点上有所有不同类型的字段
  
- `<!Doctype HTML>`就是==文档声明==，用来告诉浏览器当前网页版本

- `<meta>`表示声明==字符集==，只有用正确的字符集去==解码==，才不会出现乱码

  - ==content==属性：设备被搜索时的关键词(百度搜索、爬虫可用)。`<meta content="购物,前端">`

- `<html lang="zh-CN">`：屏蔽chrome浏览器默认提示翻译弹窗

- [超链接]()：使用==id==属性给元素打上标记，再用`<a href="#ID">`就可以==跳转==到对应的==元素位置==

  - 可以用==click( )==方法触发超链接的==href==，即使超链接==不显示也可以触发==，可以隐藏超链接，用替换的样式

- ==上标、下标==

  ![img](https://upload-images.jianshu.io/upload_images/6322775-c743b6a11f347c74.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- `<pre/>`：放入预先格式化的文本。==保留空格==和==换行符==

  