###  Vue学习笔记

1. [取对象数组下的属性值](https://blog.csdn.net/qq_31293575/article/details/81132506)
2. 不同数组的数据不能拼接成一个列表
3. `.top-left-down > table td`
- `.top-left-down`表示class
- `>`表示class的父标签下的子标签
- `table td`表示子标签table下的子标签td，**注意中间有个空格**
4. [axios使用说明](https://www.cnblogs.com/peachmeimei/p/8916098.html)
5. [axios安装使用](https://blog.csdn.net/connie_0217/article/details/78703112)
6. **箭头函数**内部没有this，this指向外层最近的调用者
- 箭头函数在调用时，不会生成自身作用域下的this和arguments
- 不像普通函数一样在调用时自动获取this，而是沿着作用域链向上查找，找到最近的外部一层作用域的this，并获取
- 在定义对象的方法/具有动态上下文的回调函数/构造函数中都不适用
7. [vue项目引入bootstrap](https://www.cnblogs.com/zlfProgrammer/p/7993168.html)
8. 使用**Less**编译，html文件依然引入**css文件地址**
9. [Less基本语法](https://www.cnblogs.com/focuslgy/p/3675346.html)
10. 引入的CDN都写到index.html中，就可以在整个项目使用
11. [Vue路由的使用](https://www.runoob.com/vue2/vue-routing.html)
分为四步：
- 第一步：import 昵称 from 真实路径
- 第二步：写映射（`path`是自己定的虚拟路径，component是关键）
- 第三步：创建 router 实例，写配置参数
- 第四步：把router放到vue对象中
12. 链接不要用a标签，用**router-link**
然后用**router-view**显示页面
13. path路径写`*`代表所有，表示没有路径找到就执行`*`对应的文件，用`redirect`导向其他路径
14. **路由配置**写在**主页js**或者**单独的router.js**文件中，注意要import
15. **按钮跳转页面**用方法，在`method`中定义该方法
16. 有了**bootstrap**，不论什么**标签**都可以用**class**来设置样式
17. [二级路由及多级路由](https://ke.qq.com/webcourse/index.html#cid=279700&term_id=100331213&taid=2294397299606676&vid=c1424l4tzfu)
18. **主页**上尽量少写东西，**组件**的东西用**router-view**拼凑在**主页**上
19. **切换页面**的时候，展示**默认内容**，用**redirect**
20. **组件**和**router-view**的区别：
- **组件**是把引用的**模板**直接显示在页面上
- **router-view标签**是**动态**的显示**跳转**的内容
21. 想要对**router判断语句**，用beforeEach和afterEach
22. 想要在其他文件中**import导入**就需要**源文件中export**
- **import**要加**{}**
- 用**export default**导入的时候，不需要**{}**
- export default**一个文件只能有一个**
23. [router-view复用](https://ke.qq.com/webcourse/index.html#cid=279700&term_id=100331213&taid=2294410184508564&vid=n14240dd9j4)
24. 插入背景图片：
在CSS中用`background：url（'...'）`
25. [箭头函数和普通function的区别！](https://ke.qq.com/webcourse/index.html#cid=279700&term_id=100331213&taid=3052085365195924&vid=b1423358dub)
- 用普通function，**this**指向的是**function**，所以会**丢失对象**有两种解决方法：
![第一种（不推荐）](https://upload-images.jianshu.io/upload_images/6322775-c49065277e5065be.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![指向的是**上一层的对象**](https://upload-images.jianshu.io/upload_images/6322775-9dc78a942edab50a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
26. [把地址ID转换成对象](https://ke.qq.com/webcourse/index.html#cid=279700&term_id=100331213&taid=3052072480294036&vid=d1423n80pmt)
- 第一步：多条json数据就是多个对象，把**对象存储到数组**
- 第二步：取出数组中对象的每个ID
27. `this.$route.params.id`
28. ![](https://upload-images.jianshu.io/upload_images/6322775-acbf2fc26f915136.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
29. [bootstrap日期控件的使用及当前日期的获取](https://www.cnblogs.com/eleven258/p/9686250.html)
30. Vue对象里面**属性**才需要加**冒号：**
31. 样式优先级
- **行间样式 > ID > class > 类型选择符**
32. [模态框经典案例](https://www.runoob.com/bootstrap/bootstrap-modal-plugin.html)
33. [**V-bind学习笔记**](https://www.cnblogs.com/vuenote/p/9328401.html)
34. **HTML5 canvas**
- [鼠标画线](https://www.cnblogs.com/qingfengliuyun092815/p/5974565.html)
35. **左键存点，右键绘图**
```html
<!DOCTYPE html>
<html>
<head> 
<meta charset="utf-8"> 
<title>菜鸟教程(runoob.com)</title> 
</head>
<body>
<div id="app">
<canvas id="myCanvas" width="300" height="300" style="border:1px solid #d3d3d3;">
您的浏览器不支持 HTML5 canvas 标签。</canvas>
	</div>
<script src="https://cdn.staticfile.org/vue/2.2.2/vue.min.js"></script>
<script>
new Vue({
  	el: '#app',
  	data() {
    	return {
			info: []
		}
  	},
	mounted () {
		  this.initDraw()
	},
	methods: {
		initDraw () {
			var drawnInfo = new Array();
			var c=document.getElementById("myCanvas");
			var ctx=c.getContext("2d");
			c.onmousedown = (ev) => {
				var btn = ev.button+1;
				if(btn == 1){//存点
					this.info.push({
					x: ev.offsetX,
					y: ev.offsetY
					})
				}
				if(btn == 3){ //循环取点
					ctx.beginPath();
					this.info.forEach((point) => {
						ctx.lineTo(point.x,point.y);
					})//封闭，连成线
					ctx.closePath();
					ctx.stroke();
				}
			}
			c.oncontextmenu = (ev) => {return false;}
		}
	}
})
</script>

</body>
</html>
```
36. ![](https://upload-images.jianshu.io/upload_images/6322775-efcbeaef48885d26.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![](https://upload-images.jianshu.io/upload_images/6322775-e8acc03f69b32cf5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![](https://upload-images.jianshu.io/upload_images/6322775-05fac6de4ef59061.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![](https://upload-images.jianshu.io/upload_images/6322775-6cd2a2f0f99b48de.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![](https://upload-images.jianshu.io/upload_images/6322775-bc13d5b9e904797a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
37. script引入vue，需要在div之后加载