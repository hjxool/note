## Tips：

- 当**需要**给类型相同的元素**循环绑定函数**时，可以用给元素**添加**自定义标签等方法**标记**，在**触发**事件函数**时**，**获取标记**，传递给同一个事件函数，而不需要创建多个同类函数；已经生成的元素跟它之前所在的数组就没有关系了，不能反推出在之前数组所在的位置

- 拖拽排序的时候，设置“目标位置 != 当前”，可以使程序只执行一次；并且要注意有没有接触到外边框，设置条件在外边框不触发；排序逻辑：因为使用的前插入方法，所以小的插在大的后一个的前面，大的插在小的前面；排序判断条件当前索引大小和目标索引大小做比较;计算索引逻辑：不断循环用前一个节点赋值(‘=’)，直到前一个节点为空，累计计算的自定义变量值就是节点的索引值

- 对象.自定义属性 = xxx相当于将xxx赋值给对象下新创建的属性

- 为了避免残留值，应当在**“即将”**调用和请求的地方清空再赋值。

- 滑块必须用clientX/Y计算鼠标到浏览器边框的距离，因为只有键鼠**事件对象**是**动态**值（随鼠标动态改变），而offsetLeft等都是**元素对象**的属性，是**静态**值；还有mousedown事件包裹mouseover最好不用点击的对象作为调用者，最好用window，因为鼠标滑动过快，超出点击的对象就会出现问题

- **三元运算**的选择结果不止是字符串，还可以是表达式，比如另一个三元运算

- “mouseenter”和“mouseleave”可以替代css“hover选择器”，“e.target”有scr属性可以更改图片地址，准确的说“e.target”包含目标的所有属性

- **表达式**和**语句**的区别：“表达式”由运算符构成，并“运算”产生一个“结果”；而“语句”包括“声明、赋值、条件判断等”，只是一句命令或者句子。像**JS**和**vue**中需要填入“表达式”的地方，就不能用“语句”，需要注意区分，而表达式一旦加上**“；”**，就不再是“表达式”，而是**表达式“语句”**，从而不被识别报错，但是加“逗号，”是可以的

- 想要获得随机数，JS原生方法中只有“random()”，但是可以借助“Math.floor(Math.random() * num)”，用**“[0~1)”**的随机数乘“总数”，再向下取整，即可获得“[0~(总数-1))”的随机数了

- 想要折叠注释掉的代码，使用“//#region 代码块 //#endregion”，就可以把中间的内容全部折叠

- **实例化对象**就是**构造函数**的**调用**者

- **对象**中的**函数**可以用“名称+()”的形式，关键词**function**可以删掉

- 对象里的“key”都是“字符串”，所以当格式错误时，写成字符串形式用引号包裹

- for循环是取不到数组外层定义的单独变量的

- 想要取小数点“后几位”，就“先取整再整除”——取**几位**就乘以10的指数，**取整**后再除以10的指数；另外js原生方法提供的都是**向下取整**，想要**向上取整**，加个**0.5**就可以了

- 对象才可以赋值修改原值，变量赋值修改原值不变；**但是！**对象复制后，**置空**再修改原值，**不会**影响复制的对象

- Number没有“length”属性！

- 对象取用属性不只用点，还可以用·**[ '属性名' ]**·，当取用的·属性名只有后缀不同时·，可以使用·**[ `前缀${动态后缀}` ]**·

- **折叠注释**·//#region ...代码块... //#endregion·

- **移除事件监听**时，传入两个参数，第一个是想解绑的事件名，第二个是**指向同一处地址**的方法**回调函数**

- **不同层**级的**js**文件中的函数**作用域**以**引入**js**的html**文件为**根目录**

- js里**有return**的表示需要立即执行，在html页面引用的时候就要**加括号**

- ==switch==语句==case==只会执行==一次==！如果`case`匹配中了且==后面没有break==，则后面的case不再执行匹配

- 当使用==！=排除某些条件时==要用[&&]()并列，当使用====匹配某些条件时==，要用[||]()

- ==null==并不是什么都没有，而是一个==空对象==，==undefined==才是什么都没有的未定义数据

- ==同时声明==：`let a=1,b=2,...`等同于`let a=1,let b=2`

- ==变量==类型指==栈中值==的类型，比如：==引用地址==、数字等
  - ==数据==类型指==堆中数据==的类型，比如：==对象==等

- js中==—等符号==表示运算符，所以==不能出现==短横线分隔命名，而HTML没有运算符之说，可以用短横线命名

- jQuery的$("类名/标签/Id").eq(N-1)方法返回**被选元素**的**指定**索引号**的元素**

- getElements打头的元素获取方法返回的是集合，就算只有一个元素也是集合的形式，所以没有appendChild方法

- drag事件中，想要**drop**必须阻止**dragover**的默认操作

- getBoundingClientRect()方法可以获取目标大小及**相对于窗口**的位置

- transform是相对于自身**当前**位置

- 自定义属性**必须以data-开头**，取值通过`dataset['']`获取（注意:data-my-name在取值时要写成myName）

