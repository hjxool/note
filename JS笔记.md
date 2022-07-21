## Tips：

- 当**需要**给类型相同的元素**循环绑定函数**时，可以用给元素**添加**自定义标签等方法**标记**，在**触发**事件函数**时**，**获取标记**，传递给同一个事件函数，而不需要创建多个同类函数；已经生成的元素跟它之前所在的数组就没有关系了，不能反推出在之前数组所在的位置
- 拖拽排序的时候，设置“目标位置 != 当前”，可以使程序只执行一次；并且要注意有没有接触到外边框，设置条件在外边框不触发；排序逻辑：因为使用的前插入方法，所以小的插在大的后一个的前面，大的插在小的前面；排序判断条件当前索引大小和目标索引大小做比较;计算索引逻辑：不断循环用前一个节点赋值(‘=’)，直到前一个节点为空，累计计算的自定义变量值就是节点的索引值
- 对象.自定义属性 = xxx相当于将xxx赋值给对象下新创建的属性
- 为了避免残留值，应当在**“即将”**调用和请求的地方清空再赋值。
- 滑块必须用clientX/Y计算鼠标到浏览器边框的距离，因为只有键鼠**事件对象**是**动态**值（随鼠标动态改变），而offsetLeft等都是**元素对象**的属性，是**静态**值；还有mousedown事件包裹mouseover最好不用点击的对象作为调用者，最好用window，因为鼠标滑动过快，超出点击的对象就会出现问题
- “e.target”不会随着冒泡移动，点的是哪个元素，指向的就是哪个，即使父元素冒泡执行方法，target指向的也是点的元素，所以要用**“currentTarget”**可以获取**绑定事件**的对象，而不是点到的对象
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
- `**阻止冒泡**`只能阻止`**同名的事件！**`，比如@click.stop，只有父级同样是@click事件才能阻止
- `**冒泡事件的妙用！**`——外层包裹mousedown(不要用click，因为鼠标抬起也会触发一次，其次`**onmousedown**`和`**onmouseup**`是**一对**，onmousedown事件本身并不好用，会在自动获取焦点等情况触发奇怪的问题)显示和隐藏功能，内层则是点击发送指令事件(用同样的关键字`**mousedown**`)，就可以触发指令后隐藏选项框，而不需要遮罩
- 对象取用属性不只用点，还可以用·**[ '属性名' ]**·，当取用的·属性名只有后缀不同时·，可以使用·**[ `前缀${动态后缀}` ]**·
- **折叠注释**·//#region ...代码块... //#endregion·
- **移除事件监听**时，传入两个参数，第一个是想解绑的事件名，第二个是**指向同一处地址**的方法**回调函数**
- **不同层**级的**js**文件中的函数**作用域**以**引入**js**的html**文件为**根目录**
- js里**有return**的表示需要立即执行，在html页面引用的时候就要**加括号**
- ==switch==语句==case==只会执行==一次==！如果`case`匹配中了且==后面没有break==，则后面的case不再执行匹配
- 当使用==！=排除某些条件时==要用[&&]()并列，当使用====匹配某些条件时==，要用[||]()
- ==null==并不是什么都没有，而是一个==空对象==，==undefined==才是什么都没有的未定义数据
- ==同时声明==：`let a=1,b=2,...`等同于`let a=1,let b=2`
- ==基础==数据类型没有==引用地址==，作为实参传入，即使修改了原值也不会改变

------

自定义属性**必须以data-开头**，取值通过**dataset[' ']**获取（注意:data-my-name在取值时要写成myName）

小技巧：把<section>默认设为隐藏，每一个<section>绑定一个唯一的ID，把导航栏自定义属性值设置为<section>ID，再通过dataset获取，此时用判断语句判断点击的是否是对应导航栏，就将其样式设为显示

## 运算符

- [%取余数]()

- 隐式类型转换：`num = num+“”等同于String(num)`，同理`string = +string等同于Number(string)`

