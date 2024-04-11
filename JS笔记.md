## Tips：

- 当**需要**给类型相同的元素**循环绑定函数**时，可以用给元素**添加**自定义标签等方法**标记**，在**触发**事件函数**时**，**获取标记**，传递给同一个事件函数，而不需要创建多个同类函数；已经生成的元素跟它之前所在的数组就没有关系了，不能反推出在之前数组所在的位置

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

- **折叠注释**`//#region ...代码块... //#endregion`

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

- getBoundingClientRect()方法可以获取目标大小及**相对于窗口**的位置

- transform是相对于自身**当前**位置

- (HTML5)自定义属性

  - **必须以data-开头**，取值通过`dataset['myName']`获取（注data-my-name在取值时要写成myName）

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

  - 在==JS==中`\u2620`是==十六进制==表示
  - 在==HTML标签==中是==十进制==表示，`<span>&#9760;`(注意&#开头)。通过计算器计算
  - 在==CSS==中也是==十六进制==，但是不需要==u==，`\2620`
  
- [进制转换]()：

  - 使用[toString(参数)]()方法，里面参数传入几，就是转换到几进制，例如`num.toString(2)`

    - toString转成的==字符串==，不能再使用toString
  - [parseInt(字符串/数字，radix)]()
    - 只能转换第一个字符是数字的字符串
    - 第二个参数只能传==0==或者==2~36==
      - ==0==表示第一个参数是==十进制==，2~36表示==二进制==等
      - 不传则根据规则解析为不同进制的==整数==
        - ==0X==开头的解析为十六进制
        - ==0==开头的解析为八进制或十六进制
        - ==1~9==开头的解析为十进制
  - [toString(radix)]()：只能传入==2~36·==的基数
  - 传入2，数字以二进制显示；传入8，以八进制；16是十六进制
  
- [位运算符]():

  - [十进制数 >> 位移个数]()：向右位移。低位==舍弃==，十进制数如果为正则左边高位补零，为负则补1
  - [<<]()：向左位移。高位舍弃，低位==补零==
  - [>>>]()和[<<<]()：无符号的右移和左移。无符号指==右移==时，不再考虑是正数还是负数，一律高位补零

## 条件语句

- [switch]()：首先会执行==case条件判断==，再执行==之后所有代码==，没有break、return等则会把之后所有case内的代码执行

  ```js
  // 或
  case a:         等同于    if(xxx===a||xxx===b){
  case b:	                     /*代码块*/
      /*代码块*/            }
  break;
  // switch进行范围判断
  switch(true){
      case xxx > 60:
          break;
  }
  // break只能作用到switch不能作用到最近的for循环
  for(let key in obj){
      switch(key){
          case 'id':
             // continue可以作用于最近的for循环
             continue;
          case 'key2':
             // return 作用于最近的函数
             return;
      }
  }

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

- [break]()：==终止==离他==最近的==一**个**循环

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

- [continue]()：==跳过==最近的一**次**循环，也可以使用==outer==标记外层

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

- **this指向**一句话攻略：==谁调用==指向谁，没有指定调用者(==自执行==函数等)就是==window==

  - 特殊情况
    - 箭头函数是==父级的this==，父级是==对象==就==没有this==，指向window

    - `document.addEventListener('xx',fn)`绑定事件`fn`中`this`指向的是==绑定事件的元素节点==

    - `let obj = new Person()`创建实例对象时，构造函数或类里的`this`指向的是==实例对象==

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

  - `()`也可以表示一个==整体==

    ```js
    list.map(item => ({
        a: 1,
        b: 2,
    }))
    // 等同于
    list.map(item => {
        return {
            a: 1,
            b: 2,
        }
    })

- 就是全局函数，this指向window；只有通过object或者say函数调用；

- 函数**声明式**写法**function foo(){/\*...\*/}**，会造成**函数提升**，**不论在何处声明都可以调用，但只有在调用的时候才启用**

- 函数**表达式**写法**var foo=function(){/\*...\*/}**，**不会**造成**函数提升**，所以**必须先声明再调用**；因为在声明foo变量的时候，会**连带执行**后面的**function，**所以**表达式**写法**会立即执行**。

- 函数提升：在预编译阶段将函数声明提升到**对应**作用域最顶端

- 自执行函数作用是创建独立作用域，外面访问不到，避免变量污染

- [自执行函数](https://www.cnblogs.com/jdWu-d/p/11587805.html)

- 不管函数，对象里面如何定义，只有在它**执行的时候**，谁**直接**调用，**this**就指向谁；

- **回调函数不在作用域内**，因为回调函数只是所**声明**函数**的实例**，**本体**还**在外**面，所以不在作用域内，得通过参数传递才能使用

- 当函数只用一次，**声明后立即调用**的情况下使用

- `(function (){})` 是一个 ==函数==对象 通过在后面的==括号==进行==传参== `(function (形参1){})(实参1,...)`

  - 后面的括号是==执行==函数==时==传入的==实参==，`function ( )`的括号是用于接收==形参==

- ```js
  //因为函数在这只是定义，还未调用，再加一个括号就是执行
  (function () {
      // ...
  })()

## 构造函数和工厂函数

- ```js
  #工厂函数     用于批量生产 同类型 对象 都是由Object生成
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
  #构造函数     声明方式和普通函数相同 但是使用时用 new关键词 创建 实例对象 由构造函数生成
  function Person(name,age){
      this.name = name   此处this指向new 创建的对象
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

  - `函数.prototype(Object实例).__proto__(Object原型对象).__proto__(null)`

  - `实例对象.__proto__(Object实例).__proto__(Object原型对象).__proto__(null)`

  - `Object创建的对象.__proto__(原型对象).__proto__(null)`

  - `函数.__proto__(Object实例).__proto__(object原型对象).__proto__(null)`

  - `Function.__proto__(Object实例).__proto__(object原型对象)`

  - 从上可以看出：

    - 实例对象是实例对象，函数对象是函数对象，实例对象的`_proto__`并不是函数对象，而是object空对象，里面有个属性==constructor==，指向实例对象的构造函数

    - `构造函数.prototype`和`实例对象.__proto__`是一个东西，指向同一个object空对象

- [原型链继承]()

  - 即将一个构造函数的==原型重新指向==另一个构造函数的==实例==，`子函数.prototype = new 父()`

  - ==构造函数==及其==实例==也用到了原型链==继承==，他们的原型链比==Object==及实例对象的原型链==多一层==

  - **注意**，因为==子的原型==重新指向了父的==实例==，而实例和构造函数里是没有==constructor==属性的，又因为子的原型是实例，再去==原型==实例==的原型==去找，所以==子的constructor指向父的构造函数==
  - 因为==constructor==指向了改变了，所以就需要重新指向。`子函数.prototype.constructor = 子函数`，因为子函数的实例找`constructor`是从原型链上，因此需要给==父函数实例==身上加个同名属性
  
- 访问一个对象时，会==先==在==自身==寻找，如果没有，则会去==原型对象==寻找

- 当创建[构造函数]()时，可以将==实例身上共有==的属性和方法添加到==构造函数==的原型对象中，这样既可以每个对象都取到，也不会==影响全局作用域==

- ==构造函数==之上还有==Object==原型对象，原型链就到头了

- ==函数==才用`func.prototype`，==对象==用的是`obj.__proto__`

  - 在`let vm = new p()`时，其实内部执行了一次`this.__proto__ = P.prototype`，所以对象身上的隐式原型指跟构造函数原型指向同处

- [原型对象]()是==一个类别==对象/函数的==公共区域==，用于存放==公用==的东西

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

  - 字符串虽然可以像数组那样取值，但是并没有`reverse`方法。可以通过字符串的`split('')`切分，转换成数组
  
- `some((当前元素的值) => { return 条件 } )`

  - 检查数组中是否有某元素，返回的是boolean值
  
- `filter( (当前元素的值) => { return 条件} )`过滤器

  - 返回指定条件的元素组成的==数组==，不会改变原数组
  - ==当前值==是Arry中每一个元素，因为filter其实执行了一次遍历，将Arry中每一个元素拿出来跟return中的判断条件进行比对，再将符合条件的元素合成一个新数组
  - 例如：`数组.filter((数组元素) => { return 数组元素.indexOf(变量) != -1 })`
  
- `find(当前值 =>{ return 条件 })`返回符合条件的==元素==。
  - 跟`filter`很像，但filter返回的是==数组==，**而**`find`返回的是==数组元素==
  
- `findIndex(数组元素 => {return 条件})`返回数组中==第一个==符合条件的元素的==索引==

  - 没有符合条件的元素返回 `-1`
  - 对于空数组不会执行
  
- `list.includes(内容)`(ES7)

  - 检测数组中是否包含指定内容。返回==true/false==
  
- `forEach( (当前值，索引)=>{ } )`

  - foreach中回调的函数是数组中每一个元素都调用一次
  
- `list.reduce((pre，current, currentIndex, array) => {return 计算}，init)`

  
  - `pre`：上一次调用回调函数的结果
  
      - ==第一次调用时==，如果指定了初始值`init`，则`pre`等于初始值，否则为`list[0]`的值
  
  - `current`：当前元素值
  
      - ==第一次调用时==，如果指定了初始值`init`，则为`list[0]`的值，否则为`list[1]`
  
  - `currentIndex`：当前元素值在数组中的索引位置
  
      - ==第一次调用时==，如果指定了初始值`init`，则为`0`，否则为`1`
  
  - `array`：调用`reduce`的数组本身
  - `init`：初始值，可选
  - 返回最后一次回调函数执行结果
  
  ```js
  // 计算1+2+3+4
  let list = [1,2,3,4]
  let res = list.reduce((pre, cur) => {
      return pre + cur
  })
  console.log(res) // 10
  // 取对象里的值
  let obj = {
      a: {
          b: [1, 2, 3]
      }
  }
  let str = 'a.b[2]'
  let list = str.split('.')//将先按.拆成数组 [a, b[2]]
  for(let i in list){
      let val = list[i]
      // 找到带[的元素
      if(val.indexOf('[') !== -1){
          // 将其以[拆成数组 ['b', '2]']
          let array = val.split('[')
          let t = []
          // 遍历 将]剔除 组成新的数组 因为用splice方法必须知道]位置索引
          // 而replace又不能修改原数组 只能将替换生成的新元素组成新数组再替换[a, b[2]]中的b[2]
          for(let k in array){
              let t2 = array[k].replace(']', '')
              t.push(t2)
          }
          list.splice(Number(i), 1, ...t)
      }
  }
  // 设置初始值为obj obj.a.b[2]本质是结果累加 所以用reduce方法
  let result = list.reduce((total, current) => {
      return total[current]
  }, obj)
  ```
  
- `for(let i of array)`遍历数组

- `indexOf('检索值'，'起始位置')`

  - 检索数组中是否有**指定元素**

  - 只检索元素中部分内容是不行的

  - 元素类型不对也是不行的

- `Object.keys(数组)`返回对象**键**组成的数组

- `sort(function(a，b){return a-b或b-a})`
  - 回调函数遵循几个个规则(==改变原数组==)
    - a在数组中的位置==必定==在b==前面==

    - 返回值==大于==0，元素==交换==位置
    - 返回值==小于==0，位置==不变==
    - 返回值==等于==0，认为元素相等，位置==不变==
    - 因此[a-b]()大于0则a，b交换位置，是==升序==；而[b-a]()，后面的元素减前面的元素，大于0则b，a交换位置，是==降序==

- `list.concat(list2, element, ...)`

  - 返回值为多个数组及元素组合成新数组
    - 例：`list = [1, 2] list.concat([3, 4], '5')`返回值`[1, 2, 3, 4, '5']`
  - ==不改变原数组==

- `list.map(item => {return 处理item})`返回一个新==数组==

  - ==不改变原数组==
  - 数组中的元素为回调函数处理后的==返回值==
  - ==空数组==不会执行
  - 回调函数参数`list.map((currentValue, index, arr) => {})`

    - `currentValue`：当前元素值
    - `index`：当前元素索引值
    - `arr`：当前元素所属数组


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
        //输出匹配结果数组
        console.log(val) // [0:匹配值, ..., groups:有()或分组的显示在这, index:位置索引, input:原始数据]
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

- `Object.keys(对象)`：返回对象**键**组成的==数组==
- `Object.values(对象)`(es8)：返回对象**值**组成的==数组==
- `Object.entries()`
  - 通常对象是不可直接遍历的，而通过`entres`可以返回一个键值对组成的数组(**二维数组，第一层是有多少个键值对，第二层是键和值组成的数组**)，从而可以在es6中的新for循环中使用，使用方式`for(let [key,value] of Object.entres())`
    - `[key,value]`是es6新提出的**解构赋值**
  
  - **Array**也可以使用，只不过是以数组索引作为key
  - 如果对象下有对象，得到的结果就只是第一层的，对象下的对象在entries后，在数组中还是对象
  
- `Object.assign(目标对象，源头对象)`(es6)：用于对象合并，合并同类赋值，不同类的作为新增，返回值是**新的目标对象**。但也可以用**扩展运算符**更简洁
  - 在**vue**中，提前准备好的属性在合并后依然具有响应性
  - 有响应式的对象和无响应式对象合并，没有响应式的依然没有响应式
- `for ( let key in object )`：遍历对象==和数组！==
  - 与`for(let value of array)`不同的是，遍历的是key，而`for of`遍历的是值
  - ==注意！==此方法会遍历出==原型链==上的属性！
  - 遍历数组时，==key均为字符串！==

- `delete  obj.xxx`：操作符，等同与或符。返回删除成功与否
- `hasOwnProperty`：使用`xxx in obj`会寻找obj==原型链==上的属性方法，而==hasOwnProperty==只会查找`obj`身上的属性方法

###   window方法：

- window方法可以省略`window`关键字，直接写后面的方法
- 弹窗：警告窗[alert('xxx')]()；确认框[confirm('显示内容')]()，==返回布尔值==
  - [prompt(string)]()：类似==alert()==，传入一个==字符串==作为提示文字，会弹出一个输入框，输入的内容作为`prompt函数`的返回值
- **history**.pushState/**replaceState**(state,title,url)可以在**不跳转页面**的情况下**改写**当前页面的url
- **跳转页面**：window.location.**href** = "地址?键值对"，跳转页面后使用`**location.search**`获取到`？`之后的内容，对获取的内容进行字符串分割，创建对象obj = { }，使用**obj['key'] = '值'**，将分割好的键值对装入对象
- [sessionStroage]()：会话存储
  - 使用`sessionStroage.自定义变量名 = 值`，就可以将**值**存入**自定义变量**名，取用的时候直接用`sessionStroage.自定义变量名`就可以获取值
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

- 当以`fn()`调用时，==this==指向==调用者==，例如：`a.b.fn()`，this指向`b`

- 当以`new fn()`调用时，==this==指向==实例对象==

- `fn.call(obj, params1, params2,...)`：传入的第一个参数是==this的指向==，后面的参数是通常函数调用时传入的==实参==

  - `obj.fn.call()`==改变指向的同时就已经执行了==

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
    ```

  - `call`原理

    ```js
    Function.prototype.newcall = function (obj, ...p) {
        let obj = obj || window //call方法传入null时默认指向window
        obj.p = this //关键点 因为调用者是函数所以直接复制下来
        let res = obj.p(...p) // 传参、执行并存储返回结果
        delete obj.p //关键点 因为不改变原对象所以要删除复制的属性
        return res
    }
    function fn(...p) {
        console.log(this.name)
        console.log(...p)
        return 1
    }
    let obj1 = {
        name: 'xxx1'
    }
    console.log(fn.newcall(obj1, 1, 2, 3, 4))

- `fn.apply(obj, [...args])`：同`call`，但第二个参数是==实参==组成的==数组==，==传入==函数时已经被==展开==，并==依次==赋值给形参

  - `fn.call(obj)`和`fn.apply(obj)`等同于`obj.fn()`，只不过是临时的，让`fn()`成为`obj`的方法**调用**
  - **注意！**等同于只是说相当于这样用，但不能真的写成`obj.fn()`！因为obj里没有`fn`方法，`.call`只是临时的！

- `fn.bind(newObj, params1, params2,...)`：不同于`call`和`apply`，**返回**的是一个函数，==不会直接调用==

  - 直接调用：`obj.fn.bind(newObj)(params)`
  
  - 原理
  
    ```js
    Function.prototype.newBind = function (obj, ...p){
        let that = this //关键点 返回的匿名函数 保存this
        let cusfn = function (){
            // 关键点 bind返回的函数可以用 new关键字 只不过会丢失this
            // 而instanceof 可以判断是否是实例关系 利用这点可以判断是否使用了new
            if(this instanceof cusfn){
                // 关键点 因为要判断是否使用了new即实例 所以必须用具名函数
                that.call(this, ...p)
            }else{
                that.call(obj, ...p)
            }
        }
        return cusfn
    }


## 对象

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

- 只要是==函数==，加`()`就可以执行，哪怕是==变量==形式

- 区分==对象==和==数组==

  - `xxx.constructor === Array/Object`

  - ``xxx instanceof Array/Object`

  - `Array.isArray(xxx) //true或false`

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

- function==函数也是对象==

  - 不加`()`，作为实参时传入的是==函数对象==，不会执行

  - 加`()`，作为实参时则==先执行==再传入执行后的==结果==

  - 没有跟在`return`后的匿名函数，要么赋值保存`let fn = function()`，否则单`function (){}`会因为==没有函数名==被视作两个独立部分，因此需要用括号包裹`(function (){})`，表示成整体，==自执行函数==由此演化而来

  - ※函数既是对象，因此也可以往其自身添加属性及方法

    ```js
    function express(){
        let app = function (){}
        app.use = function(params){...}
        const http = require('http')
        const server = http.createrServer(app)
        app.listen = function(port, callback){
            server.listen(port, callback)
        }
        return app
    }
    let app = express()
    app.use(...)
    app.listen(80, ()=>{
        console.log('80端口开启')
    })

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
  - **※** 可以接收参数，`new Date(年, 月, 0)`创建指定年月的==月末==日期对象，再用==getDate()==获取==指定月份有多少天==
    - **注意**末尾传参==0==很关键
    - ==new Date('2023/1/10')==与==new Date(2023，0，10)==生成的日期对象是一样的
  - 可接收参数，`new Date(时间戳)`，转换为年月日==时分秒==
  - 可接收参数`2022-8-25`不用写具体时间，会转化为当天`00:00:00`
- [getDate()]()：获取当前日期==几号==
- [getDay()]()：获取当前==星期==，返回==0-6==的值，==0表示周日==

  - ==getDate==减去==getDay==等于所在周的周日那天
  - `(当月第一天星期 + index(从0开始)) % 7`即可得出当月每天的星期
- [getMonth()]()：获取当前==月份==，返回==0-11==的值，==0表示1月==

  - 虽然`getMonth()`返回的是`0~11`但是`new Date('2022-1-1')`中传入的日期月份接收`1~12`

- [getFullYear()]()：获取当前==年份==
- [setTime(时间戳)]()：==改变原Date对象==。`date.setTime(xxx)`，date就会变为时间戳对应的时间
- `Date.now()`和`dateObject.getTime()`：获取==当前时刻时间戳==

## Math对象方法

- `Math.floor(num)`：向下取整(floor意为地板)

- `Math.ceil(num)`：向上取整(ceil意为天花板)

- `Math.round(num)`：四舍五入。等同于Math.floor(x+0.5)，因为能四舍五入进一位的数加上0.5一定可以进一位

- ※`Math.random()`：生成==**包括**0~**不包括**1==的随机数

  - 生成==X~Y==的随机数：`Math.round( Math.random() * ( Y - X ) + X )`（思路是：0~1乘一个数扩大范围，再加一个数确保最小值，但是要保证不超限，所以乘的数等于差值）

- `Math.pow(底数，指数)`：求幂运算
- `Math.sqrt(num)`：开方运算
- `Math.max/min(a，b，c，...)`：返回最大/最小值
- `Math.abs(num)`：绝对值。传入一个数字

## 定时器

- 当一个函数同时执行两个不相干==动作==时，会卡顿，比如：骑自行车需要脚提供速度和手提供方向，如果将这两个动作放在一起，整个过程就会变得卡顿！
  - 方法：用[setInterval]()定时执行==动作==，通过==触发事件==设置==定时器触发条件==

- `for循环`中，如果==索引==用`var index`声明，则索引就变成了全局变量，定时器内不会保存当前索引值，因为等执行时根据索引的地址值去找时，索引已经是最终累加的结果。
  - 但如果是`let index`声明，则每个循环中都是==独立作用域==下的`index`

- ==清除定时器==后，==不会立即==将整个回调函数干掉，而是等全部逻辑执行完后再清除
- 定时器的回调函数并==不是到时间就触发！==必须在主线程栈里面的==初始化代码==逻辑执行出结果后，才轮到==事件队列==(事件循环模型)里的代码执行
  - ==事件==包括：==onclick==等==监听事件==，定时器的==回调函数(事件)==

  - `setTimeout`是运行完一次后，==进行延迟，再==出发下一任务，所以一定会满足延迟时间
  
    - 对比`setInterval`是间隔一段时间将==任务放入任务队列==，而事件队列一次只能执行一个回调，前一个执行时间过长就会导致时间错乱
  
  - 因此使用`setTimeout`模拟`setInterval`
  
    ```js
    function newInterval(callback, delay) {
        // 注 为了每次递归调用自身时传入参数 不能直接调用外部函数 需要将callback留到闭包中
        function inside(){
           callback()
           // 使用递归让每次setTimeout执行完再次执行自身
           setTimeout(inside, delay)
        }
        setTimeout(inside, delay) // 第一次执行时的定时器
    }
    newInterval(() => {
        console.log(111)
    }, 1000)
  
- `setTimeout()`可以不传入时间参数，使其内部代码==异步==执行！配合页面挂载元素，在==异步==操作元素实现类似Vue`$nextTick(()=>{})`效果

## 元素在页面中的位置

![img](https://upload-images.jianshu.io/upload_images/6322775-d2bce26f79b3c151.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- [offsetWidth/Height]()：元素整个大小。包括宽高、==边框==

- [clientWidth/Height]()：可视大小，有滚动条的话，会==减去滚动条大小==。包括padding、content
  - ==鼠标==的[clientX/Y]()也是==相对于**可见窗口(body)**==的位置，可视窗口的左上角始终是(0,0)点。想要获取相对于==页面(超出body大小)==的坐标用[pageX/Y]()
  
- [scrollHeight/Width]()：滚动大小(==不是元素自身大小！是被撑大后的大小==)。包括==可视区域（clientHeight）==、滚动条大小、隐藏部分
- ==判断滚动条到底==：`scrollHeight - scrollTop == clientHeight`
    - **注意**：`offsetHeight + scrollTop > scrollHeight`
- 滚动大小实际上就等于==滚动条可滚动宽/高==加上==可视窗口大小==

![img](https://upload-images.jianshu.io/upload_images/6322775-0cf168f634ec329d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- offsetTop/Left是相对父级（注：滚动显示容器里，里面的每一个元素它的offset父级都是显示容器），并且会显示(※)**累计滚动隐藏部分**；
- offset返回的是绝对值，哪怕设置的是百分比，返回的也是px；offsetHeight是自身

![img](https://upload-images.jianshu.io/upload_images/6322775-6dd4b0d7e07d70c9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- `scrollHeight`包括不可见部分的元素高度（只包含padding和content，即使是在IE盒模型下也不会计算border的值）

![img](https://upload-images.jianshu.io/upload_images/6322775-3a3c4a65808c7cb1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

<center/>scrollTop（滚动条向下滚动的距离）

![image-20230912111718695](C:/Users/admin/AppData/Roaming/Typora/typora-user-images/image-20230912111718695.png)

- `getBoundingClientRect()`求元素==距视窗==边距

  - 会随滚动而变化，不是元素在页面的固定边距！
  - 不是距离祖、父元素的绝对距离！


### 懒加载

- 方法一：触发频繁不推荐

  - 通过`data-`自定义属性，使得既保留图片地址，又不会自动加载

    ```html
    <img data-src="1.png">
    <img data-src="2.png">
    <img data-src="3.png">

  - 判断距离视窗顶部距离是否小于视窗高度，如果小于则将自定义属性取出添加到`<img src>`属性上

    ```js
    let img_list = document.querySelectorAll('img')
    // 监听滚动事件
    window.addEventListener('scroll', e => {
        for(let val of img_list){
            let img_top = val.getBoundingClientRect().top
            if(img_top < window.innerHeight){
                let src = val.getAttribute('data-src')
                val.setAttribute('src', src)
            }
        }
    })
    ```

- 方法二：浏览器提供的构造函数`intersectionObserver`(推荐)

  - 只有节点进入和离开==可见区域==时才触发，对比方法一是滚动时一直触发。其次触发一次后取消观察即可，后续不会再重复执行，触发频率低
  - `intersectionObserver`是==构造函数==，因此要创建实例`const observer = new IntersectionObserver(callback)`，传入回调函数作为参数
  - 实例身上的方法
    - `observer.observe(DOM节点)`观察目标节点，当目标节点出现和消失在可见区域内时触发回调函数
    - `observer.unobserve(DOM节点)`==取消==观察目标节点

  ```js
  // HTML部分和方法一相同
  
  // 获取节点
  let img_list = document.querySelectorAll('img')
  // 此回调函数会在看见时和看不见时触发
  const callback = (entries, observer) => {
      // 传入的形参是 所有观察对象组成的数组
      entries.forEach(item => {
          // 每项的isIntersecting属性值为boolean 表示是否交叉 即是否在可见区域内
          if(item.isIntersecting){
              // target属性即观察对象节点
              let dom = item.target
              // 将自定义属性值取出放到src属性上
              let src = dom.getAttribute('data-src')
              dom.setAttribute('src', src)
              // 触发后已经将src赋值加载到页面了不再需要观察
              observer.unobserve(dom)
          }
      })
  }
  // 创建实例
  const observer = new IntersectionObserver(callback)
  // IntersectionObserver可以传入options指定作为视口的元素
  const options = {
      // 如果为null或者没有传入options则 默认使用浏览器视口
      root: document.getElementById('xx'),
      rootMargin, // 扩展或缩小root边界 默认0px
      // 指定观察者的回调函数在元素的可见比例 穿过这些值时被执行 值是数字或数字组成的数组
      // 默认为0 目标元素一出现在视口中就会触发回调
      threshold,
  }
  const observer = new IntersectionObserver(callback, options)
  // 对每个图片节点都进行观察
  img_list.forEach(dom => {
      observer.observe(dom)
  })

- Tips

  - `onwheel`：鼠标滚轮事件
    - 想横向滚动，可以用“@wheel”，此事件在**鼠标滚轮**滚动时触发，然后手动设置，事件触发一次横向滚动距离
    - wheelDelta为正则是滚轮向上，为负则向下
  - `onscroll`：滚动条滚动事件
  - ※**offset偏移量**是依据**上一级定位元素(position:relative等)**确定的
  - ※**offsetParent**和**parentNode**的区别：

  - `offsetParent`
    - 查找路径：有带有**position**属性的上一级→带有position属性的上一级。(body虽然没设置position，但是因为所有元素都脱离不了body文档流，所以总能找到body是最外层)
    - 最外层：body→null

  - `parentNode`
    - 查找路径：**不考虑position**，单纯的父级元素往上查找。
    - 最外层：body→html→document→null
  - ==判断滚动触底==
    - `scrollTop`取值范围：`[0，(scrollHeight - clientHeight)]`
    - ==滚动条到底==：`scrollTop = scrollHeight  - clientHeight`
    - ==滚动条距离底部50px以内==：`(scrollHeight  - clientHeight) - scrollTop <= 50`
    - ==滚动条距离底部5%以内==：`scrollTop / (scrollHeight  - clientHeight) >= 0.95`

## iframe标签

- 是一个==单独的视窗==

- 内部执行的任何方法都==获取不到==iframe==父级==的元素

  - **只能**通过==获取父级页面window对象==`window.parent.document`才能操作==父页面==document元素

- iframe内的元素获取==相对视窗==的位置都是以==iframe为边界==

  - 鼠标事件也取不到iframe外的边距

- 会在执行完父级的js后再执行iframe中内容

- `vw`和`vh`是相对于视窗的，即是说是==相对于iframe==，而不是浏览器窗口

- ==父获取子==iframe的window：`iframeElement.contentWindow`
  - ==子获取父==的window：`window.parent`

- 通过`window.parent.父级方法`调用、传参到父级

  - ==必须在服务器环境下！==
  - 可以通过函数传参，来获取父页面元素，但这种方法前提是，父页面也是自己编写

- `window.parent.document.querySelector`：通过==window.parent==获取到父==页面==document从而获取元素

- 不同页面间通信`postMessage`、`onmessage`

  - 不是==多进程==独有的方法
  - window自带==postMessage==以及==onmessage==，`window.postMessage(数据)`是给==自身==发送消息，使用`window.onmessage = function(e){e.data}`即可接收到数据
- `window.open`即使在`iframe`中调用，依然会在浏览器打开新页签


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

- `object.defineProperty(对象名，'键'，{ 配置项 })`

  - ```js
    let source = {
        name: 'xxx',
        age: 18
    }
    let proxy = {}
    for(let key in source){
        Object.defineProperty(proxy, key, {
            get(){
                // 代理对象本身只是一个空壳 映射 到原始对象
                return source[key]
            },
            set(value){
                // 所有修改/读取操作均发生在原始数据身上
                source[key] = value
            }
        })
    }

![img](https://upload-images.jianshu.io/upload_images/6322775-b5e3b3b6e236d059.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

<center>为指定对象添加属性；并且该属性不可被遍历

- `defineProperty`定义的属性==默认不可被修改、检索==

- `defineProperty`的好处
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

  - 当**扩展运算符**中的数据有**引用类型**就是浅拷贝，

-  [深拷贝]()：复制内存中存储的值，并开辟一块新内存空间用于存储。包括：ES6的**Object.assign**、**扩展运算符{ ... }**、迭代获取基本数据类型进行赋值

  Tips：

  - 虽然ES6的两种方法可以实现深拷贝，但因为堆栈的性质，只能进行一层深拷贝，如果对象下还有对象，则只会进行浅拷贝
  - 深拷贝方法：
    - 最==简单==的：==`Json.parse(Json.stringify(obj))`==
      - 缺点：**无法拷贝**==函数==、和值为==undefined==的属性，==Date==对象会转变为==字符串==

- “**传值和传址**”：**基本**数据类型的**等号赋值**是传值，而对象这种**引用**数据类型则是**传址**

-  “**基本数据类型**”：undefined、number、string、null

-  “**引用数据类型**”：对象、数组、函数

## DOM对象操作

- js操作DOM本质其实是修改==标签可配置属性==，所以像==css文件==中的样式是无法通过js读取的

- 元素节点也是对象，既可以用`===`判断是否为同一节点，修改节点属性也不会改变节点对象的==地址值==，因此还是同一节点

- [classList]()：虽然是个只读对象，但是拥有特殊方法修改

  - `add('class1',...)`，往对象身上添加一个或多个class，同名的不会添加
    - 多次触发`add`方法会给class属性末尾添加多个类名
  - `remove('class',...)`，移出一个或多个class，移出不存在的class不会报错
  - `toggle('class'，true/false)`，true添加类名，false移除类名
  - `dom.className`是**覆盖**class属性，dom.classList 是**添加**属性或者删除已有属性

- [getElements...]()：带有`Elements`字眼的获取到的节点，哪怕只有一个也会封装到==数组==里返回

  - 获取的是==动态==节点数组，如，创建全局变量保存节点数组，后续进行增删节点，全局变量的节点数组也会随之改变

  - **但是**`querySelectorAll`返回的是静态长度的固定长度数组

- 获取到节点后，控制台输出的第一层字段==都是可配置属性==，比如要获取`<input value="xxx"/>`输入框中的内容，就用==元素.属性名==

  - 所有==属性==均适合这个规则，==除了Class！==，因为==class==是==保留字段==，想取用得用==元素.className==

- ==元素==是==标签==，==节点==是==包括标签、文本、空格==的所有dom

- 获取==父/子==元素

  - `children`：属性，返回==数组==

    - 获取==子元素==标签`dom.children`(与[childNodes]()相反，childNodes是获取所有==节点==)

  - `parentNode`：属性，返回==单个元素==

    - 获取==父元素==标签。最多到document层
    - 不要用此方法配合`offsetLeft`等方法获取边距，因为会获取到`document`，offset就应该用offset开头的方法

  - `offsetParent`：属性。获取上一级带有`position`属性的父级。最多到body层

  - `父节点.getElementBy...`：函数方法。除了用`document`去调用getElement，节点也可以用该方法来获取底下的所有子节点、孙节点

  - `innerHTML`：属性。不止用于设置和获取==标签内==文本内容，最主要的作用是显示和设置==标签内所有节点==

    ```js
    例如：ul.innerHTML = `<li>1</li>
    					<li>2</li>
    					<li>3</li>`
    这样就可以直接 增/删/改 元素   删改 不建议 用这种方式 因为动作幅度太大
    但是往一个 空 的标签里添加 多个元素 用这种方式更便捷

- 添加节点

  - `parent.appendChild(节点)`或`parent.append(节点)`
    - 在指定父节点的子节点列表末尾插入节点

    - 节点会在传入`append`方法时，从它的==父节点中删除再插入新的位置==
      - 因为同一个节点不可能出现在文档中不同位置
      - 要保留原节点，需==先使用==`child.cloneNode()`创建一个副本，再将副本插入目标父节点
        - 注：副本不会自动保持节点状态同步

    - ==返回值==：追加后的子节点，如果是虚拟节点`documentFragment`，则会返回空`documentFragment`
    - 如果传入的是`documentFragment`，则会将`documentFragment`中所有节点==转移==到父节点，并留下一个空的`documentFragment`

- 获取==兄弟==元素

  - [previousElementSibling]()：属性。获取==前一个兄弟元素==标签，好处是不用索引即可获取相邻元素
  - [nextElementSibling]()：属性。

- ※[querySelector]()：函数方法。非常强大，因为可以根据[CSS选择器]()来查询一个元素节点。比如`document.querySelector('.aaa div')`，就可以找到class aaa下的div标签元素

  - 仅能找到符合条件的==第一个==元素
  - 同样可以用于某一元素下去搜索子、孙元素
  - [querySelectorAll]()：查找==所有==符合条件的元素
    - 返回的==始终==是一个==数组==
    - `querySelectorAll`其实不如`getElementsByClassName`等方法好用，因为`querySelectorAll`返回的是==固定长度==数组，而`getElements...`等返回的数组会随着==DOM节点数量改变而改变==
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

- `setAttribute(属性，值)`：用以给标签元素设置属性

  - 注！不是==自定义==属性，属性名写什么就是什么，没有`data-`前缀
  - 例`imgDom.setAttribute('src', '1,png')`

- `getAttribute(属性名)`：获取标签元素属性值

  - 例` let src = imgDom.getAttribute('data-src')`

- `dom.dataset['自定义属性名']`：获取元素身上==自定义属性==

  ```html
  <div data-xx="aaa"></div>
  <div data-xx="bbb"></div>
  ```

  ```js
  let dom_list = document.querySelector('div')
  for(let val of dom_list){
      val.onclick = (e) => {
          // 通过event事件对象获取
          //data-前缀的自定义属性都在dataset属性中
          console.log(e.path[0].dataset.xx)
          // 通过dataset属性获取
          console.log(val.dataset['xx'])
      }
  }

## 虚拟节点documentFragment

- 一个文档片段，轻量版的`document`，不是真实DOM树的一部分，它的改变不会触发DOM树重新渲染

- 用例

  ```js
  // 创建
  // 此时是一个空的文档片段
  let fragment = document.createDocumentFragment()
  // 从真实DOM树装入节点
  let dom = docuemnt.querySelector('ul')
  let child
  // 每次循环将dom的第一个节点转移到虚拟节点树中
  // 通过appendChild装入的节点会从真实DOM树中删除
  while (child = dom.firstChild) {
      fragment.appendChild(child)
  }
  // 对虚拟节点编辑后插入真实DOM树
  dom.appendChild(fragment)

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

## 事件流、冒泡、捕获

1. 首先是从最外层想内的捕获阶段
   - 从外向内，找到**点击**的最里层子元素
   - 捕获过程中遇到的事件都==不会触发==
2. 执行目标元素身上的事件(如果有的话)
3. 再从目标元素事件向最外层冒泡阶段
   - 从内向外，依次触发父级元素身上的事件(无论是否同名事件)
   - 阻止冒泡也只能阻止同名事件触发
   - ==事件冒泡的妙用==：
     - 子元素绑定`onmousedown`，并阻止冒泡，显示选项框，父容器绑定`onmousedown`，在选项框上点击后会冒泡到父容器，关闭选项框
   - `event.target`
     - 表示所点击的最里层元素
       - 例：多层嵌套的元素，最外层父容器上绑定了`click`事件，点击子元素触发`click`，传入的`event`事件对象中`target`表示从哪个子元素开始冒泡，并不是触发事件的父元素
     - 即使父元素冒泡触发事件，target指向的也是点的元素
   - `event.currentTarget`
     - 表示==绑定事件==的元素节点，而不是点击的最里层子元素
   - 取消事件冒泡
     - `event.cancelBubble = true`
   - 当层级嵌套元素身上绑定的是==不同名事件==
     - 先触发的是**父元素**==不同名==事件！
     - 然后才是从最里层子元素到父元素的==同名事件==触发
     - 例，层级嵌套元素，最外层绑定`onclick`，中间层绑定`onmousedown`，最里层绑定`onclick`，点击**最里层**元素时，先触发`onmousedown`，再从最里层的`onclick`冒泡到最外层`onclick`
   - 事件冒泡只适用于==父子==关系，==同级==的兄弟元素互相==遮盖==的时候，==不会触发==被遮挡元素

- 事件委派
  - 给子元素共同的父容器绑定事件，点击子元素就会==冒泡==触发==父元素==事件
  - 因为是共同的事件，所以要区分点击的是否是期望的元素。通过`event.target`获取点击的子元素，通过==className==等属性，进行区分

## try catch finally

- ```js
  try{
      // 操作流程
  }catch(err){
      // 操作流程中抛出异常时进入catch
  }finally{
      // try正常执行完后执行
      // catch异常处理完后依然会执行
      // catch中处理异常时抛出异常会先进入finally 然后再抛出catch中操作异常
      // try中有return后执行
  }

- 总结
  - 无return且未出现异常`try-->finally`
  - 出现异常`try-->catch-->finally`
  - try或catch中有`return`
    - 返回值为==基本类型==，finally执行完不会改变返回值
    - 返回值为==对象==，finally执行完会改变返回值
    - finally中有`return`会改变try、catch的返回值

## 宏任务和微任务

- 宏任务
  - 花费时间较长
  - 包括
    - 新程序或子程序被直接执行，如`<script>`标签中运行的代码，控制台中输入的代码属于==子程序
    - 事件回调函数，如鼠标点击事件
    - `setTimeout`、`setInterval`
    - `I/O`操作、`UI rendering`等
- 微任务
  - 花费时间短
  - 包括
    - `Promise.then().catch().finally()`
    - `MutationObserver `
    - `Object.observe`
- 事件循环`event loop`
  - 是一个不断进行循环的机制，寻找可以执行的任务
  - 在执行完==同步任务==，即清空==调用栈==后，会**先**执行==微任务==，将微任务==队列==清空后才会执行==宏任务==
  - `主线程宏任务`→`微任务`→`渲染`→`宏任务`

## 柯里化

- 用嵌套函数闭包的形式将接收==多个参数==的函数变换成接收==单一==参数的函数

  - 换言之，就是多个函数接收不同参数最后组合在一起使用

  ```js
  function fn1(p) {
      return function fn2(p2, p3) {
          return `${p}${p2}:${p3}`
      }
  }
  // 第一个参数是重复的 就可以作为闭包中默认参数
  const url = fn1('https://')
  let t = url('127.0.0.1','8080')
  console.log(t)
  ```

- 柯里化在兼容性方面的应用

  ```js
  // IE等老浏览器没有addEventListener
  // 执行script脚本时自执行 添加一个全局变量(函数) 使用返回的新函数添加事件监听
  const whichEvent = (function () {
      // 如果支持addEventListener方法
     if(window.addEventListener) {
         return function (dom, type, callback, useCapture) {
             // useCapture是进行捕获或事件冒泡的选项 默认false冒泡
             dom.addEventListener(type, function (event) {
                 // 因为传入的回调函数不确定是什么形式this指向不同 为了规避this指向问题
                 // 就得用call强制改变调用者为触发事件的元素节点 并传入event事件参数
                 callback.call(dom, event)
             }, useCapture)
         }
     } else if(window.attachEvent) {
         return function (dom, type, callback) {
             dom.attachEvent('on' + type, function (e) {
                 callback.call(dom, e)
             })
         }
     }
  })()
  
  // 绑定事件监听时
  let dom = document.querselector('.aa')
  whichEvent(dom, 'click', function (e){...}, false)

- 面试题

  - 观察规律可得，每个函数的返回值都是一个新的函数复制，且执行累加计算

  ```js
  count(1)(2)(3) = 6
  count(1,2,3)(4) = 10
  count(1)(2)(3)(4)(5) = 15
  ```

  - 关键点

    - `count()`函数执行结果不能只返回一个函数，其返回函数的返回值也得是该函数本身
    - 函数的返回值是另一个函数，但是在==**打印输出**==时会==隐式转换==为==字符串==，也就是执行了`toString()`
      - 这里打印输出很重要！因为函数执行结果是函数本身，那这个函数是没有办法终止并最终输出结果的，但是在函数被`console.log()`打印输出时，会执行`toString()`，所以只要==覆写==`toString`方法让其计算结果即可

    ```js
    function count(...params) {
        // 注:这里用了rest参数 是数组
        let fn = (...params2) => {
            // 注:这里用了拓展运算符将数组展开
            params.push(...params2)
            // 注此处是后续fn返回fn自身
            return fn
        }
        // 覆盖toString方法 计算结果 params是闭包变量
        // console.log打印的是fn()执行结果 因此要覆盖fn的toString
        fn.toString = () => {
            return params.reduce((pre, cur) => {
                return pre + cur
            })
        }
        // 此处是count()第一次执行返回的fn
        return fn
    }
    console.log(count(1)(2)(3)) // 6

- 柯里化==箭头函数==的使用

  ```js
  let list1 = [
      {aaa: '111', bbb: '222'},
      {aaa: '333', bbb: '444'},
  ]
  let list2 = [
      {ddd: false},
      {ddd: true},
  ]
  // 柯里化
  let fn = name => item => item[name]
  // 等同于
  let fn = (name) => {
      // 箭头函数只接受 单个参数 时可以省略()
      return (item) => {
          // return后只有 一个值 时可以省略 return
          return item[name]
      }
  }
  let name_aaa = fn('aaa')
  let name_ddd = fn('ddd')
  // 数组.map返回传入回调执行结果组成的数组
  console.log(name_aaa.map(name_aaa)) // ['111', '333']
  console.log(name_aaa.map(name_ddd)) // [false, true]
  
  // 对比非柯里化函数
  list1.map(item => item.aaa)
  list2.map(item => item.ddd)
  ```

## 拖拽

- `dragover`和`drop`需要`e.preventDefault()`取消默认行为

- 拖拽排序
  - 设置“目标位置 != 当前”，可以使程序只执行一次，并且要注意有没有接触到外边框，设置条件在外边框不触发
  - 排序逻辑
    - 因为使用的前插入方法，所以小的插在大的后一个的前面，大的插在小的前面
    - 排序判断条件当前索引大小和目标索引大小做比较
    - 计算索引逻辑
      - 不断循环用前一个节点赋值，直到前一个节点为空，累计计算的自定义变量值就是节点的索引值

- ※`drag`事件中，想要`drop`必须阻止`dragover`的默认操作

- 用在拖拽**目标**上的事件

  - `ondragstart `开始拖动**元素**时触发


  - `ondrag `**元素**正在拖动时触发

  - `ondragend `完成**元素**拖动后触发
  - `ondragenter `当被鼠标拖动的对象进入其**容器范围**内时**触发**此事件
  - `ondragover `被拖动的对象在另一对象**容器范围**内拖动时**触发**此事件
  - `ondragleave `鼠标拖动的对象离开其**容器范围**内时**触发**此事件
  - `ondrop `在一个拖动过程中，释放鼠标键时触发此事件


## MSE(Media Source Extensions)

- B站开源的==flv.js==思路就是将FLV文件流转码成MP4片段(video标签可播放格式)，然后通过MSE将MP4片段喂给`<video>`
- MSE是W3C规范，即新的API接口，允许JS为`<audio>`和`<video>`==动态==构造数据源
  - 没有MSE时，对`<video>`的操作仅止于标签提供的能力，不能对视频流进行操作
  - 如在线视频播放切换清晰度，直接替换`src`会导致视频中断，重新播放
  - MSE可以创建多个`sourceBuffer`对象，可以理解为准备了多个不同清晰度的源。对象内可以==动态==添加==数据片段==，这些数据片段可以是不同清晰度
  - 例YouTube的video标签中`src="blob:https://..."`，可以看到地址栏是一个二进制文件对象，可以像操作文件一样操作Blob对象，从而动态的往`<video>`标签塞入二进制数据
- MSE可以
  - 动态清晰度切换
  - 视频拼接。如视频插入广告
  - 音频语言切换
  - 动态控制视频加载
  - ...

## 网页打开电脑软件

- 方法是通过自定义URL协议，让网页调用本地安装的客户端

- 自定义协议

  - 格式：`xxx://参数`
    - `xxx`是协议头，由注册表中`HKEY_CURRENT_USER\Software\Classes\xxx`项名称决定
    - 参数可以更改`...\xxx\Shell\Open\command`的`默认`字符串值来更改

- 如何注册自己编写的软件进注册表

  1. 首先得有一个`.exe`文件

  2. 创建`xx.txt`文件，填入下面内容

     ```txt
     Windows Registry Editor Version 5.00
     
     [HKEY_CURRENT_USER\Software\Classes\test]
     "URL Protocol"=""
     @=""
     
     [HKEY_CURRENT_USER\Software\Classes\test\Shell]
     
     [HKEY_CURRENT_USER\Software\Classes\test\Shell\Open]
     
     [HKEY_CURRENT_USER\Software\Classes\test\Shell\Open\command]
     @="\"C:\\xx.exe\" \"%1\""
     ```

  3. 修改`.txt`文件后缀名为`.reg`。打开此文件，注册表编辑器会弹出对话框，询问是否确认添加

  4. 测试地址栏打开桌面应用

     - 地址栏输入`xxx://11`就会提示打开`xx.exe`

- 安卓和IOS跳转应用原理

  - IOS

    - **URL Schemes**

      - 类似HTTP 协议，是应用之间的通信协议
      - 形如`appName://somepath?parameter=value`

    - **openURL 方法**

      - IOS提供了`UIApplication` 类，其中`openURL:` 方法，用于打开指定的 URL

      - 例如

        - 拨打电话

          ```txt
          NSURL *url = [NSURL URLWithString:@"tel://10086"];
          [[UIApplication sharedApplication] openURL:url];
          ```

        - 发送短信

          ```txt
          NSURL *url = [NSURL URLWithString:@"sms://1383838438"];
          [[UIApplication sharedApplication] openURL:url];
          ```

    - **应用间跳转**

      - 创建两个示例应用，例如 `Test1`和 `Test2`

      - 在`Test2`中实现一个跳转方法

        ```js
        NSURL *url = [NSURL URLWithString:@"myTest://"];
        if ([[UIApplication sharedApplication] canOpenURL:url]) {
            [[UIApplication sharedApplication] openURL:url];
        } else {
            NSLog(@"没有安装应用");
        }
        ```

      - iOS 9.0 及更高版本，需要配置协议白名单（LSApplicationQueriesSchemes），以允许跳转到指定的应用

    - **跳转到指定界面**

      - 为了跳转到指定界面，我们需要在 URL 后面添加一些标记，用于告知被跳转的应用需要打开哪个界面。这涉及到两个应用之间的通信
      - 在被跳转的应用的 `AppDelegate` 中监听代理方法，例如 `application:handleOpenURL:` 或 `application:openURL:options:`

  - 安卓

    - **URL Scheme**
      - 安卓应用也可以注册一个 URL Scheme，用于从浏览器或其他应用中启动本应用。通过指定的 URL，可以让应用直接打开特定页面或执行指定动作
    - **Schema 链接**
      - 安卓应用可以通过 schema 链接来拉起其他应用。直接使用 `window.location.href` 跳转到 schema 链接即可。注意，每个手机应用都有独有的 schema，最好保持 iOS 和安卓的 schema 一致

  - 总结

    - 应用的跳转，通过配置URL Scheme和Schema 链接实现，而==通信==则依赖于==协议==

## DOM元素全屏

- 注：因为全屏后元素内的弹窗等低于全屏层级，导致显示不出来

- 示例

  ```js
  let ele = document.getElementById('reserve');
  // 检测document中是否有正在全屏的元素
  if (!document.fullscreenElement) {
      // 如果没有 则调用 元素身上的方法放大
      ele.requestFullscreen();
  } else {
      // 注: 退出放大时使用的是document身上的方法
      if (document.exitFullscreen) {
          document.exitFullscreen();
      }
  }