- js**原生**事件处理函数事件名必须叫`event`

  - `onclick = function(e)`和`addEventListener`可以用其他标识表示event，但不能不传参；如果要**传入其他参数**，需要进行**封装**

    ![img](https://upload-images.jianshu.io/upload_images/6322775-8524511c83b9d754.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

    <center>封装</center>

  - 在HTML事件处理程序中可以直接传入多个参数，并且可以不传入event直接传参，非常看重event关键词，可以直接使用

  - 在`οnclick=function`以及`addEventListener`中不再看重event关键词，可以传任意标识，但只能传event。**简单来说**就是“html页面”要写就写完整“event”，“js页面”可以用任意标识符表示event

- `addEventListener`和`on事件`的区别是：对同一个DOM，on事件唯一，但是事件监听器可以添加多个，并先后执行，不会后一个覆盖前一个

- JS中写在==字符串==里的命名不能用==**驼峰式**==！因为已经当作是放在HTML属性的格式，==驼峰式==只能是==不用引号包裹==的才能在HTML上解析为==短横线分隔==

## 运算符、类型转换

- [%取余数]()

- typeof返回数据类型；NaN的返回值是number，NaN是个特殊数字

  - null是空对象，返回的也是object

- ==隐式==类型转换：`num = num+“”等同于String(num)`，同理`string = +string等同于Number(string)`

  Tips：

  - 但是任何数字和字符串==相加==都会转换为==字符串运算==

- `Number()`等方法不会转变原值，都是依靠返回值赋值给自身

  - 但是[Number]()有个缺点，==无法处理==`100px`类型的字符串，只要字符串中带不是数字的字符，就会转化为`NaN`
    - `NaN`特点
      - 自身不相等`NaN === NaN //false`
      - JS全局函数`isNaN(value)`
      - `Object.is(value1, value2)`判断两个值是否相等，用这个方法判断NaN可以等于自身`Object.is(NaN, NaN) //true`
  - ※因此需要[parseInt]()，解析并转换字符串(注！==只能转换字符串==，number是可以转换其他类型的)，但是要求字符串==必须以数字开头==
    - 如果字符串数字中有小数点，[parseInt]()会==省略小数点后的数字==，[parseFloat]()和[Nmber]()会==保留小数点==

- ++a和a++都会改变==原值==，**但是**==++a/a++==跟a是==两个东西==，==a++输出==的是`原值a`，==++a输出==的是`自增后的值（新值）`

  - ```js
    Tips：let a = 20 
    	  a = a++   因为相当于a先做了一次++运算，a = 21，然后又进行了一次 a = a++(值20)
    	  console.log(a)//20
    ```

- [let c = a||b或a&&b]()：这种用法是因为对于==非布尔值==进行与或非运算时，会[先转换为布尔值]()，然后再运算，[最后返回原值]()，

  - ```js
    let result = 5 && 6 //result=6 如果两个值都为true则返回后面的值
    let result = 2 && 0 //result=0 如果有一个值为false，则返回**第一个**遇到的false值
    let result = 0 && 2 //result=0
    let result = NaN && 0 //result=NaN
    let result = 2 || NaN //result=2
    Tips:js中与或都是短路运算，与是查找false，遇到false就不再进行后面的运算，或运算是查找true，遇到第一个true就不再进行后面的运算
    ```

- [Unicode编码]()：用处很多！用`\u2620`等编码形式可以表示==中文==、==图标(element UI的小图标就是编码)==

  - Tips：
    - 在==JS==中`\u2620`是==十六进制==表示
    - 在==HTML标签==中是==十进制==表示，`<span>&#9760;`(注意&#开头)。通过计算器计算
    - 在==CSS==中也是==十六进制==，但是不需要==u==，`\2620`

- [进制转换]()：

  - 使用[toString(参数)]()方法，里面参数传入几，就是转换到几进制，例如`num.toString(2)`

    Tips:

    - toString转成的==字符串==，不能再使用toString

  - [parseInt(字符串/数字，radix)]()

    Tips:

    - 只能转换第一个字符是数字的字符串
    - 第二个参数只能传==2~36==，不传则根据规则解析为不同进制的==整数==。==0X==开头的解析为十六进制；==0==开头的解析为八进制或十六进制；==1~9==开头的解析为十进制；==只有==传入==2==，才能按==二进制==去转换为==十进制==

  - [toString(radix)]()：只能传入==2~36·==的基数

    Tips:

    - 传入2，数字以二进制显示；传入8，以八进制；16是十六进制

- [位运算符]():

  - [十进制数 >> 位移个数]()：向右位移。低位==舍弃==，十进制数如果为正则左边高位补零，为负则补1
  - [<<]()：向左位移。高位舍弃，低位==补零==
  - [>>>]()和[<<<]()：无符号的右移和左移。无符号指==右移==时，不再考虑是正数还是负数，一律高位补零



## 条件语句

- [switch]()：首先会执行==case条件判断==，再执行==之后所有代码==，没有break、return，则会把之后所有case内的代码执行

  - Tips：

    - ```javascript
      case a:         等同于    if(xxx===a||xxx===b){
      case b:	                     /*代码块*/
          /*代码块*/            }
      break;
      ```

    - ```javascript
      switch(true){           小技巧：switch进行范围判断，通过switch传入true值与case中的条件进行判断
          case xxx > 60:
              break;
      }
      ```

## 循环语句

- [while]()：需要三个步骤：
  1. 创建一个初始变量`let a = dom.parentNode`
  2. 在循环里设置条件`while(a!=null)`
  3. ==循环体内==定义一个更新表达式，更新==初始变量==，`{a = a.parentNode}`

- [do...while]()：跟while的区别是，do...while先执行代码块==再==进行判断、while==先==判断再执行

- 取==质数==的一个小技巧：

  ```js
  let flag = true
  for(let i=2;i<num;i++){
      if(num%i==0){
          flag = false
          break;善用break等跳过重复不需要的循环从而大大节约时间
      }
  }
  if(flag){
      alert('是质数')
  }else{
      alert('不是质数')
  }
  ```

- [break]()：终止离他==最近的==一个循环。也可以使用

  ```js
  outer:
  for(let i=0;i<2;i++){
      console.log('外层')
      for(let j=0;j<4;j++){
          break; 此时break 终止的是外层outer标记的循环
          console.log('内层')
      }
  }
  ```

- [continue]()：==跳过==最近的一个循环，也可以使用==outer==标记外层

## 防抖、节流

- [防抖]()：
  - 只能获取到==定时**执行前**最新的值==！因为每次都==清除旧==定时器，设置==新==定时器，新定时器里的闭包值都是新值
  - 使用“underscore”插件，使用时在“**created**”函数中用变量承接“**_.debounce(执行函数，防抖时间)**”函数，再在watch等需要使用的地方调用这个**变量**
  - Tips:
    - 想要给**防抖**执行的方法中**传入参数**，可以曲线救国，将参数**传入防抖**函数，**再**在防抖函数中**传给里面**的方法
  - 核心在于“**闭包**”，用函数包裹函数的形式可以形成闭包，就能形成作用域链，即使重新调用生成了另一个计时器，也能通过闭包的作用域链找到之前函数的变量(定时器)，并清除。
  - 思路：频繁调用的场景下，每次都设一个定时器，当小于定时再次调用时，会根据作用域链找到之前的定时任务，并清除同时再设一个新的定时任务，从而达到刷新计时的效果，当大于定时再调用，之前的定时任务已经执行完毕，变量重新回归null
  

![](https://upload-images.jianshu.io/upload_images/6322775-5b5f7fcbc417ab58.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

<center>函数防抖

![](https://upload-images.jianshu.io/upload_images/6322775-42930d48acd99bc6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

<center>防抖简化版，因为核心理念就是清除、定时、清除、定时，如果超过时间，则定时任务完成后“timer”自动回归为null

- [节流]()：每隔一段时间执行一次
  
  - 可以获取到==新旧值==，可用于==延迟执行==回调方法，因为==不清除==定时器
  - 思路：利用闭包记录下每次调用时的时间，并计算当前时间和上一次时间的差值大于固定值时就执行，否则跳过，并继续叠加时间，直到差值大于固定时间
  

![img](https://upload-images.jianshu.io/upload_images/6322775-967503d0ac497b27.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

<center>函数节流（在初次调用时执行，滑动停止时立即停止）

- 上面这个实现思路有个问题，使用的Date记录的是系统时间，即上一次停止调用时开始累计，当时隔很久再次调用时会**立即触发**而**不是重新**计算从滑动开始到结束的时间段，所以使用**标识符**改进

![img](https://upload-images.jianshu.io/upload_images/6322775-f9fa0cfa41e6b00b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

<center>每次重新开始计算时间，延后停止

- ==注意！==
  - 以上网络截图均是`dom.事件 = 防抖/节流(fn,delay)`形式绑定的事件方法，此时外部函数已经执行过一次，以==内部函数==作为返回值，之后再触发事件，使用的就是==闭包==，里面的`flag`、`timer`就变成了一直会延续下去的局部变量，==因此才能一直以一个变量为判别是否执行的标准==
  - 使用`<标签 事件="防抖/节流(fn,delay" />`这种形式是==无法使用闭包==的！因为每次调用都会创建新的闭包而不是使用闭包
  - 另一种实现思路是创建==全局变量==，将==闭包==函数提出来单独使用

## 作用域、闭包、this

- [var关键字]()声明的变量会在所有代码执行之前==声明(但**不会赋值！**)==

  -  `a() var a = function(){ }`不会提前创建函数，因为未赋值

- [function关键字]()声明的函数会在所有代码前被创建

- ```js
  let a = 1
  function fun(){
      let a = 2
      console.log(a) 会输出2 因为优先在自身局部作用域下查找操作的变量 有就使用 没有向上一级查找
  }
  ```

- this出现的地方**一定**是在==函数中==，指向的也==一定是一个对象==，由它的==调用者==决定。如果没有==指定对象==去调用，就是==**window**==，例如[自执行函数]()、`function xx(){ function xx2(){ this指向window }; xx2() 因为xx2这里没有指定调用者 } xx()`

  - TIps：
    - this找最近的函数`{}`，这个函数**被谁调用就指向谁**，

    - **但是**函数在哪创建==作用域==就在哪，**不会**随着调用者而改变(即对象内调用一个方法，也是去声明函数的地方找它的作用域，找不到的变量就去作用域上级查找，很有可能就是去找全局作用域)

    - 对象内的方法是箭头函数，则箭头函数内的this指向对象父级的调用者。使用var _this = this绑定可以解决

    - 使用`let obj = new 构造函数()`创建的对象`obj`就是this指向的对象
    
    - ==事件绑定==给谁，this就是谁，比如`element.onclick = function(){ this }`，此处this指向`element`

- 不同函数的this区别如下：

  - 普通函数：调用时被决定。
    - 谁调用我，我的this就指向谁
    - js中this是根据函数的**执行环境**动态绑定的：全局函数中this指向的是window；当函数作为某个对象的方法被调用时，this就等于那个对象
  - 箭头函数：**定义时**被决定
    
    - 箭头函数==没有this==，是从==父级作用域继承==而来
    
    - 箭头函数的this是==父级==**函数**的this==，**没有则指向window**，==**对象是没有this的！只有函数才有this！**如果箭头函数的上一级是==对象==则==继续往上级找==
    
      ```js
      例如	
      let a = {
          b:{
              c:{
                  d:()=>{
                      console.dir(this) 此处this指向window 因为父级c是对象没有this
                  }
              },
              e:function (){
                  let fun = ()=>{
                      console.dir(this) 此处this指向b 因为父级e是函数，e的this指向b
                  }
                  fun()
              }
          }
      }
      function P(){
          this.f = ()=>{
              console.dir(this) 箭头函数的父级是 构造函数P 而不是 调用者this
          }
      }
      let a = new P()
      深刻理解父级作用域
      let t = {
          a:123,
          b(){
              return {
                  fn(){
                      console.log(this) this指向return的对象{fn(),fn2:()=>{}}
                  },
                  fn2:()=>{
                      console.log(this) this指向对象t 因为箭头函数没有this 是从父级作用域继承的
                  }
              }
          },
          c() {
              return {
                  fn3() {
                      return {
                          fn4: () => {
                              console.log('fn4', this) this指向{fn3()} 对象不属于*作用域*！
                              						因此往上一级作用域找就是 fn3(){作用域}
                              						而fn3的调用者就是 {fn3()}
                          }
                      }
                  }
              }
          }
      }
  - 匿名函数：this是静态的，始终指向==声明时所在作用域==
    -  **多数情况**下，是由`windows方法中调用`，因此this指向window；此处解释一下为什么是window，因为不同函数方法或者全局定义的匿名函数，是由**JS引擎**各个模块分别**管理**，从**一开始**就已经定义好了this指向

- **this指向**一句话攻略：==谁调用==指向谁，没有指定调用者(==自执行==函数等)就是==window==，特殊情况——箭头函数是==父级的this==，父级是==对象==就==没有this==，指向window

- [作用域]()：

  - 在==声明时==已经==确定==！即函数体即使传入另一个函数作用域内执行，也是看执行的函数==声明处==所处的==作用域==区域

    - ```js
      let x = 10
      function xx(){
          console.log(x)
      }
      function xx2(fn){
          let x = 'aaa'
          fn()
      }
      xx2(xx) // 输出10
      ```

- [闭包]()：

  - 产生条件(条件)：1、==函数嵌套==；2、==内部==函数==引用==了==外部变量==

  - 何时产生(时间)？==外部==函数执行==返回==内部函数作为结果；之后重复调用返回的函数，其内部引用的变量会在==闭包==中一直叠加传递下去

  - 作用：延长==局部变量==生命，在==函数外==可以操作读写==内部数据==。用人话讲其实就是，==保持内部函数里的变量的唯一性==，用==全局变量==替代闭包是一样的

  - 死亡：如`let f = fn() f = null`，当存储闭包的变量变成垃圾对象时

  - 应用：

    ```js
    1、JS模块
    	function module(){
            let a = 2
            function add(){
                console.log(++a)
            }
            function sub(){
                console.log(--a)
            }    
            return {add,sub}
        }
    	let m = module()		可以看到此处两个闭包函数操作的是同一个变量a，先加后减
        m.add() // 3
    	m.sub() // 2
    2、不使用脚手架引入**模块**原理
    引入模块文件中 往window上添加管理对象
    	(function (){
        	let a = 2
            function add(){
                console.log(++a)
            }
            function sub(){
                console.log(--a)
            }    
            window.module = {add,sub}
        	想往window上添加方法，直接用 window.module = add
    	})()
    执行文件中 直接调用模块对象里的方法
    	module.add()
    ```
  
  - Tips：
  
    - for循环在闭包的的差异性
  
      ```js
      // 基础for循环，每次循环都是 单独作用域 包括索引index
      for(let i = 0;i < 3;i++){
          setTimeout(()=>{
              console.log(i) // 0 1 2
          },100)
      }
      // for循环外声明的变量即父级作用域的变量，在异步函数中是唯一的，即指向的内存地址只有一处
      // 因此会叠加传递
      let index = 0
      for(let val of array){
          setTimeout(()=>{
              console.log(index) // 3 3 3
          },100)
          index++
      }

## 自执行函数

- `()`表示函数的执行，而函数必须是一个==整体==，所以需要用`(function (){})`的形式将整个函数包起来，再用`(function (){})()`去执行函数

- 就是全局函数，this指向window；只有通过object或者say函数调用；

- 函数**声明式**写法**function foo(){/\*...\*/}**，会造成**函数提升**，**不论在何处声明都可以调用，但只有在调用的时候才启用**

- 函数**表达式**写法**var foo=function(){/\*...\*/}**，**不会**造成**函数提升**，所以**必须先声明再调用**；因为在声明foo变量的时候，会**连带执行**后面的**function，**所以**表达式**写法**会立即执行**。

- 函数提升：在预编译阶段将函数声明提升到**对应**作用域最顶端

- 自执行函数作用是创建独立作用域，外面访问不到，避免变量污染

- [自执行函数](https://www.cnblogs.com/jdWu-d/p/11587805.html)

- 不管函数，对象里面如何定义，只有在它**执行的时候**，谁**直接**调用，**this**就指向谁；

- **回调函数不在作用域内**，因为回调函数只是所**声明**函数**的实例**，**本体**还**在外**面，所以不在作用域内，得通过参数传递才能使用

- 当函数只用一次，**声明后立即调用**的情况下使用

- > (function ( ) {
  >
  > ​    statement
  >
  > }) ( );//因为函数在这只是定义，还未调用，再加一个括号就是执行
  >

- `(function (){})` 是一个 ==函数==对象 通过在后面的==括号==进行==传参== `(function (形参1){})(实参1,...)`
  
  - 后面的括号是==执行==函数==时==传入的==实参==，`function ( )`的括号是用于接收==形参==

## 构造函数和工厂函数

- ```js
  #工厂函数     用于批量生产**同类型**对象 都是由Object生成
  function person(name,age){
      let obj ={}
      obj.name = name
      obj.age = age
      return obj
  }
  let newObj = person('xxx',18)
  console.log(newObj.constructor) 等于Object
  console.log(newObj instanceof Object) true
  console.log(newObj instanceof person) false
  ```

- ```js
  #构造函数     声明方式和普通函数相同 但是使用时用**new关键词**创建**实例对象** 由构造函数生成
  function Person(name,age){
      this.name = name   此处this指向new**创建的对象**
      this.age = age
  }
  let newObj = new Person('xxx',18)
  console.log(newObj.constructor) 等于Person
  console.log(newObj instanceof Object) true
  console.log(newObj instanceof Person) true
  ```

- 本质上，==构造函数==也是普通函数，只有当用==new关键字==调用时才是构造函数，虽然可以用普通函数的方式去调用，但没有意义

- 函数也是==对象==，`function xxx(){}`在栈中是以`xxx：0x123`形式存储，指向==堆==中的==Function对象==

- ```js
  function fn1(){                                  function fn2(){
      let t = new Date()							     let t = 2
      return t										 return t
  }												 }
  let t = new fn1() 此处t为返回的Date实例对象		   let t = new fn2() 此处t为空对象！
  结论：new一个有返回值的普通函数 在返回值是基础数据类型时 new出来的实例是空对象！

## 原型对象、原型链

- [原型链]()：

  - `函数.prototype(object空对象).__proto__(object原型对象).__proto__(null)`

  - `实例对象.__proto__(object空对象).__proto__(object原型对象).__proto__(null)`

  - `Object创建的对象.__proto__(原型对象).__proto__(null)`

  - `函数.__proto__(Function对象).__proto__(object原型对象).__proto__(null)`

  - `Function.__proto__(Function对象).__proto__(object原型对象)`

  - 从上可以看出：

    - 实例对象是实例对象，函数对象是函数对象，实例对象的`_proto__`并不是函数对象，而是object空对象，里面有个属性==constructor==，指向实例对象的构造函数

      Tips：

      - `构造函数.prototype`和`实例对象.__proto__`是一个东西，指向同一个object空对象
      - `let aaa = 'text' aaa.__proto__`和`Vue.prototype`并不是一个东西

- [原型链继承]()

  - 即将一个构造函数的==原型重新指向==另一个构造函数的==实例==，`子函数.prototype = new 父()`

  - ==构造函数==及其==实例==也用到了原型链==继承==，他们的原型链比==Object==及实例对象的原型链==多一层==

  - **注意**，因为==子的原型==重新指向了父的==实例==，而实例和构造函数里是没有==constructor==属性的，又因为子的原型是实例，再去==原型==实例==的原型==去找，所以==子的constructor指向父的构造函数==

    TIps：

    - 因为==constructor==指向了改变了，所以就需要重新指向。`子函数.prototype.constructor = 子函数`，因为子函数的实例找`constructor`是从原型链上，因此需要给==父函数实例==身上加个同名属性

- 访问一个对象时，会==先==在==自身==寻找，如果没有，则会去==原型对象==寻找

- 当创建[构造函数]()时，可以将==实例身上共有==的属性和方法添加到==构造函数==的原型对象中，这样既可以每个对象都取到，也不会==影响全局作用域==

- ==构造函数==之上还有==Object==原型对象，原型链就到头了

- ==函数==才用`func.prototype`，==对象==用的是`obj.__proto__`

  - 在`let vm = new p()`时，其实内部执行了一次`this.__proto__ = P.prototype`，所以对象身上的隐式原型指跟构造函数原型指向同处

- [原型对象]()是==一个类别==对象/函数的==公共区域==，用于存放==公用==的东西

  Tips：

  - 原型对象都有[constructor]()
  - `构造函数.prototype` = 原型对象，`原型对象.constructor` = 构造函数

  ```js
  function xx(){
      this.a = 1
  }
  let a = new xx()
  console.log(xx.prototype.constructor == xx) //true
  console.log(a.__proto__.constructor == xx) //true
  console.log(a.__proto__ == xx.prototype) //true
  ```

  - **只有**==读取属性==时才会在**原型链**中查找，当给实例==设置属性==时，是给当前实例对象上设置属性

- ==实例对象==和==函数==的原型都指向==**空的Object实例**对象==，==Object空对象==的原型指向==Object原型对象==，注意，到原型对象之间都隔了一次==空对象==

  Tips：

  - 在`function XX(){}`内部隐式加入了`xx(this).prototype = {}`，在`let aa = new xx()`时则是隐式`aa(this).__proto__ = xx.prototype`(这步是在==创建==实例对象时做的，后面不会再将新的显示原型地址值赋给实例的隐式原型)
  - 虽然`xx.prototype`是一个空对象，**但是**`xx.prototype`和`xx.prototype.函数.prototype`并不是同一个空对象

- `function xxx()`等于`let xxx = new Function()`，而又有`Function = new Function()`。所以，所有==函数==都是==Function==的实例对象，因此，函数同时具有`__proto__`和`prototype`，且他们的隐式原型都指向都相同

- ```js
  函数的原型指向对象默认是空Object实例 但Object不满足 因为Object原型对象是原型链尽头
  函数.prototype instanceof Object //true
  Object.prototype instanceof Object //false
  Function.prototype instanceof Object //true
  ```

- [instanceof]()原理：`A instanceof B`，B的显示原型对象在A的原型链上(原型链有交叉)，则返回true，否则返回false

## JS原生方法：

### Array方法：

- `push() `数组末尾**添加**元素，并**返回新**的**数组长度**

  - 可以传入多个元素，在数组中的顺序以传入的顺序排列

- `pop()`**删除**数组最后一个元素，并**返回被删**的**元素**。==改变原数组==

- `unshift()` 数组头部**添加**元素。==改变原数组==

- `shift()`数组头部**删除**第**一个**元素，无参数。==改变原数组==

- `splice(起始位置，删除个数，一个或多个元素)`

  - 从起始位置开始(**包含**)，删除指定个数的元素。并在起始位置==前==添加元素，并返回**被删元素**组成的**数组**（注：==会改变原数组==）


  - “删除个数”不写，**默认**是**删除**起始位置之后的**所有**元素、如果“删除个数”写0，一个都不删除，插入的元素在**起始位置前**

  - ==索引从0开始==

- `slice(start,end)`

  - 截取==包括==开始到==不包含==结束的数组元素并作为一个==新数组==返回。
  - ==不改变原数组==

  - ==start、end==超出数组长度不会报错，只会截取有的部分

- `join(可选：'符号')`

  - 将**数组**合并为一个**字符串**返回，参数可传自定义符号，在字符串中用自定义符号拼接元素


  - 不传入参数，默认用==逗号==分隔！最好是传入==空字符串==

- `reverse`：反转数组中元素顺序，会==改变原数组==

  - 字符串虽然可以像数组那样取值，但是并没有[reverse]()方法。可以通过字符串的[split('')]()切分，转换成数组

- `some((当前元素的值) => { return 条件 } )`

  - 检查数组中是否有某元素，返回的是boolean值

- `filter( (当前元素的值) => { return 条件} )`过滤器

  - 返回指定条件的元素组成的==数组==，不会改变原数组
  - ==当前值==是Arry中每一个元素，因为filter其实执行了一次遍历，将Arry中每一个元素拿出来跟return中的判断条件进行比对，再将符合条件的元素合成一个新数组
  - 例如：`数组.filter((数组元素) => { return 数组元素.indexOf(变量) != -1 })`

- `find(当前值)=>{ return 条件 }`返回符合条件的==元素==。

  - 跟`filter`很像，但filter返回的是==数组==，**而**`find`返回的是==数组元素==

- `list.includes(内容)`(ES7)

  - 检测数组中是否包含指定内容。返回==true/false==

- `forEach( (当前值，索引)=>{ } )`

  - foreach中回调的函数是数组中每一个元素都调用一次

- `reduce( (前一次返回值，当前数组元素内容)=>{ return 统计 }，初始值 )`

- `for(let i of array)`遍历数组

- `indexOf('检索值'，'起始位置')`

  - 检索数组中是否有**指定元素**

  - 只检索元素中部分内容是不行的

  - 元素类型不对也是不行的

- `Object.keys(数组)`返回对象**键**组成的数组

- `sort(function(a，b){a-b或b-a})`

  - 回调函数遵循几个个规则：(==改变原数组==)
    - a在数组中的位置==必定==在b==前面==

    - 返回值==大于==0，元素==交换==位置
    - 返回值==小于==0，位置==不变==
    - 返回值==等于==0，认为元素相等，位置==不变==
    - 因此[a-b]()大于0则a，b交换位置，是==升序==；而[b-a]()，后面的元素减前面的元素，大于0则b，a交换位置，是==降序==

- `concat(source，...)`

  - 拼接多个数组及元素
  - ==不改变原数组==



###   string方法：

- ==字符串==在底层是以==数组的形式==保存，所以可以使用大部分数组的方法 

- `indexOf('检索值'，'起始位置')`

  - 检索字符串中是否有**指定内容**，有则返回**位置索引**，没有则返回**-1**；

  - 注意，检索空字符串`''`时，返回的值是`0`，因为所有字符串在开头都拥有空字符串(**也可以用于数组**)，==但不能使用正则去匹配==

  - ==区分大小写==

- `string.match(正则表达式)`

  - 匹配内容**包括**==括号内子匹配==内容，以及==整体匹配==结果，以**数组形式**返回

    ```js
    let t = 'data:image/png;base64'
    t.match(/:(.*?);/) // 输出 [':image/png;', 'image/png', index:4,...]
  - `\`是==转义字符==，用`"我说：\"今天天气不错\" "`输出出来就是`我说："今天天气不错"`

- `string.matchAll(正则表达式)`(ES11)

  - 所匹配==正则==**必须**是==全局==匹配！即必须要有`/.../g`

  - 返回==整体匹配==结果，**以及**括号内==子匹配内容==，以==可迭代对象==形式返回

    ```js
    let t = `
    <li>
    	<a>阿甘正传</a>
    	<p>2022-10-1</p>
    </li>
    <li>
    	<a>阿甘正传2</a>
    	<p>2011-10-1</p>
    </li>
    `
    let r = /<a>(?<name>.*?)<\/a>.*?<p>(?<time>.*?)<\/p>/gs  全局匹配、包括换行、()捕获、?<>分组命名
    let result = t.matchAll(r)
    for (let val of result) {
        console.log(val)	输出匹配结果数组
    }

- `replace(string，new string)`

  - 用新字符串替换指定字符，或者用新字符串替换==正则匹配==的字符。该方法==不会改变原字符串==


  - 只会替换==第一个匹配==的字符

  - 要替换全部匹配字符，需要用`replaceAll`或者※`replace(/正则/全局匹配，"替换内容")`

  - `字符串`中见到`\n`就是换行

  - 返回==修改后==的字符串

- `substring(start，end)`

  - 截取从==开始索引(包含)==到==结束索引(不包含)==的字符串

- `slice`

  - 同数组


  - 切割下来的是==字符串==而不是数组

- `split('分隔符'/正则表达式)`

  - 以什么分隔就传入对应的字符或者正则对象，会以这个字符或规则截断原字符串，组成==数组==，数组中不会有作为分隔的字符


  - 如果传入空字符串`""`，则拆分每个字符
  - `split("a")`或`split(/[A-z]/)`

- `string.charCodeAt(index)`返回字符串索引位置的字符的==Unicode码==

### object方法：

- [Object.keys(对象)]()：返回对象**键**组成的==数组==
- [Object.values(对象)]()(es8)：返回对象**值**组成的==数组==
- [Object.entries()]()：通常对象是不可直接遍历的，而通过`entres`可以返回一个·键值对·组成的数组(**二维数组，第一层是有多少个键值对，第二层是键和值组成的数组**)，从而可以在es6中的新for循环·**for(let i of 可迭代变量)**·中使用，使用方式·for(let **[key,value]** of Object.entres())·，·[key,value]·是es6新提出的·**解构赋值**·
  
  Tips：
  
  - ·**Array**·也可以使用，只不过是以数组索引作为·key·
  - 如果对象下有对象，得到的结果就只是第一层的，对象下的对象在entries后，在数组中还是对象
- [Object.assign(目标对象，源头对象)]()(es6)：用于对象合并，合并同类赋值，不同类的作为新增，返回值是·**新的目标对象**·。但也可以用·**扩展运算符**·更简洁
  
  Tips：
  
  - 在**vue**中，提前准备好的属性在合并后依然具有响应性
  - 有响应式的对象和无响应式对象合并，没有响应式的依然没有响应式
- [for ( let key in object )]()：遍历对象==和数组！==

  - 与[for(let value of array)]()不同的是，遍历的是key，而`for of`遍历的是值
  - ==注意！==此方法会遍历出==原型链==上的属性！
  - 遍历数组时，==key均为字符串！==

- [delete  obj.xxx]()：操作符，等同与或符。返回删除成功与否
- [hasOwnProperty]()：使用`xxx in obj`会寻找obj==原型链==上的属性方法，而==hasOwnProperty==只会查找`obj`身上的属性方法

###   window方法：

- window方法可以省略`window`关键字，直接写后面的方法
- 弹窗：警告窗[alert('xxx')]()；确认框[confirm('显示内容')]()，==返回布尔值==
  - [prompt(string)]()：类似==alert()==，传入一个==字符串==作为提示文字，会弹出一个输入框，输入的内容作为`prompt函数`的返回值
- **history**.pushState/**replaceState**(state,title,url)可以在**不跳转页面**的情况下**改写**当前页面的url
- **跳转页面**：window.location.**href** = "地址?键值对"，跳转页面后使用`**location.search**`获取到`？`之后的内容，对获取的内容进行字符串分割，创建对象obj = { }，使用**obj['key'] = '值'**，将分割好的键值对装入对象
- [sessionStroage]()：会话存储
  - 使用`sessionStroage.自定义变量名 = 值`，就可以将**值**存入**自定义变量**名，取用的时候直接用**sessionStroage.自定义变量名**就可以获取值
  - ==删除==的时候使用`sessionStroage.removeItem("自定义变量名")`
- [localStorage]()：本地存储
  - 保存：`localStorage.setItem('key', 'value')`
  - 读取：`localStorage.getItem('key')`
  - 删除：`localStorage.removeItem('key')`
  - 与`sessionStroage`相同点
    - 都要求协议、IP、端口一致才能读写

  - 与`sessionStroage`不同点
    - `sessionStroage`还要求在==同一浏览器标签页下==
    - 生存周期不同：`localStorage`只要不主动清除不会消失，`sessionStroage`关闭浏览器**标签页**就会消失



### document方法：

- [cookie]()：
  - ==设置cookie==：`document.cookie = "user = xx"`
  - **默认**情况下浏览器关闭就清除cookie
    - ==添加保存时间==： `document.cookie = "user=xx;expires=Thu, 18 Dec 2043 12:00:00 GMT(此处时间是Date对象的toString)"`，通过Date对象的[setDate]()方法可以**设置**当月中的**某一天**，用getDate()**获取当天**日期
    - ==删除cookie==的原理：将expires 参数设置为当前时间或者前几天即可
  - ==同名==的cookie会被覆盖，==新==的cookie不会覆盖，会加到string尾部
  - ==读取cookie==：`cookie.match(reg)`


- [session、cookie、token]()三者区别：

  - cookie是载体，不管是session还是token都是通过cookie传递；浏览器访问页面是会自动带上cookie，因为在第一次http请求时，服务器会返回一个“set-cookie”，告诉浏览器要保存下来，并且访问时携带“cookie”；根据不同网站保存不同的cookie
  - session由服务器端生成，存储在服务器端，访问时是通过把用户名密码发送至客户端进行验证，通过后**服务器**生成sessionID并设置结束时间，再将sessionID通过cookie发送至浏览器，并将**结束时间**设置为cookie**有效期**
  - token由服务器端生成，通过cookie发送到客户端，由客户端保存，可以放在cookie或者storage；
  - session的缺点就是如果服务器崩溃，所有用户都得重新登陆，因此，就有人想出了不用存储在服务器，而是每次访问都进行验证，通过服务器加密算法生成token发送给客户端进行保存，减少了服务器压力，用户再次访问时，发送token到服务器，服务器再通过加密算法和密匙计算一次token并进行对比，就可以知道是否是之前登陆过的用户


- [execCommand('copy')]()：缺点是只能复制选中的内容。但可以通过创建一个临时输入框，调用select方法，达到复制的目的

  ```js
  let temp = document.createElement('textarea')只能用textarea
  temp.innerText = 'xxx'
  document.body.appendChild(temp)
  temp.select()
  document.execCommand('copy')
  document.body.removeChild(temp)
  ```

- [推荐navigator.clipboard.writeText(’内容‘)]()：直接将传入参数复制

### function方法：

- 当以`func()`调用时，==this==指向==调用者==，例如：`a.b.func()`，this指向`b`

- 当以`new func()`调用时，==this==指向==新创建的对象==

- [call(obj，params1，params2，...)]()：传入的第一个参数是==this的指向==，后面的参数是通常函数调用时传入的==实参==

  - `obj.fnc.call()`改变指向的同时就已经执行了

  - **注意**call==无法改变箭头函数==的this，因为箭头函数的this是父级函数的this，**但是**可以==父级函数调用call改变this后再调用箭头函数==，这时箭头函数的this就跟着改变了

    ```js
    let t = {
        fn1(){
            return () => {
                console.log(this.aaa)
            }
        },
        aaa: 123
    }
    let t2 = {aaa:'qwe'}
    t.fn1()() //123
    t.fn1.call(t2)() //'qwe'

- [apply(obj，[...args] )]()：同[call]()，但第二个参数是==实参==组成的==数组==，==传入==函数时已经被==展开==，并==依次==赋值给形参

  Tips：

  - [func.call(obj)]()和[func.apply(obj)]()等同于`obj.func()`，只不过是临时的，让`func()`成为`obj`的方法调用
  - **注意！**等同于只是说相当于这样用，但不能真的写成`obj.func()`！因为obj里没有`func`方法，`.call`只是临时的！

## js对象(数组)

- 对象细节问题

  - 什么是对象？
    - ==描述==一个==事物==的多个数据==集合==

  - 为什么需要对象？
    - 管理数据。比如，在一个对象下有多个数据，都是描述这个对象的

  - 对象的组成
    - 属性：属性名(==字符串==)和属性值(==任意==)组成
    - 对象的==方法==描述这个对象可以==做什么==。==属性值==是函数
  - `object.obj2`是对象吗？
    - 不，只是obj2的==地址值==，只有加`.`才是去对象中取值

- 只要是==函数==，加==（）==就可以执行，哪怕是==变量==形式

- 区分==对象==和==数组==

  - `xxx.constructor === Array/Object`

  - ``xxx instanceof Array/Object`

  - `Array.isArray(xxx) //true或false`


  Tips：

  - [typeof]()是区分==基本==类型和==引用(对象)==类型的，返回的是==字符串==，注意全是==小写==；

    - 可以判断==undefined==、==字符串/数字/布尔==、==函数==，==不能==判断==null==和==对象/数组==。简记：可以判断==除了null、Object、Array==的所有

  - [instanceof]()是区分==对象的具体==类型(不靠谱，因为所有都可以判断为Object)，==不能==判断==null==

    - ```js
      console.log(对象 instanceof Object) true
      console.log(对象 instanceof Array) false
      console.log(数组 instanceof Object) true 因为数组是Array构造函数的实例，而Array又是Object原型对象的构造函数
      console.log(数组 instanceof Array) true
      console.log(函数 instanceof Object) true
      console.log(函数 instanceof Function) true
      ```

  - [===]()相对万能，可以用`null === null`判断null以及undefined，因为null和undefined类型的数据只有一个值

- [对象的两种取值/定义方式]()：`obj.key `和 `obj["key"]`，但是如果key是一个变量（传进来的参数），就只能用obj[key]（注意少了引号）

  - ```js
    obj = {
        ["key"]: value
    }
    因为“属性名”本质上————就是一字符串
    但是不能传"aa.bb"这种字符串
    
  - `array[字符串]`时字符串会转为数字，对于不能转为数字的才会报错

    - 内部转换数字用的是==Number==函数

- [删除对象中的一个属性]()：`delete obj.name`，但是不论怎样，都得取得属性==父级==才能增删改查

- [in运算符]()：用于检查一个对象中是否有指定的属性。`'属性名' in obj`，会返回一个true和false

- ==变量==和==值==是存在[栈内存]()中，而引用类型数据是存在[堆内存]()中，，当变量保存的是一个引用类型数据时，栈中是以`key：value`的形式保存着引用类型数据在==堆空间==中的==物理地址==

- [function函数也是对象]()：[不加()]()，作为实参时传入的是==函数对象==，不会执行；[加()]()，作为实参时则==先执行==再传入执行后的==结果==

  - Tips：

    - ```js
      匿名函数：
      let a = function (){ }             function(){ /*代码块*/ } 这样写会有问题 没有函数名会当成两个独立的部分
      a(params)                          正确的匿名函数写法 (function(){ }) 这样才会当成一个整体
      ```

- [对象中定义函数方法]()

  - `key:function(){ }`

  - `key:()=>{ }`

  - `key(){}`

  - 调用的时候则是`obj.key()`

- 数组和对象本质上是一样的，只不过因为存储方式不同，数组比对象效能高，所以对象可以`obj.xxx = ''`进行赋值，数组也可以给==不存在的索引==添加元素

  Tips：

  - 建议用`array[length] = ''`的形式而不是`array[index]`直接去添加，因为未定义索引和已知索引之间的元素会用`null`填充
  - ※数组[length]()是==可以被修改==的，当length==大于原长度==，则会用==null==填充，当length==小于原长度==，则==删除==多出的元素(用这个方法来删除多出的元素)，==但是Vue==无法通过这种方式检测到数据修改
  - [字符串]()和数组==存储方式==是一样的，都是通过==索引==来存储一个个字符(元素)

- ※[对象赋值=]()：输出的还是`{age:12} `，因为函数调用时，是将==实参的内容==(对象的话，内容是地址)==赋值形参==，此时在函数中又给==形参==赋值了一次==新的对象地址==，==形参==就跟==原先的对象断开了联系==，所以不会影响到实参。注！且因为==函数执行完毕==时，函数中的==局部变量==会从内存中==自动释放==，所以==函数==中赋值==新的对象是垃圾数据==，函数执行完就找不到了

  ```js
  let a = { age: 12 }
  function xxx(obj){
      obj = { name: 'Tom' }
  }
  xxx(a)
  console.log(a)
  有所区别的是：
  let a = b a、b都是“变量”
  func(实参) 所谓“实参”就是变量的“值” 不是变量本身！
  function func(形参) “形参”则是“变量” 只不过是局部的
  ```

## Date对象

- [new Date()]()：会返回代码执行时的时间，也可以传入固定格式参数`12/2/2011 11:10:30`，指定时间

  Tips:

  - new Date返回的是==对象==，可以用[toString()]()方法格式化时间，得到 "Thu Aug 11 2022 14:53:42 GMT+0800 (中国标准时间)" 这样一个字符串。再用字符串方法取出想要的内容
  - 通过HTML元素获取的日期其实也是==Date对象==，因此不需要格式化时间，用对象方法即可获取想要的参数
  - **※** 可以接收参数，`new Date(年, 月, 0)`创建一个指定年月的日期对象，再用==getDate()==获取==指定月份有多少天==
    - **注意**末尾传参==0==很关键
    - ==new Date('2023/1/10')==与==new Date(2023，0，10)==生成的日期对象是一样的
  - 可接收参数，`new Date(时间戳)`，转换为年月日==时分秒==
  - 可接收参数`2022-8-25`不用写具体时间，会转化为当天`00:00:00`
- [getDate()]()：获取当前日期==几号==
- [getDay()]()：获取当前==星期==，返回==0-6==的值，==0表示周日==

  Tips：

  - ==getDate==减去==getDay==等于所在周的周末那天
- [getMonth()]()：获取当前==月份==，返回==0-11==的值，==0表示1月==
- [getFullYear()]()：获取当前==年份==
- [setTime(时间戳)]()：==改变原Date对象==。`date.setTime(xxx)`，date就会变为时间戳对应的时间

## Math对象方法

- [Math.floor]()：向下取整(floor意为地板)

- [Math.ceil]()：向上取整(ceil意为天花板)

- [Math.round]()：四舍五入。等同于Math.floor(x+0.5)，因为能四舍五入进一位的数加上0.5一定可以进一位

- ※[Math.random()]()：生成==**包括**0~**不包括**1==的随机数

  Tips：

  - 生成==X~Y==的随机数：`Math.round( Math.random() * ( Y - X ) + X )`（思路是：0~1乘一个数扩大范围，再加一个数确保最小值，但是要保证不超限，所以乘的数等于差值）

- [Math.pow(底数，指数)]()：求幂运算
- [Math.sqrt]()：开方运算
- [Math.max/min(a，b，c，...)]()：返回最大/最小值

## 定时器

- Tips
  - 当一个函数同时执行两个不相干==动作==时，会卡顿，比如：骑自行车需要脚提供速度和手提供方向，如果将这两个动作放在一起，整个过程就会变得卡顿！
    - 方法：用[setInterval]()定时执行==动作==，通过==触发事件==设置==定时器触发条件==

  - `for循环`中，如果==索引==用`var index`声明，则索引就变成了全局变量，定时器内不会保存当前索引值，因为等执行时根据索引的地址值去找时，索引已经是最终累加的结果。
    - 但如果是`let index`声明，则每个循环中都是==独立作用域==下的`index`

  - ==清除定时器==后，==不会立即==将整个回调函数干掉，而是等全部逻辑执行完后再清除
  - 定时器的回调函数并==不是到时间就触发！==必须在主线程栈里面的==初始化代码==逻辑执行出结果后，才轮到==事件队列==(事件循环模型)里的代码执行
    - ==事件==包括：==onclick==等==监听事件==，定时器的==回调函数(事件)==

  - `setTimeout()`可以不传入时间参数，使其内部代码==异步==执行！配合页面挂载元素，在==异步==操作元素实现类似Vue`$nextTick(()=>{})`效果



## 元素在页面中的位置

- ![img](https://upload-images.jianshu.io/upload_images/6322775-d2bce26f79b3c151.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

  Tips：

  - [offsetWidth/Height]()：元素整个大小。包括宽高、==边框==

  - [clientWidth/Height]()：可视大小，有滚动条的话，会==减去滚动条大小==。包括padding、content

    Tips:

    - ==鼠标==的[clientX/Y]()也是==相对于**可见窗口(body)**==的位置，可视窗口的左上角始终是(0,0)点。想要获取相对于==页面(超出body大小)==的坐标用[pageX/Y]()

  - [scrollHeight/Width]()：滚动大小(==不是元素自身大小！是被撑大后的大小==)。包括==可视区域（clientHeight）==、滚动条大小、隐藏部分

    Tips：

    - ==判断滚动条到底==：`scrollHeight - scrollTop == clientHeight`
      - **注意**：`offsetHeight + scrollTop > scrollHeight`
    - 滚动大小实际上就等于==滚动条可滚动宽/高==加上==可视窗口大小==

- ![img](https://upload-images.jianshu.io/upload_images/6322775-0cf168f634ec329d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

offsetTop/Left是相对父级（注：滚动显示容器里，里面的每一个元素它的offset父级都是显示容器），并且会显示(※)**累计滚动隐藏部分**；

​    offset返回的是绝对值，哪怕设置的是百分比，返回的也是px；offsetHeight是自身

![img](https://upload-images.jianshu.io/upload_images/6322775-6dd4b0d7e07d70c9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

<center/>scrollHeight代表包括不可见部分的元素高度（只包含padding和content，即使是在IE盒模型下也不会计算border的值）

![img](https://upload-images.jianshu.io/upload_images/6322775-3a3c4a65808c7cb1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

<center/>scrollTop（滚动条向下滚动的距离）

![img](https://upload-images.jianshu.io/upload_images/6322775-3e30a11ad6d49ba4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

<center/>getBoundingClientRect()求元素距视窗边距

### Tips：

1. [onwheel]()：鼠标滚轮事件
   - 想横向滚动，可以用“@wheel”，此事件在**鼠标滚轮**滚动时触发，然后手动设置，事件触发一次横向滚动距离
   - wheelDelta为正则是滚轮向上，为负则向下

2. [onscroll]()：滚动条滚动事件
3. ※**offset偏移量**是依据**上一级定位元素(position:relative等)**确定的

4. ※**offsetParent**和**parentNode**的区别：
   - [offsetParent]()
     - 查找路径：有带有·**position**·属性的上一级→带有position属性的上一级。(body虽然没设置position，但是因为所有元素都脱离不了body文档流，所以总能找到body是最外层)
     - 最外层：body→null
   - [parentNode]()
     - 查找路径：·**不考虑position**·，单纯的父级元素往上查找。
     - 最外层：body→html→document→null

5. [判断滚动触底]()：
   - [scrollTop]()取值范围：[0，==( scrollHeight  - clientHeight )==]
     - ==滚动条到底==：scrollTop = scrollHeight  - clientHeight
     - ==滚动条距离底部50px以内==：(scrollHeight  - clientHeight) - scrollTop <= 50
     - ==滚动条距离底部5%以内==：scrollTop / (scrollHeight  - clientHeight) >= 0.95

## iframe标签

- 是一个==单独的视窗==

- 内部执行的任何方法都==获取不到==iframe==父级==的元素

  - **只能**通过==获取父级页面window对象==`window.parent.document`才能操作==父页面==document元素

- iframe内的元素获取==相对视窗==的位置都是以==iframe为边界==

  - 鼠标事件也取不到iframe外的边距

- 会在执行完父级的js后再执行iframe中内容

- [vw]()和[vh]()是相对于视窗的，即是说是==相对于iframe==，而不是浏览器窗口

- ==父获取子==iframe的window：[iframeElement.contentWindow]()

  - ==子获取父==的window：[window.parent]()

- 通过[window.parent.父级方法]()调用、传参到父级

  Tips：

  - ==必须在服务器环境下！==
  - 可以通过函数传参，来获取父页面元素，但这种方法前提是，父页面也是自己编写

- [window.parent.document.querySelector]()：通过==window.parent==获取到父==页面==document从而获取元素

- 不同页面间通信[postMessage]()、[onmessage]()

  - 不是==多进程==独有的方法
  - window自带==postMessage==以及==onmessage==，`window.postMessage(数据)`是给==自身==发送消息，使用`window.onmessage = function(e){e.data}`即可接收到数据


## 数据劫持/代理

- 所谓==劫持/代理==即用==另一个变量==去获取/修改==原数据==身上的属性。==注：==监听==原数据==添加代理，调用[get]()/[set]()方法可能会造成==死循环==，因为一调用get/set，就又会读取/修改，然后再次触发get/set方法

  - 监听==源对象==时，为了避免==死循环==

    - `get()`中不能使用`源对象.属性`或`源对象[key]`的方式取值，这样会触发`get()`导致死循环
    - `set()`中不用`源对象.属性 = newValue`
    - 需要中自定义函数中转一下，避免直接取值，并且通过==闭包==为==源对象==自身及每一个属性添加不会死亡的局部变量值

    ```js
    let obj = {test:111}
    function setWatcher(target,key,value){
        observe(value) // 遍历子属性
        Object.defineProperty(target,key,{
            get(){
                return value // ※注意
            },
            set(newValue){
                value = newValue // ※注意
            }
        })
    }
    function observe(obj){
        if(!obj || typeof obj !== 'object'){
            return
        }
        Object.keys(obj).forEach((key)=>{
            //这步非常关键！设置监听前必须先取值，为了闭包中创建不会回收的局部变量
            setWatcher(obj,key,obj[key])
        })
    }
    observe(obj)
    obj.test = 'hhh' // 触发set() 修改了局部变量
    console.log(obj.test) //触发get() 返回修改后的局部变量

- [object.defineProperty(对象名，'键'，{ 配置项 })]()

  - ```js
    let obj = {}
    let name = ''
    Object.defineProperty(obj,'key',{
        get(){
            return name
        },
        set(value){
            obj['key'] = value 注意 这里不是修改代理的值 所有的修改/读取操作均发生在原始数据身上 
            				    代理对象本身只是一个空壳 映射 到原始对象
        }
    })

![img](https://upload-images.jianshu.io/upload_images/6322775-b5e3b3b6e236d059.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

<center>为指定对象添加属性；并且该属性不可被遍历

- defineProperty定义的属性==默认不可被修改、检索==

- defineProperty的好处：
  1. js默认没有数据绑定，js不会帮你做这个事情，定义过的变量用变量名赋值给另一个属性后，不会随着原先的变量改变而改变。但是defineProperty中的**get函数**可以将其**绑定**
  2. get函数作用是**双向**绑定(**改原始值**会跟着变动)，**get函数**本身不会传入参数，它“return”返回的是外面定义好的变量，所以当**set函数**修改了外部定义的变量，get中**返回值**也就变了；set函数是**改对象属性**时触发，修改后**原始值**也会发生改变(原始值会发生改变是因为修改值对原始值进行了**赋值**操作，**再触发**的**get函数**)；使用set函数时必须也要有get函数，不然set修改完值后，没有对应的属性接收
  3. 数据代理：通过另一个对象来操作原先的对象属性，通过get、set函数实现

## 下载

1. `window.location.href = down_url`下载会打开一个空白页，体验不好

2. [download]()属性，以==下载文件的方式==下载href属性上的链接。必须要加，不然无法下载文件流

   - 决定下载文件的文件名，**但是**==跨域==文件下载==不会生效==！此时就需要用==get==从文件地址获取==流文件==，再下载！

3. `a.target = '_blank'`在空白页打开

4. 请求==返回类型==必须设置`responseType: "blob"`
   - 加了`responseType: "blob"`就不需要再用`new Blob([res.data])`了，因为已经是二进制流文件，只要`a.download`写好文件名称，就不会出现无格式的txt下载

5. `URL.createObjectURL(File对象/Blob对象)`将文件流转换成==a标签==可识别的==链接==形式

   - 下载完后需[URL.revokeObjectURL(url)]()释放内存
   - 与`fileReader.readAsDataURL(file)`的区别
     - `readAsDataURL`
       - 生成的是==base64==字符串
       - 返回值很长
       - JS垃圾回收机制自动清除
       - 异步方法
       - 处理==多文件==时，每个文件要对应一个`FileReader`对象
     - `createObjectURL`
       - 生成的是==对象在内存中的URL==！
       - 很短的一个==url地址==
       - 永存于当前`document`中，只能通过`revokeObjectURL`手动清除
       - 同步方法

6. 实例

   ![image-20221114112003852](C:/Users/admin/AppData/Roaming/Typora/typora-user-images/image-20221114112003852.png)

   ![image-20221114104516632](C:/Users/admin/AppData/Roaming/Typora/typora-user-images/image-20221114104516632.png)

- Tips:
  - 有时候需要从回包里获取==响应头==里的字段，但是默认暴露出来的字段有限，需要后端设置允许访问的字段。
    - 使用`res.headers['content-disposition']`取出额外文件信息
  
  - 链接可以直接访问文件的可以用`a.download = '文件名' a.href = url`下载文件
    - 浏览器可以识别并自动下载的才行，图片应该是直接打开
  
  - 本地文件==导入导出==
  
    ```js
    // 导出 存储到本地文件
    function save(){
        let obj = {}
        let str = JSON.stringify(obj)
        let blob = new Blob([str], {type:'application/json'})//将JSON转成二进制字符串
        let url = URL.createObjectURL(blob)// 必要 将Blob对象转成指向这个对象在内存中的url
        let a = document.createElement('a')// 创建a标签进行下载
        a.download = '测试.mp4'
        a.href = url
        a.target = '_blank'
        document.body.appendChild(a)
        a.click()
        document.body.removeChild(a)
        URL.revokeObjectURL(url)
    }
    // 导入 读取文件内容 获取数据 或 预览
    function read(e){
        let file = e.target.files[0]
        let reader = new FileReader()
        // 读取json
        reader.readAsBinaryString(file)//注意此方法读取到的中文字符是乱码 需要用readAsText
        reader.onload = (data)=>{
            console.log(data.target.result)
        }
        // 读取视频图片 生成base64字符串 放入<img>/<video>预览
        reader.readAsDataURL(file)
        reader.onload = ()=>{
            let video = document.createElement('video')
            video.src = data.target.result
            video.style.width = '200px'
            video.style.height = '200px'
            video.autoplay = true
            document.body.appendChild(video)
        }
    }
    ```
  
  - base64转==Blob==和==File==。关键点都在于得到字符串的Unicode码
  
    ```js
    let arr = base64.split(',')// base64中,号前时数据类型,后是base-64编码的字符串
    let str = atob(arr[1])// atob是js原生方法 用于解码base64 返回解码字符串
    let n = str.length
    let u8arr = new Unit8Array(n)// 生成 长度同字符串 元素全是0的数组
    while(n--){
        u8arr[n] = str.charCodeAt(n)// charCodeAt返回字符串指定位置Unicode码
    }
    let option = {type: arr[0].match(/:(.*?);/)[1]}
    // 转Blob
    let blob = new Blob([u8arr], option)
    // 转File对象
    let file = new File([u8arr], fileName, option)


##  JSON

- [JSON.stringfy]()：将JS值(数组、对象等)转换为JSON字符串
  - `stringify(值,null,缩进)`可以格式化JSON字符串。**注！**格式化后的值必须放入==<pre/>==标签

- [JSON.parse]()：将JSON字符串转换为JS，==不改变原始数据==
- JSON允许传递值包括：字符串、数字、布尔值、null、普通对象、数组。不允许传递方法及函数对象，这是js独有的

## 浅拷贝和深拷贝：

-  [浅拷贝]()：只复制对方的引用地址，并不会开辟新的内存空间，修改复制值原值也会变。包括：引用类型的**赋值**、

  Tips：

  - 当·**扩展运算符**·中的数据有·**引用类型**·就是浅拷贝，

-  [深拷贝]()：复制内存中存储的值，并开辟一块新内存空间用于存储。包括：ES6的·**Object.assign**·、·**扩展运算符{ ... }**·、迭代获取基本数据类型进行赋值

  Tips：

  - 虽然ES6的两种方法可以实现深拷贝，但因为堆栈的性质，只能进行一层深拷贝，如果对象下还有对象，则只会进行浅拷贝
  - 深拷贝方法：
    - 最==简单==的：==`Json.parse(Json.stringify(obj))`==
      - 缺点：**无法拷贝**==函数==、和值为==undefined==的属性，==Date==对象会转变为==字符串==

- “**传值和传址**”：·**基本**·数据类型的·**等号赋值**·是传值，而对象这种·**引用**·数据类型则是·**传址**·

-  “**基本数据类型**”：undefined、number、string、null

-  “**引用数据类型**”：对象、数组、函数

## DOM对象操作

- js操作DOM本质其实是修改==标签可配置属性==，所以像==css文件==中的样式是无法通过js读取的

- [classList]()：虽然是个只读对象，但是拥有特殊方法修改

  - `add('class1',...)`，往对象身上添加一个或多个class，同名的不会添加
    - 多次触发`add`方法会给class属性末尾添加多个类名
  - `remove('class',...)`，移出一个或多个class，移出不存在的class不会报错
  - `toggle('class'，true/false)`，true添加类名，false移除类名
  - dom.className是**覆盖**class属性，dom.classList 是**添加**属性或者删除已有属性

- [getElements...]()：带有`Elements`字眼的获取到的节点，哪怕只有一个也会封装到==数组==里返回

- 获取到节点后，控制台输出的第一层字段==都是可配置属性==，比如要获取`<input value="xxx"/>`输入框中的内容，就用==元素.属性名==

  - 所有==属性==均适合这个规则，==除了Class！==，因为==class==是==保留字段==，想取用得用==元素.className==

- ==元素==是==标签==，==节点==是==包括标签、文本、空格==的所有dom

- 获取==父/子==元素

  - [children]()：属性，返回==数组==。获取==子元素==标签`dom.children`(与[childNodes]()相反，childNodes是获取所有==节点==)

  - [parentNode]()：属性，返回==单个元素==。获取==父元素==标签。最多到document层

    Tips：

    - 不要用此方法配合`offsetLeft`等方法获取边距，因为会获取到`document`，offset就应该用offset开头的方法

  - [offsetParent]()：属性。获取上一级带有`position`属性的父级。最多到body层

  - [父节点.getElementBy...]()：函数方法。除了用`document`去调用getElement，节点也可以用该方法来获取底下的所有子节点、孙节点

  - [innerHTML]()：属性。不止用于设置和获取==标签内==文本内容，最主要的作用是显示和设置==标签内所有节点==

    - ```js
      例如：ul.innerHTML = "<li>1</li>
      					<li>2</li>
      					<li>3</li>"
      这样就可以直接 增/删/改 元素   删改 不建议 用这种方式 因为动作幅度太大
      但是往一个 空 的标签里添加 多个元素 用这种方式更便捷
      ```

- 获取==兄弟==元素

  - [previousElementSibling]()：属性。获取==前一个兄弟元素==标签，好处是不用索引即可获取相邻元素
  - [nextElementSibling]()：属性。

- ※[querySelector]()：函数方法。非常强大，因为可以根据[CSS选择器]()来查询一个元素节点。比如`document.querySelector('.aaa div')`，就可以找到class aaa下的div标签元素

  - 仅能找到符合条件的==第一个==元素
  - 同样可以用于某一元素下去搜索子、孙元素
  - [querySelectorAll]()：查找==所有==符合条件的元素
    - 返回的==始终==是一个==数组==
    - `querySelectorAll`其实不如`getElementsByClassName`等方法好用，因为`querySelectorAll`返回的是==固定长度==数组，而`getElementsByTagName`等返回的数组会随着==DOM节点数量改变而改变==
    - `querySelectorAll`节点属性会动态改变，只是返回的数组长度不变

- [createElement]()：创建完元素以后==并不能==查找到节点，因为还在内存中，并没有放入页面，得[appendChild]()放入页面后才能查到

- ==父节点==使用方法：

  - [appendChild]()：向父节点==末尾==添加元素
  - [insertBefore(新节点，旧节点)]()：向旧节点前==插入==一个新元素
  - [replaceChild(新节点，旧节点)]()：==替换==节点
  - [removeChild(子节点)]()：==删除==节点

- [getComputedStyle(元素，伪类元素)]()：==window==方法，**只读**。读取元素==当前生效的样式==。没有设置则会返回==真实值==

  ```js
  let dom = document.querySelector('.aaa')
  let css = getComputedStyle(dom,null) 获取到css对象
  console.log(css.width)//100px
  ```

- [document方法和属性]()

  - [title]()：`document.title`可直接修改页面名称

  - [documentElement]()：用于操作==文档根元素==。

    - 有==style==等属性

    - ※==rem==就是相对于这个根元素的字体大小(rem不设置默认16px)

      ```js
      window.onresize = ()=>{
          let dom = document.documentElement
          let width  = dom.clientWidth
          let t = width / 375	相对最小尺寸的倍数
          let fontsize = t * 10	最小375px时字体大小10px
          if(fontsize > 20) fontsize = 20
          dom.style.fontSize = `${fontsize}px`
      }

- [setAttribute(属性，值)]()：用以给标签元素设置自定义属性


## BOM对象

- [BOM对象]()：window、Navigator、location、History、screen
- [location]()：直接输出得到的是==完整路径地址==。因为浏览器很长，所以有不同方法是为了截取不同==段落==
  - [href]()：完整的URL
  - [reload(参数)]()：`location.reload(true)`传入参数`true`可强制刷新缓存，等同`ctrl+F5`
- [history]()：负责保存历史记录
  - `history.replaceState(null，null，url)`：第三个参数用于修改当前地址栏历史记录
- [window]()
  - `window.onblur`事件可以控制切换不同页面时的动作，同理还有`window.onfocus`
  - `window`是`Window`的实例，像`postMessage`等方法在`window`实例身上
  - `Window`不能直接访问

## 事件冒泡、委派等

- [事件冒泡]()：1、只会==从底层元素往上==传递；2、只能触发==同名==事件，所以阻止冒泡也只能阻止同名事件触发

  - `**冒泡事件的妙用！**`——外层包裹mousedown(不要用click，因为鼠标抬起也会触发一次，其次`**onmousedown**`和`**onmouseup**`是**一对**，onmousedown事件本身并不好用，会在自动获取焦点等情况触发奇怪的问题)显示和隐藏功能，内层则是点击发送指令事件(用同样的关键字`**mousedown**`)，就可以触发指令后隐藏选项框，而不需要遮罩

  - [e.target]()不会随着冒泡移动，点的是哪个元素，指向的就是哪个(只会获取到==元素==，不会获取到元素里的文本节点等)，即使父元素冒泡执行方法，target指向的也是点的元素，所以要用**“currentTarget”**可以获取**绑定事件**的对象，而不是点到的对象

  - ==取消事件冒泡==：[event.cancelBubble = true]()

    Tips：[document]()中包含许许多多的==元素==，当有元素==取消冒泡==，==绑定到document==上的方法==**就不会触发**！==。因为==点击==的是document里的元素，是通过里层的元素冒泡到外层的document，才能执行document身上绑定的方法

  - 当==不同名事件==是父子关系时，先触发的是==父级==的事件

    - ==同名==事件，如都是`onclick`，先触发的就是==底层==元素

  - 事件冒泡只适用于==父子==关系，==同级==的兄弟元素互相==遮盖==的时候，==不会触发==被遮挡元素

- [事件委派]()：给元素==共同的祖先==绑定事件，点击子元素时就会==冒泡==触发==父元素==事件。

  Tips：

  - 减少给同样元素绑定事件次数，提高性能
  - 因为是共同的事件，所以要区分点击的是否是期望触发的元素。通过==e.target==来获取==className==等属性，进行辨识


## 函数、变量提升

- 指函数/变量==声明处之前就使用==，区别是
  - 变量提升后是undefined，而函数可以正常使用
  - 因为变量提升只是在内存中创建但是值以及类型==未定==，而函数声明会在代码进程开始之前，并不是js中所在位置
- ==注意！==用变量声明形式创建的函数并==不会提升==
- **只有**==var==声明的变量才会提升！用==let==延后声明会报错

## 读取文件并传输

- [new FileReader()]()：
  - 用[readAsArrayBuffer(file)]()将文件转换为二进制数据，才能触发[onload]()事件
  - 再用`fr.onload = (e) => { e.target.result }`可以获取到转换的二进制数据

- ==input==标签获取到的文件可以直接用[slice(start,end)]()切割
  - 也可以readAsArrayBuffer转换二进制后切割
- ==二进制==数据没有==length==属性，需要用[.byteLength]()获取字节长度
  - ==文件==用[.size]()属性获取字节长度

## 进程、线程

- ==一个==应用可以启动==多个进程==，一个进程就是一个==实例==

- [多线程方法]()用[web worker]()

  - ==无需==引入js文件

  - ==worker不能操作DOM！==

  - ==不能跨域==加载js文件

  - ```js
    主线程js代码
    let worker = new Worker(js文件路径)	创建worker实例
    worker.onmessage = function (e) {	监听worker通信事件 接收worker传来的数据
        console.log(e.data) //data里装着通信数据
    }
    function query(){	点击事件 通过postMessage方法传递数据
        worker.postMessage(dom.value)
    }
    ```

  - ```js
    worker.js代码
    ※必须用 var 关键字
    var onmessage = function (e){	e.data 接收主线程数据
        //计算
        postMessage(计算结果)	向主线程发送数据
    }
    ```

## MD5、FileReader

- [SparkMD5]()只能读取[FileReader]()对象生成的结果

  - 哪怕是==Blob==或者==File==类型对象也会报错

- [FileReader]()

  - [readAsBinaryString]()：读取为二进制字符串

    - 可以读取==Blob==或者==File==类型对象

    - 适合==小文件==，大文件加载过慢

    - 只有==SparkMD5.hashBinary==才可以读取生成MD5

      ```js
      file_reader.onload = (e) => {
          let md5 = SparkMD5.hashBinary(e.target.result)
      }

  - [readAsArrayBuffer]()：读取为数组缓冲区

    - 可以读取==Blob==或者==File==类型对象

    - 只有SparkMD5的==数组缓冲==对象才可以读取生成MD5

      ```js
      let spark = new SparkMD5.ArrayBuffer()
      file_reader.onload = (e) => {
          spark.append(e.target.result)
          let md5 = spark.end()
      }
      ```

## 错误异常处理

- [try{ }catch( ){ }]()中可以配合[throw]()关键字自定义报错内容

  ```js
  function fn( params ) {
      this.m = params
      this.name = 'xxx'
  }
  try{
      if(...){
         throw new fn('error')	throw相当于return 后面是对象就给catch一个对象 后面是数字 就给catch一个数字
      }
  }catch(e){
      console.log(e.m,e.name) 对应上面自定义函数中的属性
  }
  ```

## 虚拟DOM树DocumentFragment

- 没有父对象的最小文档对象，并不是真实DOM树的一部分，改变虚拟DOM不会触发DOM树的重新渲染
- `let dom = document.createDocumentFragment()`：创建虚拟节点对象，包含节点对象中所有属性、方法
- `dom.appedn(childDom)`：在虚拟节点中==最后==一个子对象后插入子节点
- `dom.preend(childDom)`：在虚拟节点中==第一个==子元素前插入子节点

- 需要注意的地方
  - 将真实DOM树中的节点添加到虚拟DOM中，真实DOM中的节点会==消失==
  - 将==虚拟==DOM添加到真实DOM树中后，会留下一个==空的==`DocumentFragment`