- `Number()`等方法不会转变原值，都是依靠返回值赋值给自身

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
    - 在js中`\u2620`是==十六进制==表示，到了HTML标签中就得是`<span/>&#9760;(十进制)`(注意&#和；结尾)。通过计算器计算

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

------

typeof返回数据类型；NaN的返回值是number，NaN是个特殊数字

null是空对象，返回的也是object

------

事件监听器：window.addEventListener('load', init, false);

function init(){ }

在绑定事件方法时：循环**变量**是**传递不进**函数的，**而是**会把参数当作**事件触发的对象**

## 防抖：

使用“underscore”插件，使用时在“**created**”函数中用变量承接“**_.debounce(执行函数，防抖时间)**”函数，再在watch等需要使用的地方调用这个**变量**

​    Tips:

​        1、想要给**防抖**执行的方法中**传入参数**，可以曲线救国，将参数**传入防抖**函数，**再**在防抖函数中**传给里面**的方法

​    核心在于“**闭包**”，用函数包裹函数的形式可以形成闭包，就能形成作用域链，即使重新调用生成了另一个计时器，也能通过闭包的作用域链找到之前函数的变量(定时器)，并清除。

​    思路：频繁调用的场景下，每次都设一个定时器，当小于定时再次调用时，会根据作用域链找到之前的定时任务，并清除同时再设一个新的定时任务，从而达到刷新计时的效果，当大于定时再调用，之前的定时任务已经执行完毕，变量重新回归null

![](https://upload-images.jianshu.io/upload_images/6322775-5b5f7fcbc417ab58.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

函数防抖

![](https://upload-images.jianshu.io/upload_images/6322775-42930d48acd99bc6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

防抖简化版，因为核心理念就是清除、定时、清除、定时，如果超过时间，则定时任务完成后“timer”自动回归为null

## 节流：

每隔一段时间执行一次

​    思路：利用闭包记录下每次调用时的时间，并计算当前时间和上一次时间的差值大于固定值时就执行，否则跳过，并继续叠加时间，直到差值大于固定时间

![img](https://upload-images.jianshu.io/upload_images/6322775-967503d0ac497b27.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

函数节流（在初次调用时执行，滑动停止时立即停止）

​    上面这个实现思路有个问题，使用的Date记录的是系统时间，即上一次停止调用时开始累计，当时隔很久再次调用时会**立即触发**而**不是重新**计算从滑动开始到结束的时间段，所以使用**标识符**改进

![img](https://upload-images.jianshu.io/upload_images/6322775-f9fa0cfa41e6b00b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

<center>每次重新开始计算时间，延后停止

## 作用域

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

- this出现的地方**一定**是在==函数中==，指向的也一定是一个==对象==，由它的==调用者==决定

  - TIps：
    - this找最近的函数`{}`，这个函数**被谁调用就指向谁**，

    - **但是**函数在哪创建==作用域==就在哪，**不会**随着调用者而改变(即对象内调用一个方法，也是去声明函数的地方找它的作用域，找不到的变量就去作用域上级查找，很有可能就是去找全局作用域)

    - 对象内的方法是箭头函数，则箭头函数内的this指向对象父级的调用者。使用var _this = this绑定可以解决

    - 使用`let obj = new 构造函数()`创建的对象`obj`就是this指向的对象

- 不同函数的this区别如下：

  - 普通函数：调用时被决定。
    - 谁调用我，我的this就指向谁
    - js中this是根据函数的**执行环境**动态绑定的：全局函数中this指向的是window；当函数作为某个对象的方法被调用时，this就等于那个对象
  - 箭头函数：**定义时**被决定(因为没名字，所以不能像普通函数那样由对象调用，只能在定义处调用)
    - 箭头函数的this是父级(作用域链的上一层)的this，**没有则指向window**
  - 匿名函数：谁调用指向谁
    -  而**多数情况**下，是由`windows方法中调用`，因此this指向window；此处解释一下为什么是window，因为不同函数方法或者全局定义的匿名函数，是由**JS引擎**各个模块分别**管理**，从**一开始**就已经定义好了this指向

## 自执行函数

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
  
- ```js
  (function (){}) 是一个 函数对象 通过在后面的括号进行传参 (function (){})(params1,...)
  ```
  
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

## 原型对象

- 访问一个对象时，会==先==在==自身==寻找，如果没有，则会去==原型对象==寻找
- 当创建[构造函数]()时，可以将==实例身上共有==的属性和方法添加到==构造函数==的原型对象中，这样既可以每个对象都取到，也不会==影响全局作用域==
- ==构造函数==之上还有==Object==原型对象，原型链就到头了

## JS原生方法：

### Array方法：

- push(‘ ’) 数组末尾**添加**元素，并**返回新**的**数组长度**

- pop( )**删除**数组最后一个元素，并**返回被删**的**元素**

- *unshift**(‘ ’) 数组头部**添加**元素**

- **shift**()数组头部**删除**第**一个**元素，无参数

- [splice(起始位置，删除个数，一个或多个元素)]()：从起始位置开始(**包含**)，删除指定个数的元素。并在起始位置==前==添加元素，并返回**被删元素**组成的**数组**（注：==会改变原数组==）

  TIps：

  - “删除个数”不写，**默认**是**删除**起始位置之后的**所有**元素、如果“删除个数”写0，一个都不删除，插入的元素在**起始位置前**
  
- [slice(start,end)]()：截取==包括==开始到==不包含==结束的数组元素并作为一个新数组返回

- [join(可选：'符号')]()：将**数组**合并为一个**字符串**返回，参数可传自定义符号，在字符串中用自定义符号拼接元素

- [reverse]()：反转数组中元素顺序，会==改变原数组==

- “**some((当前元素的值) => { return 条件 } )**”：检查数组中是否有某元素，返回的是boolean值

- **“filter( (当前元素的值) => { return 条件} )”**过滤器，返回指定条件的元素组成的数组，不会改变原数组；“当前值”是Arry中每一个元素，因为filter其实执行了一次遍历，将Arry中每一个元素拿出来跟return中的判断条件进行比对，再将符合条件的元素合成一个新数组

- include：检测数组中是否包含指定内容

- **forEach( (当前值，索引)=>{ } )：**foreach中回调的函数是数组中每一个元素都调用一次

- `reduce( (前一次返回值，当前数组元素内容)=>{ return 统计 }，初始值 )`： 

- “**for(let i of array)**”：遍历数组

- **“indexOf('检索值'，'起始位置')”：**检索数组中是否有**指定元素**。只检索元素中部分内容是不行的；元素类型不对也是不行的

- “Object.keys(数组)”：返回对象**键**组成的数组

- [sort(function(a，b){a-b或b-a})]()：回调函数遵循几个个规则：(==改变原数组==)

  1. a在数组中的位置==必定==在b==前面==

  2. 返回值==大于==0，元素==交换==位置
  3. 返回值==小于==0，位置==不变==
  4. 返回值==等于==0，认为元素相等，位置==不变==
  5. 因此[a-b]()大于0则a，b交换位置，是==升序==；而[b-a]()，后面的元素减前面的元素，大于0则b，a交换位置，是==降序==
  
- [concat(source，...)]()：拼接多个数组及元素

###   string方法：

- ==字符串==在底层是以==数组的形式==保存，所以可以使用大部分数组的方法 

- [indexOf('检索值'，'起始位置')]()：检索字符串中是否有**指定内容**，有则返回**位置索引**，没有则返回**-1**；同时还必须注意，检索“空字符串 ' ' ”时，返回的值是“0”，因为所有字符串在开头都拥有空字符串(**也可以用于数组**)，==但不能使用正则去匹配==

- [match]()：`string.match(正则表达式)`。根据正则查找并返回**匹配的内容**，以**数组形式**返回
  
  - Tips：`\`是==转义字符==，用`"我说：\"今天天气不错\" "`输出出来就是`我说："今天天气不错"`
  
- [replace(string，new string)]()：用新字符串替换指定字符，或者用新字符串替换==正则匹配==的字符。该方法==不会改变原字符串==
  
  Tips：
  
  - 只会替换==第一个匹配==的字符
  
  - 要替换全部匹配字符，需要用[replaceAll]()或者※`replace(/正则/全局匹配，"替换内容")`
  
- [substring(start，end)]()：截取从==开始索引(包含)==到==结束索引(不包含)==的字符串

- [slice]()：同数组

- [split('分隔符'/正则表达式)]()：以什么分隔就传入对应的字符或者正则对象，会以这个字符或规则截断原字符串，组成==数组==，数组中不会有作为分隔的字符

  Tips：
  
  - 如果传入空字符串`""`，则拆分每个字符
  - `split("a")`或`split(/[A-z]/)`

### object方法：

- “Object.keys(对象)”：返回对象**键**组成的数组
- “Object.values(对象)(es8)”：返回对象**值**组成的数组
- "Object.entries()"：通常对象是不可直接遍历的，而通过`entres`可以返回一个·键值对·组成的数组(**二维数组，第一层是有多少个键值对，第二层是键和值组成的数组**)，从而可以在es6中的新for循环·**for(let i of 可迭代变量)**·中使用，使用方式·for(let **[key,value]** of Object.entres())·，·[key,value]·是es6新提出的·**解构赋值**·
  - Tips：·**Array**·也可以使用，只不过是以数组索引作为·key·
- "Object.assign(目标对象，源头对象)(es6)"：用于对象合并，合并同类赋值，不同类的作为新增，返回值是·**新的目标对象**·。但也可以用·**扩展运算符**·更简洁
  - Tips：在**vue**中，提前准备好的属性在合并后依然具有响应性

- “**for(let i in object)**”：遍历对象
- **操作符**(等同与或符)**：delete**——“delete obj.property”删除对象上的指定属性，有返回值，返回删除成功与否
- [hasOwnProperty]()：使用`xxx in obj`会寻找obj==原型链==上的属性方法，而==hasOwnProperty==只会查找`obj`身上的属性方法

###   window方法：

- window方法可以省略`window`关键字，直接写后面的方法
- 弹窗：警告窗alert('xxx')；确认框confirm('显示内容')，返回布尔值；提示框prompt('提示内容'，'预设内容')，返回的是一个字符串
- **history**.pushState/**replaceState**(state,title,url)可以在**不跳转页面**的情况下**改写**当前页面的url
- **跳转页面**：window.location.**href** = "地址?键值对"，跳转页面后使用`**location.search**`获取到`？`之后的内容，对获取的内容进行字符串分割，创建对象obj = { }，使用**obj['key'] = '值'**，将分割好的键值对装入对象
- sessionStroage本地存储：使用sessionStroage.自定义变量名 = 值，就可以将**值**存入**自定义变量**名，取用的时候直接用**sessionStroage.自定义变量名**就可以获取值；删除的时候使用sessionStroage.removeItem("自定义变量名")
- [prompt(string)]()：类似==alert()==，传入一个==字符串==作为提示文字，会弹出一个输入框，输入的内容作为`prompt函数`的返回值

### document方法：

- [cookie]()：
  - 设置cookie：document.cookie = "user=hou jie"；
  
  - 默认情况下浏览器关闭就清除cookie，添加保存时间： document.cookie = "user=hou jie;  expires=Thu, 18 Dec 2043 12:00:00 GMT（此处时间是new Date()）"，通过new Date()中的setDate()可以**设置**当月中的**某一天**，用getDate()**获取当天**日期；
  - 删除cookie的原理：将expires 参数设置为当前时间或者前几天即可；
  - 同名的cookie会被覆盖，新加的cookie不会覆盖会加到string尾部;
  - cookie.match时`=`后面的[^;]+必须用括号括起来才会返回等号后面的值


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
- [apply(obj，[...args] )]()：同[call]()，但第二个参数是==实参==组成的==数组==，==传入==函数时已经被==展开==，并==依次==赋值给形参

------

jQuery的$("类名/标签/Id").eq(N-1)方法返回**被选元素**的**指定**索引号**的元素**

------

object.className是**覆盖**class属性；object.classList 是**添加**属性或者删除已有属性

------

getElements打头的元素获取方法返回的是集合，就算只有一个元素也是集合的形式，所以没有appendChild方法

------

drag事件中，想要**drop**必须阻止**dragover**的默认操作

------

getBoundingClientRect()方法可以获取目标大小及**相对于窗口**的位置

而transform是相对于自身**当前**位置

------

切换按键一类添加样式时：在异步执行里设置当前点击样式，然后在异步(.then)外执行清除当前点的样式

------

监听事件时，只有固定格式的onclick=function(e){}或者click((e) => {})及最外层function可以获取到event

------

js**原生**事件处理函数事件名必须叫“event”

**onclick = function(e)**和**addEventListener**可以用其他标识表示event，但不能不传参；并且**只能传入**event参数，如果要**传入其他参数**，需要进行**封装**

![img](https://upload-images.jianshu.io/upload_images/6322775-8524511c83b9d754.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

封装

总结：在HTML事件处理程序中可以直接传入多个参数，并且可以不传入event直接传参，非常看重event关键词，可以直接使用；在"οnclick=function"以及addEventListener中不再看重event关键词，可以传任意标识，但只能传event。**简单来说**就是“html页面”要写就写完整“event”，“js页面”可以用任意标识符表示event

**addEventListener**和**on事件**的区别是：对同一个DOM，on事件唯一，但是事件监听器可以添加多个，并先后执行，不会后一个覆盖前一个

------

字符串和数字累加：因为任何东西都可以转成字符串，所以js在自动判断时，会把数字转换成字符串

------

和**服务器双向传递数据**可以用SSE(Server-Sent Event)：

创建SSE对象 var source =new EventSource(url)；

通信刚建立触发的事件 source.onopen =function (event)

服务器发来的数据 source.onmessage

通信错误时回调方法 source.onerror

结束通信 source.close();

通信传递文件流触发事件 source.addEventListener

------

日期格式化或者数字替换：padStart(保持的长度，'用什么来填充')和padEnd(保持的长度，'用什么来填充')

------

## js对象(数组)

- 区分==对象==和==数组==：`xxx.constructor === Array/Object`或者`xxx instanceof Array/Object`或者`Array.isArray(xxx) //true或false`

- [对象的两种取值/定义方式]()：`obj.key `和 `obj["key"]`，但是如果key是一个变量（传进来的参数），就只能用obj[key]（注意少了引号）

  - ```js
    obj = {
        ["key"]: value
    }
    因为“属性名”本质上————就是一字符串

  - `array[字符串]`时字符串会转为数字，对于不能转为数字的才会报错

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

- [对象中定义函数方法]()：必须用`key:function(){ }`或者`key:()=>{ }`的形式，调用的时候则是`obj.key()`

- 数组和对象本质上是一样的，只不过因为存储方式不同，数组比对象效能高，所以对象可以`obj.xxx = ''`进行赋值，数组也可以给==不存在的索引==添加元素

  Tips：

  - 建议用`array[length] = ''`的形式而不是`array[index]`直接去添加，因为未定义索引和已知索引之间的元素会用`null`填充
  - ※数组[length]()是==可以被修改==的，当length==大于原长度==，则会用==null==填充，当length==小于原长度==，则==删除==多出的元素(用这个方法来删除多出的元素)，==但是Vue==无法通过这种方式检测到数据修改
  - [字符串]()和数组==存储方式==是一样的，都是通过==索引==来存储一个个字符(元素)

## Date对象

- [new Date()]()：会返回代码执行时的时间，也可以传入固定格式参数`12/2/2011 11:10:30`，指定时间
- [getDate()]()：获取当前日期==几号==
- [getDay()]()：获取当前==星期==，返回==0-6==的值，==0表示周日==
- [getMonth()]()：获取当前==月份==，返回==0-11==的值，==0表示1月==
- [getFullYear()]()：获取当前==年份==

## Math对象方法：

- [Math.floor]()：向下取整(floor意为地板)

- [Math.ceil]()：向上取整(ceil意为天花板)

- [Math.round]()：四舍五入。等同于Math.floor(x+0.5)，因为能四舍五入进一位的数加上0.5一定可以进一位

- ※[Math.random()]()：生成==**包括**0~**不包括**1==的随机数

  Tips：

  - 生成==X~Y==的随机数：`Math.round( Math.random() * ( Y - X ) + X )`（思路是：0~1乘一个数扩大范围，再加一个数确保最小值，但是要保证不超限，所以乘的数等于差值）

- [Math.pow(底数，指数)]()：求幂运算
- [Math.sqrt]()：开方运算
- [Math.max/min(a，b，c，...)]()：返回最大/最小值


------

console.log(i++)表示**先输出**再自增

![img](https://upload-images.jianshu.io/upload_images/6322775-05d9548814b77e92.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

setTimeout会将里面的函数先放入**任务队列**，4秒后执行，此时for循环已经执行完了(因为属于全局，优先执行)，var定义的全局变量i=5，setTimeout里的函数再接着i=5，继续累加(因为作用域链中只保留i的索引，等到执行的时候再去找当前值)

有回调函数的就是闭包；

**要理解闭包，关键就是理解作用域链**

任务队列会保存下来作用域链中的原始值，但不进行计算

------

for循环是在下一次循环执行前再自增

------

## 元素在页面中的位置

![img](https://upload-images.jianshu.io/upload_images/6322775-d2bce26f79b3c151.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![img](https://upload-images.jianshu.io/upload_images/6322775-0cf168f634ec329d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

offsetTop/Left是相对父级（注：滚动显示容器里，里面的每一个元素它的offset父级都是显示容器），并且会显示(※)**累计滚动隐藏部分**；

​    offset返回的是绝对值，哪怕设置的是百分比，返回的也是px；offsetHeight是自身

![img](https://upload-images.jianshu.io/upload_images/6322775-6dd4b0d7e07d70c9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

scrollHeight代表包括不可见部分的元素高度（只包含padding和content，即使是在IE盒模型下也不会计算border的值）

![img](https://upload-images.jianshu.io/upload_images/6322775-3a3c4a65808c7cb1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

scrollTop（滚动条向下滚动的距离）

![img](https://upload-images.jianshu.io/upload_images/6322775-3e30a11ad6d49ba4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

getBoundingClientRect()求元素距视窗边距

### Tips：

  1、想横向滚动，可以用“@wheel”，此事件在**鼠标滚轮**滚动时触发，然后手动设置，事件触发一次横向滚动距离

  2、wheelDelta为正则是滚轮向上，为负则向下

  3、※**offset偏移量**是依据**上一级定位元素(position:relative等)**确定的

  4、※·**offsetParent**·和·**parentNode**·的区别：

​    ·**offsetParent**·

​      查找路径：有带有·**position**·属性的上一级→带有position属性的上一级。(body虽然没设置position，但是因为所有元素都脱离不了body文档流，所以总能找到body是最外层)

​      最外层：body→null

​    ·**parentNode**·

​      查找路径：·**不考虑position**·，单纯的父级元素往上查找。

​      最外层：body→html→document→null

------

## object.defineProperty(对象名，'键'，{value:值})：

- 为指定对象添加属性；并且该属性不可被遍历

![img](https://upload-images.jianshu.io/upload_images/6322775-b5e3b3b6e236d059.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- defineProperty定义的属性默认不可被修改、检索

- defineProperty的好处：
  1. js默认没有数据绑定，js不会帮你做这个事情，定义过的变量用变量名赋值给另一个属性后，不会随着原先的变量改变而改变。但是defineProperty中的**get函数**可以将其**绑定**
  2. get函数作用是**双向**绑定(**改原始值**会跟着变动)，**get函数**本身不会传入参数，它“return”返回的是外面定义好的变量，所以当**set函数**修改了外部定义的变量，get中**返回值**也就变了；set函数是**改对象属性**时触发，修改后**原始值**也会发生改变(原始值会发生改变是因为修改值对原始值进行了**赋值**操作，**再触发**的**get函数**)；使用set函数时必须也要有get函数，不然set修改完值后，没有对应的属性接收
  3. 数据代理：通过另一个对象来操作原先的对象属性，通过get、set函数实现

------

## 下载：

1. “window.location.href = down_url”下载会打开一个空白页，体验不好

2. download属性可有可无；添加·**a.target = '_blank****'**·在新建空白页打开更好

![img](https://upload-images.jianshu.io/upload_images/6322775-8601768fc123b84c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

“创建a标签”，会直接下载体验较好

##  JSON方法：

  `JSON.stringfy`：将JS值(数组、对象等)转换为JSON字符串

  `JSON.parse`：将JSON字符串转换为JS

## 浅拷贝和深拷贝：

  [浅拷贝]()：只复制对方的引用地址，并不会开辟新的内存空间，修改复制值原值也会变。包括：引用类型的**赋值**、

​    Tips：当·**扩展运算符**·中的数据有·**引用类型**·就是浅拷贝，

  [深拷贝]()：复制内存中存储的值，并开辟一块新内存空间用于存储。包括：ES6的·**Object.assign**·、·**扩展运算符{ ... }**·、迭代获取基本数据类型进行赋值

​    Tips：虽然ES6的两种方法可以实现深拷贝，但因为堆栈的性质，只能进行一层深拷贝，如果对象下还有对象，则只会进行浅拷贝

  “**传值和传址**”：·**基本**·数据类型的·**等号赋值**·是传值，而对象这种·**引用**·数据类型则是·**传址**·

  “**基本数据类型**”：undefined、number、string、null

  “**引用数据类型**”：对象、数组、函数

----

## DOM对象操作

- [classList]()：虽然是个只读对象，但是拥有特殊方法修改
  - `add('class1',...)`，往对象身上添加一个或多个class，同名的不会添加
  - `remove('class',...)`，移出一个或多个class，移出不存在的class不会报错
  - `toggle('class'，true/false)`，true添加类名，false移除类名