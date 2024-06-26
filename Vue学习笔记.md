# vue2.xx-1

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

# vue2.xx-2

## Tips：

1. 点击div区域外隐藏div，只需要把遮罩改为透明，并加一个点击事件，将v-show标识都置为false
2. created钩子函数在data之后执行
3. 修饰符后还可以跟修饰符，当按键组合的修饰符前后写在一起，可以组成组合键，而不是单个按键触发
4. **绑定事件时**调用vue实例外的方法必须要加“（）”，vue实例里面的方法，想传参时才加()，这是vue帮用户偷懒做的一个处理；**插值语法**和**类似解析JS表达式**的地方**必须要**加“**()**”，因为在**JS**中，**不加括号**，js会认为你是把**函数本身插入到这**个地方，把里外的语句都**展示**出来，但是**加了括号**，JS就会知道你是想运行这个函数，并且只返回它的运行结果
5. data中数据一发生变化，“模板”就会重新进行解析
6. 在vue对象**内部可以**用vue**对象名**称**调用自己**的组件
7. 只要**不是**页面**初始化**的时候同步**显示的内容**就**可以调用异步**的数据
8. “{{}}”本质上是帮你**省掉**了vue实例**对象名**，原本的写法应该是vm.vm最外层的属性名
9. 在“mounted”中操作节点，因为只有这时才将真实节点放入页面了
10. 解决页面闪动(加载速度问题)，可以使用”v-cloak“——这是一种特殊属性，在页面加载完后，vue会删除这一属性，搭配css，可以将写了”v-cloak属性“的地方都隐藏起来，直到vue正式接管”容器“后再删除显示元素标签
11. 要定义**全局**属性、指令等，使用“Vue.method”等函数注册全局
12. 组件渲染速度就是比较慢，在vue实例对象里初始调用节点是可以的，但是组件中不行
13. 函数并不是必须用`**()**`，而是在只需要返回结果时才用`**()**`
14. 数据的根在哪个组件就在哪操作数据！
15. 组件中为什么要把data写成是函数形式?
    - 因为如果是data对象，则在不同组件调用时都在用同一个地址引用，即会发生同一组件的复用在更改不同位置的值时，会同时发生变化。而`data`使用==函数形式==，返回的==对象==，在每次调用`data()`时，返回的都是一个新的对象（虽然其内容一样，但是地址不同）
16. Vue实例对象身上==最外层==会携带`methods`、`data`配置项中的属性方法，调用Vue对象的属性方法直接`vm.xxx`即可，且==会触发==Vue双向绑定

## 响应式

- vue对**数组**中的元素不会进行数据代理，只对**数组元素以外**的所有变量类型，进行数据代理。所以**修改**数组**元素**不会引起vue**更新模板**，只有通过Array中**改变原数组**的==方法==，才会引起虚拟DOM更新(因为Vue对==数组的操作方法重新进行了封装==)；注意！数组中的元素，哪怕是对象也是没有响应式的。(我之前没有遇到这个问题是因为没有修改过单个数组元素，都是整体替换，而数组本身是有数据代理的)

  Tips:

  - 请求回来的数据之所以具有响应式，是因为提前已经在data配置项中==声明了数组==，将请求数据(数组)地址赋给该数组，里面的所有变量都具有了响应式

- 数据**劫持**和数据**代理**是一个意思，都是用**defineProperty()**

- **只有data或者set()中的属性变量才有响应式！**

  - ==小技巧==：请求回来的数据先进行构造、添加属性，==再==赋值给==data中提前定义好的变量==，即可为自己添加的属性绑定响应式
    - 配置项data中声明的数组用`push()`等方法也会给==没有响应式属性==的对象添加响应式
    - **一旦**对象已经==被数据劫持==，再给该对象用`obj.key`的形式添加新属性，[defineProperty]()是监听不到的！再去操作数组`list.push(obj)`，也只能监听到数组发生了改变，但是对象的改变监听不到！因此不会再给对象身上新添加的属性绑定响应式！
  - js中变量没有固定类型，因此赋值给==data中变量==的是**基础数据类型**还是**对象**都具有响应式

- Vue代理思路

  ```js
  首先封装一个函数 将data中的数据传入		function ob(obj){
  遍历传入的对象 取出key 做数据代理				let keys = Object.keys(obj)
  										 keys.forEach((k)=>{
  注意这里有一个很聪明的做法 						Object.defineProperty(this,k,{
  将ob函数的this传入进去							 get(){
                                                     	return obj[k]
                                                     }     
                                                     set(val){
                                                      obj[k] = val
                                                      }	
                                                 }) 
                                               })   
                                            }
  ```

- Vue的响应式不仅会重新解析==HTML==模板，而且会==重新执行==**引用**了响应式变量的==方法==！

## vue指令：

- ![img](https://upload-images.jianshu.io/upload_images/6322775-07bad3e6685ee9da.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

<center>两个方式的区别在于v-text是将标签内的内容全部提换；另一个是只替换指定位置

#### v-text 

​	替换文本

#### v-html 

​	识别变量里面的html标签，并用正确的方式渲染出来；使用方法：<p v-html="vue变量"></p>

#### v-on 绑定事件

- 有v-on:click、v-on:monseenter。

- vue提供了一种简略写法@click。绑定的方法在methods中定义；若想访问**原始DOM**事件可以在传实参的时候加入**$event特殊变量(函数自带的默认值)**，就可以取到**原生**事件的**对象**(==但是==如果默认事件传入的是多个参数，多个$event==并不能==代表多个参数)；使用**修饰符**要注意**顺序**；使用keyup修饰符可以拼接组合键位；**.exact**修饰符可以限制仅限对应键位时才触发

- 没有使用v-on就无法使用Vue-==methods==中定义的方法，甚至==data==中的变量都无法传入，只能使用外部声明的变量

#### v-bind 动态绑定

- (但是**单向，只能从data流向页面**)，可以用v-bind:动态变化值 = "表达式"；要用vue动态改变值的HTML原生属性都需要用**v-bind:属性名**；除了用**字符串表达式**，**动态绑定**属性时**必须**用**对象**和**数组**表达式，使用方法：:class="{ 样式:vue属性(true/false) / 样式：true/false }"、"[ '样式1', '样式2' ]"，样式用引号；v-bind绑定可以针对某一属性使用三元运算**:**style="{ fontSize：isRed ? '30px' : '16px' }"，style的**属性必须**是**驼峰式**或者用括号括起来；**对象**必须是**键值对**的形式，:class时必须设一个可判断的值
  - class也可以动态绑定，第一个参数是默认样
  - 可以建立一个空对象，在触发事件的时候建新属性赋初值
  - 在==挂载==时进行

![](https://upload-images.jianshu.io/upload_images/6322775-538dae39965c59ac.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### v-model 

​	用在**表单**控件上，实现**双向绑定**。双向绑定的原理就是“v-bind：value”接收从data流入的值，“v-on：事件”监听事件改变data里的值，合起来就是v-model；“v-model”默认是收集表单中的“value”，对于“勾选框”没有默认value，就需要手动在标签上写一个value；且对于“多选框”，“v-model”接收值**必须**用数组形式，相当于在data定义时，进行了一个数据类型验证；

##### 	v-model修饰符

​         **“.lazy”**将输入时的**立即**同步转变为，在**“change”**事件**之后**同步，即输入框“**失去焦点**”后才同步数据

​        **“.number”**将输入值转变为“number”类型，非数字类型的字符以及之后的字符(无论是否是数字)将被舍弃；**不能**和“type=“text””兼容

​        **“.trim”**将输入框“**首尾**”的“**空白字符**”抹掉

#### v-once：

​	在页面初始渲染后就视为静态值，只要页面不刷新，后面值再如何变都不会改变的“初始值”;

​    注意跟“事件修饰符**once**”做区分，once是**事件**只执行一次，而v-once是**渲染内容**只渲染一次

#### v-pre：

​	会“跳过”编译，加快渲染速度，即html标签中写什么内容就原封不动的还原到页面上，通常用于“没有插值、指令的标签节点”

#### 自定义指令：

- 使用“directives对象”作为配置项，自定义指令时有两种写法：

#####     函数形式

- 相当于对象形式的==bind和update生命周期==，默认会传入4个参数

  - `dom`：==标签节点==
    - 可以直接对**节点进行操作**
  - `binding`：指令接收值等==详细信息==
    - 参数中的value则可以获取到**自定义指令属性**传入的值，如`v-loading="true"`
  - `vnode`：Vue 编译生成的虚拟节点
  - `oldVnode`：上一个虚拟节点，仅在 `update` 和 `componentUpdated` 钩子中可用

  ```js
  directives: {
      my_loading(dom, binding, vnode, oldVnode) {
          binding = {
              name: '指令名 不包括v-前缀',
              value: '绑定值',
              oldValue: '指令绑定的前一个值',
              expression: '字符串形式的指令表达式 如v-loading="true"',
              arg: '传给指令的参数 如v-focus:foo中的foo',
              modifiers: '包含修饰符的对象 {foo: true}'
          }
      },
      // 示例
      my_loading(dom, data) {
  			// 判断页面中是否存在加载遮罩
  			let load = document.querySelector('.my_loading');
  			let { value } = data;
  			if (load) {
  				// 存在则修改位置和大小
  				load.style.width = `${dom.clientWidth}px`;
  				load.style.height = `${dom.clientHeight}px`;
  				let p = dom.getBoundingClientRect();
  				load.style.left = `${p.left}px`;
  				load.style.top = `${p.top}px`;
  				// 如果传入值为false则不显示
  				load.style.display = `${value ? '' : 'none'}`;
  			} else {
  				// 不存在则创建
  				load = document.createElement('div');
  				// 设置样式
  				load.className = 'my_loading';
  				// 设置子节点
  				load.innerHTML = `
                      <div class="icon">
                        <svg viewBox="25 25 50 50" class="box">
                          <circle cx="50" cy="50" r="20" fill="none"></circle>
                        </svg>
                      </div>
                  `;
  				// 根据绑定元素设置大小
  				load.style.width = `${dom.clientWidth}px`;
  				load.style.height = `${dom.clientHeight}px`;
  				// 根据绑定元素确定页面位置
  				let p = dom.getBoundingClientRect();
  				load.style.left = `${p.left}px`;
  				load.style.top = `${p.top}px`;
  				load.style.display = `${value ? '' : 'none'}`;
  				document.body.appendChild(load);
  			}
  		},
  }

#####     对象形式

- 有多个声明周期
  - `bind`：只调用一次，在第一次绑定到元素时调用，一般用于初始化设置
  - `inserted`：被绑定元素插入父节点时调用(仅保证父节点存在，但**不一定已被插入文档**中)
  - `update`：**所在组件**的 VNode 更新时调用，但是==可能发生在其子 VNode 更新之前==
  - `componentUpdated`：==指令所在组件==的 VNode 及其==子==VNode**全部更新**后调用
  - `unbind`：只调用一次，指令与元素解绑时调用

##### 	TIps：

- 所有指令相关的this，都不是vue，因为指令方法已经把节点传入给用户操作，剩下的事跟vue没什么关系了，都是dom操作，这块比较特殊，需要注意一下

- v-if只会触发`bind`、`inserted`周期，v-show只会触发一次bind和inserted，v-show的变化会引起模板重新解析，因此`update`会触发

## 计算属性与watch

- ==计算属性==

  - 计算属性简写形式的值是==只读==的！不能赋值
  
    ```js
    computed:{
        // 简写形式相当于getter 不能赋值
        fullName() {
            return this.firstName + ' ' + this.lastName
        },
        // 要具备赋值功能需要写完整形式
        fullName:{
            get() {
                // getter中的this本身指向的是windows 但是vue把this指向改为了vue实例
                return this.firstName + ' ' + this.lastName
            },
            set(newValue) {
                let name = newValue.split(' ')
                // 注意 此处修改的是getter方法所依赖的变量
                this.firstName = name[0]
                this.lastName = name[1]
            }
        }
    }
  
  - computed计算属性和methods是不同的，computed是基于响应式的，data中的值没有发生改变，不论刷新多少次都不会重新计算（缓存），会返回之前的结果，所以**computed适合双向绑定**时使用，这样在检测到数值改变的时候才会调用。
  
  - 计算属性的意义就在于全写在html页面上难以维护和阅读，写在实例里，使用起来就像**data**中的属性一样  
  
  - 何为计算属性？即用“data”中定义的“原有属性”，进行“加工处理”，生成“新属性”(例如：firstName和lastName加工生成fullName)，这个“新属性”是可以当作“data中的属性”直接调用的，而不是视为“methods”
  
  - “计算属性”跟“methods和data”中的属性不同，因为它里面没有默认函数，但是计算属性有，所以vue对象上绑定的其实是计算属性中“get()”的返回值，然后赋值给了跟计算属性名称同名的“新属性”
  
  - 计算属性命名不能是`'obj.key'`这种，只能是单层的`'key':{get(), set()}`或者`key(){}`
  
  - 侦听器虽然可以监控数据变动，但是书写比计算属性更麻烦，需要将改变的属性先声明再监听，但是计算属性可以省略声明
  
  
  
    - get函数中使用“this”，本身的指向应该是windows，但是这里vue帮你做了一件事，就是把“this”的指向改成“vue实例”，所以在这类vue给定的特殊函数中，可以用this
  
  
    - `data`配置项先于==计算属性==执行，所以一定不要把计算属性用在`data`中做==初始化==
  


- ==侦听器watch==：
  - 带**$**符号的是vue实例对象**自带**的属性和方法

  - $watch('监听的属性'，function(变化后的值，变化前的值) {代码})

  - 只要有==变化==就会==触发==侦听器，无需对比新旧值
    - `watch`其实内部进行了一次新旧值对比，如：监听变量是==基本数据类型==，修改变量为原值，==不会触发==`watch`；同样，监听的如果是对象，则实际监听的是对象在内存中的==地址值==，因此改变对象下属性并==不会触发==`watch`

  - **watch**用在执行**异步**和开销较大的计算时更适合；最适合的场景是输入框联想，在输入内容时是中间状态，但是停止输入后开始执行方法（限制操作的频率）


![img](https://upload-images.jianshu.io/upload_images/6322775-ea5ac780ccd1e2fd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

<center>watch可以用于限制输入，因为输入的时候先进来的是newValue，如果不做任何操作才会显示在界面上，但是如果做条件判断再给监听的属性重新赋值，看起来就会像输入框限制用户输入一样

- **watch**可以侦听**计算属性**

![img](https://upload-images.jianshu.io/upload_images/6322775-93d6e8b49c55be6d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

<center>watch其实是根据字符串查找属性，所以“$watch”中第一个参数也是“字符串形式”

- 深度监听：

![img](https://upload-images.jianshu.io/upload_images/6322775-349fcd5bbca90c5e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

<center>当data中数据层层嵌套时，用简写的形式——直接写属性名是不行的，因为“key：value”的key“不合规”，所以要用完整形式书写——字符串形式(当中嵌套调用的“.”是不能省略的哦)

![img](https://upload-images.jianshu.io/upload_images/6322775-5f8b0966eac8f4dc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

<center>想要监听嵌套对象本身是否发生改变，需要使用“deep”选项，因为监听对象，只是监听它所引用的地址，其内容本身的变化并不会触发监听

- 计算属性和侦听器不同：
  - **计算属性不需要**在data中实现**声明**，可直接引用，但是需要“return”返回值(计算“属性”本身是不存在的，而是通过“已有属性”计算得来)
  - **侦听器**则是在**已有**声明**变量**的基础上进行运算，赋值改变已有值，不需要“return”
  - “计算属性”是加工“已存在”属性“生成”新属性，而侦听器是“修改”data中“原有属性”
  - 监听是监听“改变”的数据，而计算属性是创造新的属性“展示在页面上”
  - 计算属性**必须**要加“**return**”，所以有**限制**，而watch不需要**返回值**，所以可以畅快的写定时器等复杂任务
  - 打印结果——“计算属性”不是函数，也不能“加()”，methods不管加不加括号，类型都是function

## 生命周期

- 生命周期：给用户不同阶段添加自己代码的机会

- beforeCreate：vue**对象创建后**，规则制定完毕

  - 声明的函数都还没有

- created：方法和回调都==已经配置==，但是跟html页面对应的属性和方法还没有挂载上去，$el不可见；

  - 请求一开始能看到的页面元素数据
  - data中的变量和methods中的方法都已经创建好了，但是在生命周期函数中为“data变量”赋值时，data中的数据是不能为空的，要么随便给一个初始值，要么用“Number、String”等类限制传入数据类型

![img](https://upload-images.jianshu.io/upload_images/6322775-85b5be56f78a787b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- beforeMount：所有对节点的操作都不会奏效
  - beforeMount及之前的周期函数都==先于[v-bind]()==执行
  - beforeMount及之前的周期函数都==先于组件任何生命周期执行==

- mounted：挂载成功，html节点都已被vue节点**替换**
  - **注意！**==mounted==只是==挂载节点==，还没==渲染==，渲染前元素大小获取到都是0
  - ==组件==在此时才被放到真实dom节点
  - 此时[v-bind]()触发的方法已经==先于mounted==执行了！
  - 挂载节点，即，所有标签上的方法都已经处理完毕。在mounted生命周期再创建静态变量就晚了
  - `mounted()`执行时，==组件==已经==挂载完毕==，该执行的方法都执行完了

- beforeUpdate：数据变化但是没传递到虚拟DOM
- updated：虚拟DOM和html上的都已经更新
- beforeDestroy：监听、子组件和事件未被销毁前
- destroyed：销毁完成
- 通过`let vm = new Vue()`接收创建的实例对象，**不能**在==beforeUpdate之前的生命周期中获取到！==
  - 因为beforeUpdate之前的生命周期都是在==初始化==，初始化没完成之前，==new Vue还没有返回值！==
  - **但是**==不论在哪个生命周期==，==this==都能取到vue实例，因为钩子函数也是函数，this指向它的调用者，也就是内部正在运作的vue实例，但是new Vue还没有返回这个实例

![img](https://upload-images.jianshu.io/upload_images/6322775-7de7c499c66ac7e6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- 配置项==执行顺序==
  1. `props`：父组件传递给子组件的属性优先被处理
  2. `data`：处理完`props`后才处理，将其加入Vue实例中
  3. `computed`：处理完`data`后计算属性添加到Vue实例中
  4. `watch`：在得到计算属性后添加侦听器
  5. `created`：处理完以上选项后，才调用声明周期钩子函数，完成实例化
  6. `mounted`等生命周期
  7. `methods`：在`mounted`后将方法添加到Vue实例中

## axios异步

- 使用axios或者ajax发送请求时默认异步进行的，此时如果赋值给作用域外的全局变量，在请求完成前，全局变量会在未赋值的情况下被调用。
- 解决办法：在then里面写下需要调用到全局变量的方法(将其封装，便于阅读)
- 异步执行的方法，即使把后续函数写在该方法后面也不会及时获取到数据

------

vue实例中不同函数之间要调用需要加this(此时this指向的是window)，即window.xxx(属性或者方法)

methods中方法互相调用：通过this.$options.methods.方法名查找methods中的方法

## keydown/up便捷事件：

- @keyup.13 回车 
- @keyup.enter 回车 
- @keyup.left 左键 
- @keyup.right 右键 
- @keyup.up 上键 
- @keyup.down 下键 
- @keyup.delete 删除键
- @keyup.enter 回车
- @keyup.left 左键
- @keyup.right 右键
- @keyup.up 上键
- @keyup.down 下键
- @keyup.delete 删除键
- @keyup.enter 回车
- @keyup.left 左键
- @keyup.right 右键
- @keyup.up 上键
- @keyup.down 下键
- @keyup.delete 删除键

## 组件

- 在==根实例==[beforeMount]()之==后==，正式开始处理挂载时才执行组件的初始化
- [Vue.component('组件名',{ 配置项 })]()
  - **组件必须声明在new vue实例之前！**
  - component**读取不到**vue实例和**其他**组件中的**data数据**，有效避免了变量污染
  - ==vue根实例==也获取不到组件定义的属性
  - ==不==在页面==使用==就==不会初始化==操作，也不会报错
  - ==组件名称==是在HTML页面上使用的==标签名==


![img](https://upload-images.jianshu.io/upload_images/6322775-43c0d72fa179b58c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- [template]()内如果想写入多个标签，需要用标签包裹，其原因是组件中==只能有一个根元素==

- 每个同名的**组件**会**独立**维护自己的数据，每创建一个同名组件都有新**实例**被创建

- data
  - 必须是一个**函数**，这样**同名**的**组件**才能**各自**维护一份return回来的**拷贝**；

- props：
  - ['属性1'，'属性2']可以为**组件**创建**自定义属性**，其作用是为**同一个**组件在传入值时**分别创建**和维护**独有**的**属性（就类似data函数中的return，返回的是独一份的拷贝）**，使用props属性值的方法跟data中的属性一样；
  - **porp**可以接收任意传进来的值，并在template中（在其他地方用this.属性名的方式调用）用跟data一样的方式调用，**插值**的时候写的是**props**定义的**属性名**；
  - prop在用变量赋值时，需要用v-bind表示传入的是一个表达式而不是字符串

- ==与extend({配置项})的区别==：
  - component是“**全局注册！**”，而extend是`let 对象名 = Vue.extend({})`的形式注册的`局部组件`
  - component('组件名'，extend对象名)
  - extend使用时：`new Vue({ components:{ **组件名：extend赋值对象** } })`

- 在根实例对象使用组件是==平级的==，但是组件中可以用`components：{}`包裹其他组件，并在`template`中调用

- 组件之所以能`同名且维护不同的对象`，就是因为`extend`中返回的是`新创建的function Vuecomponent()`，就是每次调用都创建一个新的函数

- ※组件是在==挂载==时就已经==执行完==身上立即执行的方法了！

- 给组件上添加的==class==、==style==会直接添加到组件==根节点==上

- [局部注册组件]()

  ```js
  let component = {
      template:xxxx,
      data(){return { } }
      ...
  }
  new Vue({
      ...
      components:{'自定义':component},  完整写法 页面<自定义/>
      components:{component},	简写 页面<component/>
  })
  或者
  Vue.component('xxx',{
      ...
      components:{component} 同上
  })
  ```

- 不管是==全局==组件还是==局部==组件，[mixins]()==混入==对象**必须**==声明在组件**之前**！==

- [mixins: [ 变量1,变量2, ... ] ]()：将外部文件/变量混入当前组件/根实例

  - 组件是独立作用域，所以每个组件都要混入才能使用外部声明的变量和方法
  - 混入==优先==于组件==子身的同名==方法和变量。例如，混入文件里有mounted/data配置项，它的执行优先于组件内写的mounted/data

- 使用外部组件库

  - `Vue.use(xxx)`即可

- ==自定义组件==上使用`v-model`

  - `v-model`本身就是`v-bind:value`和`v-on:input`的语法糖，所以自定义组件身上只要复现这两者，就可以用`v-model`

  ```vue
  // 子组件
  <template>
    <!--子组件上实现语法糖内容-->
    <input
      :value="value"
      @input="$emit('input', $event.target.value)"
    />
  </template>
  
  <script>
  export default {
    props: {
      value: String, // 接收父组件传递的值
    },
  };
  </script>
  
  // 父组件
  <template>
    <div>
      <!--这里就可以用v-model-->
      <CustomInput v-model="message" />
      <p>输入的内容是：{{ message }}</p>
    </div>
  </template>
  <script>
  import CustomInput from './CustomInput.vue';
  
  export default {
    components: {
      CustomInput,
    },
    data() {
      return {
        message: '', // 父组件的数据
      };
    },
  };
  </script>

- Tips

  - 注册使用组件`components:{xxx:component1}`的位置**必须**在==组件声明之后==！


## 插槽

- 组件**不接收HTML**页面上的值，除非在`<template>`中使用`<slot>`标签（除了class，写在标签上是可以识别的）

- 不论具名还是匿名插槽`<slot>默认内容</slot>`内写入的内容会==默认显示==，当HTML传入内容时会覆盖掉

- ==v-slot==只能与==template==搭配，不能与其他标签使用

- ==匿名插槽==

  - 只要不写`name`标签属性的都是匿名
  - HTML上写入的未绑定名称的内容、结构全部会塞到`<slot></slot>`中间，==组件内==有**多个**匿名插槽，页面上就会==复制多份==

  ```html
  组件内
  <slot></slot>             <slot></slot>             同左
                            <slot></slot>
  HTML页面上                                           匿名插槽 用 v-slot绑定名称是default
  <h2>匿名</h2>               同左                     <template v-slot:default>
  <p>匿名2</p>                                                                 <p>xxx</p>
  <div>xxx</div>                                      </template>
  
  页面效果
  匿名 匿名2 xxx          匿名 匿名2 xxx 匿名 匿名2 xxx
  ```

- ==具名插槽==

  - ==name==**不能**用==驼峰式==命名，可以用

  - ```html
    组件内
    <slot name="xxx"></slot>
    
    HTML页面
    <template v-slot:xxx>
    	<p>v-slot 绑定名与组件内的 name名 对应</p>
    </template>
    ```

- ==动态展示==插槽

  - 接收到的都是对象

  ```html
  匿名插槽                                    具名插槽
  <slot :存值名="组件内响应式变量"></slot>      <slot name="t-t" :存值名="组件内响应式变量"></slot>
  
  HTML页面
  <template v-slot:default="接收数据名">        <template v-slot:t-t="接收数据名">
  	<p v-show="接收数据名.存值名">...</p>          <p>{{接收数据名.存值名}}</p>
      <p v-show="!接收数据名.存值名">...</p>
  </template>                                  </template>

## 子组件给父组件传值：

- 通过[$emit]()及[$on]()触发自定义事件：
  - `<component @自定义事件="fatherFun(参数1,参数2,...)" />`、`子组件内：this.$emit('自定义事件', 参数1,参数2,...)`
  - 子组件在需要**传值的变量后**，使用“vue实例.$emit('自定义事件名'，[传递的值])”，然后在**子组件标签**处使用  `v-on：自定义事件名="父组件声明变量=$event（传递进来的值）"或是"自定义函数（$emit传进来的值会作为第一个参数传入这个函数）"`，
  - “this.$emit(**event**：'submit' )”，可以直接传递“methods事件”
  - 父组件声明一个新的data属性用来接收这个值
  - 不能跨组件传，只能一层一层传递；使用“provide/inject”新特性可以打破这一限制，直接跨层传递
  


![img](https://upload-images.jianshu.io/upload_images/6322775-973e1f485034f113.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

<center>provide是个函数，内部用return形式返回一个“prop属性”</center>


![img](https://upload-images.jianshu.io/upload_images/6322775-65e782fcfaf8ad48.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

<center>在“不论层数”的子组件中用"inject"来接收，写法类似"prop"</center>

- ※`$emit`其本质是**`触发`**自己定义的事件，而`$on`或者`v-on`则是`监听`事件，当`$emit`触发时，`$on、v-on`则收到其传过来的参数
- 通过[$parent]()获取父级方法/变量：`this.$parent.fatherFunction(参数)`或`this.$parent.变量`
- 将父级函数直接传入进去：`<component :自定义名="fatherFun" />`、`组件内：props:['自定义名']`、`this.自定义名(参数)`


## 子组件间互相传值：

- 依靠==全局事件总线==
  - 安装：
    - 在==根实例==中的`beforeCreate`周期函数中为==原型Vue==绑定一个属性——`Vue.prototype.xxx = this`(vue实例)，因为在组件中是通过this(组件实例对象)在原型链中的关系来调用属性
    - 这是依靠vue及实例对象所拥有的==原型(prototype)链==特性，因为原型链可以使得不管是多深的子组件都能看到vue所定义的属性，通过在原型链上挂载vue或组件实例对象(只有==构造函数==和==原型==对象才有`$on`等方法，而**实例**对象本身没有，是通过原型链从原型对象上找的)，**子组件**借此在==原型上的实例对象==上触发和定义事件，再在另一个任意组件中==借原型属性==监听==此事件==，以此达到跨层级互相传递参数。
  
    Tips：
  
    - `$on('事件名',callback)`第二个参数是监听的事件函数，默认参数是`$emit('事件名',值)`传递过来的值
    - 全局事件总线是==全局==变量，因此`this.$bus.$on`监听事件==不会==随组件==销毁==，因此**必须**在组件生命周期中手动清除`this.$bus`身上的监听事件
      - 注：销毁监听事件使用`this.$bus.$off('eventName')`，该方法同时会销毁所有同名事件，未销毁的组件中监听事件也不会触发了

## $refs对象

- 可以获取到用ref=“name”注册的**dom元素**或者**component实例对象**；
- 但是ref不是响应式，不能用v-bind；
- ref是在渲染结束后才存在，created的时候还不存在；
- 避免在component和计算属性中应用；
- 用在**v-for**中会返回子组件**数组**；
- 通过“vue.**$ref.name**”(标签中使用过ref属性的都会被$ref对象记录在册)获取到整个标签对象
- ref获取到的节点和dom节点不一样！ref虽然指向dom节点，但是身上绑的属性都是不同的
- 有==多个同名==`ref对象`时，会`this.$refs.ref对象`会得到一个数组

## VueAPI：

#### 		set/$set(target，key，value)：

- 当vue data数据中原先未定义变量，而**后续**想**添加**时，**不能直接用js赋值语句添加！**因为后续添加的数据未在data中定义，vue就**没有**给它添加**数据劫持**，想加一个原先不存在的变量到**已有变量**上，就要使用“vue.set”方法，vue会给它添加一个“get()、set()”。**但是**这种方法也有其**局限**——就是不能在“vue.data”中直接添加，只能在data中的变量下：其中“**key**”可以是**数组索引**
  
  TIps：
  - **不要**在需要`频繁`触发的地方设置`$set`，这个方法跟`v-if`一样，在频繁切换显示的地方显示效果不佳
  - data中提前声明的==数组==具有响应式，用`array.push`的方法添加的对象也具有响应式，**但是**只有==数组在push等操作的对象==才具有==响应式==，**其后**对数组下对象的任何`obj.xxx`等操作均不具有响应式，所以想具有响应式，**必须**在数组push之前或者之后用$set
  - ==value==填入多层结构，也一并会绑定响应式

####  $nextTick(回调函数)：

​	因为vue在执行一个事件时，是将`作用域内`所有代码`**执行完**`后再解析模板，因此有一些延后渲染的效果就不能实现，就需要用`定时器setTimeOut`或者`$nextTick`。`$nextTick`是告诉vue，在模板**`解析完`**毕后**`再执行`**里面的回调；当某个操作使用的**dom结构在变化**，那么都应该放入“**Vue.nextTick()**”的回调函数中

#####  使用场景：

  - 数据改变后，基于`**新DOM**`节点进行某些操作；

  - 用第三方插件，dom发生变化，想重新应用插件
  - Vue操作dom完，此时**虚拟**dom**尚未**放入页面，放在nextTick中的数据将在**真实**dom**放入页面**后调用；

Tips：

- 在多层for循环中 使用$nextTick会在所有层级的for循环执行完在调用，因此无法一次循环执行完渲染一些节点

### 原型链：

![img](https://upload-images.jianshu.io/upload_images/6322775-eb2867b76670dcda.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```
构造函数`和`实例对象`在原型上的上一级都是`**原型对象**`，而原型对象的上一级则是`object原型对象`，**但是**vue特别的将组件原型指向改变为`**vue原型对象**
```

### Vue动画：

- 使用`<transition/>`标签包裹要显示的内容，并用`v-enter等-active`事先定义好动画，在`<transition/>`中的元素显示或隐藏等生命周期时会自动应用v-开头的css

- 多个元素标签要放在一块同时控制，必须用`<transition-group/>`包裹，并且要在应用动画的标签上绑定`key`

![image-20220701172405852](C:/Users/admin/AppData/Roaming/Typora/typora-user-images/image-20220701172405852.png)

- 生命周期总是分为三个阶段：开始状态enter、进行中enter-active、结束状态enter-to

![img](https://upload-images.jianshu.io/upload_images/6322775-a25eb3b0bfa2d3c4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- 使用@keyframes叫动画，使用enter、enter-to叫过渡

![img](https://upload-images.jianshu.io/upload_images/6322775-476428b1535beb52.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

<center/>给active加上持续时间

- **name：**可以更改`v-`开头的默认设置，用于标记不同的动画
-  **appear：**是否在初始渲染时使用过度

- **enter-class等：**适合搭配第三方样式库

- ==列表排序过渡==：新增`v-move`class。会从原位置平滑过渡到新位置

  ```css
  <transition-group/>
  .v-move{
   transition: all 0.3s;   
  }

- TIps：
  1. `<transition/>`只能应用在**单个标签**
  1. 删除==数组==元素时，周围的元素会瞬移，解决这个问题需要在`v-leave-active`过程中设置`position: absolute`，并设置`v-enter`和`v-leave-to`的动画，其他生命周期动画可以删除
  1. `<transition/>`和`<transition-group/>`可以加class，会应用到`span`标签上
  1. `<transition/>`在HTML上不会渲染，但是`<transition-group/>`在HTML上是以`<span>`标签存在
  1. ==拖拽动画==：用`<transition-group>`包裹`v-for`渲染的元素，并在元素上设置`transition`样式，当修改数组时，会自动应用过渡效果
