## Tips：

- **ES6**中对对象中的函数形式进行了简化，对象中本来应该使用`函数名(键)：function(){ }值`的形式，简化为了`函数名(){ }`

- ES5：for(let i,i<length,i++) → ES6：for(let item of items) 用法类似于v-for 

- for await of：在遍历循环异步方法时，会暂停等前一步循环的代码块执行完才继续执行

- 为什么需要数据代理？就是因为底层的的逻辑不会暴露出你增删改查时的动作，所以只能通过外部自己定义的操作来关联原来的操作，让他执行增删改查时先过一道自己写的程序，从而捕获到这个动作

- ES6新增let和const。var是全局变量，即使是在区块内声明也是作用全局

- 对象中声明函数简化写法：`let t = { fnc(){} }`


## let

- 声明变量

  ```js
  let a
  let b,c,d
  let f = 100, g = []		之前我错误的写过 let f,g=[] 形式，这只能给一个变量赋值！
  ```

- 块级作用域：在`{ let }`内声明的变量无法被父级读取

- 不存在变量提升：

  - ==var==声明的变量，==可以==在==声明前==使用，只不过是==undefined==
  - let声明的变量，不能在声明前使用，会报错

- Tips：

  - **注意**：==for(let index = 0;index<length; index++)==中，let声明的index是在==每一次==循环创建的==块级作用域==中新声明的index


## string扩展

- padStart和padEnd，有两个参数，第一个是要保持的长度，第二个是长度不够的补白字符

## 箭头函数

- 不能作为==构造函数==
- 不能使用[arguments]()变量

## 生成器

- `function * fn(){ }`function关键词后跟==*号==

  - `let obj = fn()`

- 可以使用[yield]()关键词，例：`{/*代码块*/yield 返回值; /*代码块*/yield 返回值;}`

  - 作用：分隔代码，可以理解为小==return==，yield后是==for循环==每次循环得到的结果，有几个yield就将代码分隔为几块

  - 遍历只会返回yield后的语句，但是[生成器对象.next( )]()会yield分隔开的前、后代码块，如

    ```js
    // 普通写法             // 对象内简写1              // 对象内简写2
    function * gen(){      let obj ={                 let obj = {
        console.log(1)       * fn(){                     fn:function *(){
        yield 123;             console.log(1)              console.log(1) 
        console.log(2)         yield                       yield
        yield 456;             console.log(2)              console.log(2)
        console.log(3)        }                          }
    }                       }                          }
    let t = gen()          let t = obj.fn()            let t = obj.fn()
    t.next() // 1          同左                         同左
    t.next() // 2
    t.next() // 3
    t.next() // 空
    ```
  
  - [next( )]()会返回yield后的内容
  
    - 与==for循环==不同的是，for循环拿到的是yield后的结果，比如`yield 123`，那for循环得到的就是123
  
    - 而==next( )==执行返回的是一个对象，比如`yield 123;  obj.next()`返回`{done:false,value:123}`
  
    - ==next( )==也可以==传入实参==，==后一个next('实参')==会作为==上一个==**yield**语句==整体的返回结果==，下例是一个异步执行的定时任务，但是要在前一个异步任务执行完后再执行后一个
  
      ```js
      function oneStep(){
          setTimeout(()=>{
              let data = '第1步'
              g.next(data) 过1秒后执行第二个yield 在第一个yield中调用第二个yield
          },1000)
      }
      function twoStep(){
          setTimeout(()=>{
              let data2 = '第2步'
              g.next(data2)
          },1000)
      }
      function threeStep(){
          setTimeout(()=>{
              let data3 = '第3步'
              g.next(data3)
          },1000)
      }
      function * gen(){
          let t = yield oneStep()
          console.log(t) 第二个next中传入的参数作为第一个**yield整体的返回值** 此处输出'第1步'
          let t2 = yield twoStep()
          console.log(t2) 输出'第2步'
          let t3 = yield threeStep()
          console.log(t3) 输出'第3步'
      }
      let g = gen()
      g.next() 开始执行第一个yield
      ```
  
  - [正确理解yield范围]()
  
    ```js
    /* 代码块1 */
    let t = yield /* 代码块2 */
    /* 代码块3 */
    let t2 = yield /* 代码块4 */
    /* 代码块5 */
    代码块1、2为*第一个yield*的范围
    t、代码块3、4为*第二个yield*的范围
    t2、代码块5为*第三个yield*的范围
  

## Promise

- 本质是==构造函数==，用以处理==异步操作失败或成功的结果==

- ```js
  let p = new Promise(fn1)	Promise接收 函数 作为参数
  function fn1(p1, p2) {		Promise执行时 会往函数中传入两个 函数 作为参数
      setTimeout(() => {
          let data = '数据'
          p1('成功')
          p2('err1','err2')	这两个函数分别可以用来改变 Promise对象 的 状态
      }, 1500)			   第一个参数 表示成功 可以接收 1个 参数
  }					      第二个参数 表示失败 也只能接收 1个 参数
  p.then(success, fail)	    Promise对象的 then函数 也只接收 函数 作为参数 有两个参数
  function success(...p) {	Promise对象状态为成功时会执行第一个参数
      console.log(p)	输出'成功'
  }
  function fail(...p) {		Promise对象状态为失败时会执行第二个参数
      console.log(p)	输出'err1' 只有一个参数传入进来
  }
  ```

- Promise传入的两个函数，是[异步]()执行，函数前后的代码都会执行，**但是**==成功==或==失败==函数执行后，另一个函数就不会执行

  - 即时成功函数先执行，依然会走完后续==主线程步骤==再结束`await`

- 除了then里传入两个函数，还可以用[promise对象.catch(函数)]()接收失败回调方法

  ```js
  //第一种 then里传入两个函数 用来接收 成功和失败的值
  new Promise().then(successDate => successDate, errMsg => errMsg)
  
  //第二种 then只传入一个函数 用来接收成功值 用.catch函数接收失败值
  new Promise().then(successDate => successDate).catch(errMsg => errMsg)
  
  //第三种 then只传入一个函数也没有catch 只能接收成功值
  new Promise().then(successDate => successDate)

- ※`then()`**返回值**还是==Promise对象==

  - **但是**then里的函数如果没有`return`，`then()`返回的是==值为undefined的Promise对象==

  - 如果`then( res => { return res } )`有return，则返回==值为res的Promise对象==

- Promise对象里的值，**只能**通过`p.then( res => {获取res} )`才能获取Promise对象里存的值

- 对比普通回调和Promise.then的区别

  ```js
  普通回调
  function fn1(a) {
      fn2(a, 'b')
  }
  function fn2(...p) {
      fn3(...p, 'c')
  }
  function fn3(...p) {
      console.log(p[0] + p[1] + p[2])	输出'abc'
  }
  fn1('a')
  Promise回调——then方法*返回*的结果还是*Promise对象*，所以可以接着用.then执行后续回调
  let p = new Promise((a, b) => {
      a('a')
  })
  p.then((v) => {
      return new Promise((a, b) => {
          a([v, 'b'])	成功和失败函数只能传入一个参数
      })
  }).then((v) => {
      return new Promise((a, b) => {
          v.push('c')
          a(v)
      })
  }).then((v) => {
      console.log(v[0] + v[1] + v[2])	输出'abc'
  })

## Set集合

- 一种新的数据结构，类似于==数组==，**但是**集合中==每个值都唯一==(仅限于==对象地址值/基本数据类型==)

- ==不能==用`for(let i=0;i<l;i++)`方法遍历，因为

- 属性方法

  - size：返回集合个数，类似于length
  - add：添加一个新元素，并返回新的集合，类似于push
  - delete(索引)：删除索引位置的元素，返回boolean值，类似于splice
  - has(值)：检测集合中是否包含某个元素，返回boolean

- 用例

  - 数组去重：`let t = [...new Set([1,1,2,2,3,4])] // 输出t为[1,2,3,4]`

  - 两个数组求交集：

    ```js
    let a = [1,2,3,4,5,4,3,2,1]
    let a2 = [4,5,5,6,5]
    let result = [...new Set(a)].filter(item => return new Set(a2).has(item)) 输出[4,5]
    ```

  - 并集：`let t = new Set([...a,...a2])`

  - 差集(双方集合中独有的元素)：`let t = [...new Set(a)].filter(item= > return !(new Set(a2).has(item)) )`

## Map

- ==Set==和==Map==的共同点：
  - 都无法用==数组下标==的方式获取数据，只能用==for...of==等遍历方式才能取得里面的内容
  - 自然也只能用api添加元素
- Map是一种新的数据结构。类似于==对象==，里面每个元素都是==键值对==的组合，但是==键==不仅限于字符串，甚至是==对象==都可以作为键。同样也只能用==for...of==等方法遍历取得里面的内容
- 属性方法
  - size：返回Map元素的个数，类似于length
  - set(key，value)：添加一个新元素，并返回新的集合，类似于push
  - ==get(key)==：返回键名对象的键值
  - ==delete(key)==：删除键名对应的元素
  - has(==key==)：检测Map是否包含某个元素，返回boolean值
  - clear：清空集合，返回==undefined==
- ==new Map(参数)==里能接收的初始化参数只有==二维以上的数组==，只会取数组第二层的==前两项==

- **特别需要注意**的是==for...of==遍历出来的每一项都是==数组==。数组长度都是2，第一个元素是key，第二个元素是value。

  - [Object.entries]()就是为了配合Map使用的

    ```js
    let t = {
        a:111,
        b:222
    }
    let t2 = new Map(Object.entries(t)) => 0:{key:'a',value:111}
    						    		1:{key:'b',value:222}
    for(let val of t2){
        val => ['a',111]
    }


## Class

- class是function构造函数的==语法糖==，只是为了结构语法更清晰

  ```js
  function构造函数
  function Person(name){
      this.name = name
  }
  Person.prototype.age = function(){
      console.log(18)
  }
  let hj = new Person('hj')	得到的对象身上有name属性 以及原型身上的age方法
  class类
  class Person2{	注意不能用函数形式
      constructor(name){	构造方法 名字不能修改
          this.name = name
      }
      age(){	注意!只能用这种形式 不能用ES5的对象形式
          console.log(22)
      }
      static bbb = 'bbb'	Person2的实例对象身上是看不到 static关键字声明的静态属性和方法的
                			等同于Person函数对象身上添加属性方法 其实例对象看不到
  }
  let zh = new Person2('zh')	同function构造函数得到的对象一样 但是简化了原型身上添加属性方法
  ```

- ==继承==

  - ES5类继承

    ```js
    function Phone(price){
        this.price = price
    }
    Phone.prototype.fn = ()=>{
        console.log('打电话')
    }
    function SmartPhone(price,color){
        Phone.call(this,price)	此处非常聪明的用call重新矫正this的指向 用当前构造函数的this 相当于在自己实例对象身上
        this.color = color		添加其他构造函数的属性 达到复用的目的
    }
    SmartPhone.prototype = new Phone()	重新定向原型
    SmartPhone.prototype.constructor = SmartPhone 因为原型是实例对象 所以没有constructor 这里要弄一个同名的来替代
    ```

  - Class继承

    ```js
    function Phone(price){
        this.price = price
    }
    Phone.prototype.fn = function (){
        console.log('打电话')
    }
    Phone.prototype.bbb2 = 123
    class SmartPhone extends fn{	必须要用extends关键字
        constructor(price,color){
            super(price) 或 super.fn()	super表示父类的constructor 直接传参表示继承使用父类的属性
            this.color = color			也可以直接.父类方法调用
        }
        fn2(){	此处可以用super调父类方法 但是！不能用super(参数)
            console.log(222)
        }
    }
    let t = new SmartPhone('xiaomi','red')	子类身上就有父类的属性方法了 并且！不用重新定向原型和constructor
    ```

- 父类方法的重写：即在子类上声明跟父类同名的方法

- class的==getter==和==setter==

  ```js
  class tt{
      constructor(aa){	constructor不是必须有的 有get和set同样可以读取、设置属性
          this.aa = aa
      }
      get aa(){
          console.log('读取')
          return 'aaa'	不能写return this.aa 因为会读取aa然后再触发get方法然后无限循环
      }
      set aa(newVal){
          console.log('设置')	同样不能在此处写this.aa=newVal 会无限循环
      }
  }

## Symbol

- ES6提出的一种原始数据类型，跟Number等一样，只能接受字符串作为参数

- 它的值==绝对唯一==，

  - ```js
    let t = Symbol('a')
    let t2 = Symbol('a')
    t === t2 // false
    let t3 = {
        [t]:123
    }
    t3[t] // 123
    t3[Symbol('a')] // undefined
    ```

  - 因为每个值不相等，因此作为对象的属性名，可以保证属性不重名

    - **但是**作为属性名不能用`.`运算符，必须用方括号。因为t是字符串

  - 不会出现在[for...in]()和[for...of]()、[Object.entries]()中

    - 只能用[Object.getOwnPropertySymbols(obj)]()和[Reflect.ownKeys(obj)]()获取

- ==对象自定义遍历==

  - 通常来说对象不能用==for...of==方法遍历，这个方法只能用于数组这种，对象里自带==Symbol.iterator==方法，要想遍历对象就需要添加同样的方法，还可以自定义返回的数据

    ```js
    let t = {
        array:[1,2,3,4],
        [Symbol.iterator](){
            let index = 0
            return {
                next:()=>{	next函数是固定写法
                    if(index < this.array.length){
                        let a = this.array[index]
                        index++
                        return { value:a, done:false }	return的对象也是固定写法
                    }else{
                        return { value:undefined, done:true }	undefined 和 done是固定属性写法
                    }
                }
            }
        }
    }
    ```

- ==登记注册==

  - ```js
    let t = Symbol('a')
    let t2 = Symbol.for('a')
    t === t2 // false
    let t3 = Symbol.for('a')
    t2 === t3 // true

  - 类似单例模式(只有一个实例对象)。首先在==全局搜索==被登记的Symbol是否有==该字符串==作为名称的Symbol值，如果有即返回该值，没有则新建，并登记

  - ```js
    let t = Symbol.for('a')
    Symbol.keyFor(t) // 'a'
    let t2 = Symbol('p')
    Symbol.keyFor(t2) // undefined
    ```

  - 查询一个==已登记==的Symbol的==key==

- Symbol的==值==**不能**与其他类型数据进行运算

  - 不能`Symbol('a') + 10`这样
  - 但可以`object[Symbol('key')] + 1`这样，以Symbol==值==作为对象属性名，取对象中的值进行运算

- ※Symbol==内置值==

  - 如`Symbol.iterator`等，可以作为==对象==的==属性==，从而改变对象在==特定使用场景下的表现结果==。如：使用`Symbol.iterator`改变对象在==for ... of==中的结果
  - 内置值有==固定==的==使用场景==，如`Symbol.iterator`只有在对象被==for . . . of==时才调用，`Symbol.hasInstance`在==其他对象==使用==instanceof==判断是否为==该==对象的==实例==时才调用


## 模块化

- ES6模块化：不同于ES5使用==闭包==做的模块化，ES6使用export、import关键字来导入导出

  - 把想要暴露的变量和方法前加export，例：`export let params = 'xxx'`或者`export function fn(){}`

  - js文件中想要==引入模块==，使用

    - ==注意！==通过此方法引入的模块是==独立作用域==，不能在**其他**==script标签内使用==
    
    ```html
    <script type="module">
        import * as m from 'xxx.js'
        console.log(m)
    </script>
    ```
    
    - **但是**把引入模块和JS文件写到一起就可以了
    
    ```html
    <script src="./index.js" type="module"></script>
    index.js文件内写入
    import * as m from 'xxx.js'
    console.log(m)

  - **只有**==export default==才能在import时省略==* as==

- 在一个**js文件**中想==暴露==和==引入===一些方法和属性，有几种对应形式：

  ```js
  1、通常形式 分别暴露 和 集中暴露
  export let t = 111			 let t = 111
  export function fn(){}		 function fn(){}
  						     export {t,fn}
  import * as 别名 from 'path'	*表示暴露出来的所有内容，别名则代替了所有内容 使用 别名.变量/方法 来使用
  import {t,fn} from 'path'	直接用t、fn调用
  import {t as m,fn} from 'path'	用m来调用t 可以单独给一个变量起别名
  2、default关键词
  export default {
      t:111,
      fn(){default对象内可以取到父级作用域的变量}
  }
  import 别名 from 'path'
  import * as 别名 from 'path'	注意！这种方式导入的模块会多一层default，即 别名.default.t
  import {default as 别名} from 'path'	default不能直接使用必须用别名
  ```

- ==动态引入==：

  - 在需要用到的地方调用`import('地址').then(传入的模块 =>{模块.里面暴露的方法})`

  - import函数，其返回值是promise对象

  - 动态引入`.then`内执行的实际上是`import * as 形参名`，所以`export default`时，需要`形参.default`才能取到里面的属性方法

- 要引入**css文件**，直接使用`**import 路径**`

- ==引入Npm包==：`import 别名 from '包的名称'`即可使用，例：`import $ from 'jquery'`=>`$(body).css('color','red')`


## async、await

- 使==异步==代码像==同步==代码一样执行

- async==函数==

  - 在==函数**声明处**==使用！不能`fn(async this.fn2())`

  - 可以使用`await`关键字

  - **返回值**会自动加工成==Promise对象==！哪怕返回的是基本类型数据，得到的返回值也是Promise对象

    - **但**如果async函数返回给==父级==函数(父级函数也是async)，父级函数用`await`关键字可以把==状态成功==的Promise对象值取出来，即==原数据类型==

      ```js
      async child(){
          return '123'
      }
      async parent(){
          console.log(child()) // 输出 [object promise]{...}
          console.log(await child()) // 输出 '123'
      }

  - promise对象的==状态==是==成功==或是==失败==，由==return==后的值决定

    ```js
    async function fn(){
        return 基本数据类型、对象等	返回结果状态成功
        return new Promise((success,reject)=>{
            success(内容)			  返回结果状态成功
        })
        return new Promise((success,reject)=>{
            reject(内容)			  返回结果状态失败
        })
        throw new Error('xxx')		返回结果状态失败
    }
    let result = fn()
    result.then(value => {},reason => {})
    ```

  - 在对象中声明的函数可以省略function，在函数前加async

    ```js
    // 普通用法
    async function fn(){}
    
    // 对象内
    let obj = {
        async fn(){} // 简写形式
        fn: async function(){} // 完整形式
    	fn: async ()=>{} // 箭头函数形式
    }
    
    // 函数内
    function request(fn){
        fn()
    }
    request(async ()=>{}) // 箭头函数也可使用async关键字
    request(async function(){}) // 匿名函数
    request(async )

- await==表达式==

  - ==必须==写在async函数**里**

  - await后可以跟==Promise对象==，及任意数据类型

    - 但是跟==Promise对象==，会自动帮你把Promise对象中的==值==取出来返回

      - 除了状态==成功==的Promise对象，其余状态不会进行操作，也==不会继续往下执行代码！==且==没有返回值==

        ```js
        async fn(){
            await new Promise((success,fail)=>{}) // 状态为待处理，不会执行后续代码
            await new Promise((success,fail)=>{fail('err')})// 状态为失败，不会执行后续代码
            console.log('后续')
        }

    - 当需要promise返回值时，不能`await let data = new Promise().then(res=>res)`

      - 而是`let data = await new Promise().then(res=>res)`

  - [await]()==返回值==是==promise对象==状态==成功==的**值**，即`.then(success=>data,err=>err)`then成功回调方法==返回的值==data

  - ==await后==跟异步方法返回的==promise对象！==会等该异步方法==执行返回==成功或失败结果后==才会继续==执行后续的代码

  - 要搭配==try...catch==，因为promise对象状态==失败会抛出异常==，影响后续执行

    ```js
    let r = new Promise((success,reject)=>{
        success('xxx')
    })
    async function fn(){
        try{
            let result = await r	注意，必须要跟promise对象 后跟 普通函数 没有作用
        }catch(err){
            console.log(err)
        }
    }
    ```

- Tips

  ```js
  asycn child(){
      // 这样无法等待异步执行完后再返回
      // 因为return 是立即执行的 而这时await还未返回值 相当于 return 空
      // 但因为child是async函数 因此返回的空值也被包装成Promise对象
      return await new Promise((success,fail)=>{
        // 异步操作
        setTimeout(()=>{
            success('yes')
        },2000)
      })
  }
  function parent(){
      console.log(child())// 输出 状态为“待处理”的Promise对象
  }

## 扩展运算符和rest参数

- [rest参数]()：`function (...args)`，只能用在==形参==。rest表示“剩余的部分”，既，如果`function (a, b, ...args)`，那么超出`a、b`个数的==实参==将==组合成**数组**==赋值给`args`变量

  Tips：

  - rest参数必须要放到==形参==列表==末尾==

  - rest参数==只能有一个==

  - 实参给形参的赋值，其实就是`let 形参 = 实参`，只不过看不到赋值过程

  - 用什么包裹rest参数，得到的就是什么样的数据

    ```js
    let a = [1,2,3]
    function fun1( ...args ){
        console.log(args) 输出[[1,2,3]]
    }
    function fun2( [...args] ){
        console.log(args) 输出[1,2,3] 等同于 let [...args] = a
    }
    function fun3( {...args} ){
        console.log(args) 输出{ 0:1, 1:2, 2:3 } 会自动用数组索引作为对象属性名
    }

- [扩展运算符]()：`func(...实参1, ...实参2)`，只能用在==实参==，不能`let a = ...b`，因为展开的是一个==参数序列==，只能用==容器包裹==。扩展，是将确定的东西展开，提取出里面每一个元素，所以可以有多个

  Tips：

  - `func(...实参)`时，实参不能是==对象==！因为==属性==必须用`{}`包裹，不可能`( aa:1, bb:2, cc:3 )`像这样展开。**但是**可以像`func({...对象})`这样传入一个对象进去
  - 只有一个==形参==来接收传入的实参时，**只能**使用[arguments]()关键词
  - 当展开==对象==时，想要修改对象中某属性值，可以`console.log({...对象, 对象.key:新值})`
    - 注意==新值==必须放扩展对象**后**，才能覆盖！`{obj.key:newValue, ...obj}`不能修改

  
  应用：
  
  - ==数组合并==：ES6之前使用[concat]()合并，新方法：`let new = [...array1,...array2,...]`
  - ==展开对象==：`let t = {a:1,b:'qwe} let t2 = {...t} fn({...t})`，**注意**展开对象一定要用=={ }==包裹
  - ==对象合并==：`let t = {a:1} let t2 = {b:'qwe'} let t3 = {...t,...t2}`，会将展开的对象属性取出，实行一层==浅拷贝==

## 解构赋值

- 简单来说就是——用同等结构或数量的参数直接接收要复制的变量。`let { a, b } = obj `等同于`let a = obj.a let b = obj.b`、`let [ a, b ] = array`等同于`let a = array[0] let b =array[1]`

  Tips：

  - 因为==解构==其实就是将==一层==数据==每一个==都单独拎出来==赋值==给另一堆==变量==，所以解构赋值是==深拷贝==，**但是！**如果这一层数据中某属性/数组元素==保存的内容==是另一块内存空间的==地址==，那么就只有这个属性/元素是==浅拷贝==

- [数组的解构赋值]()：`let [ a, b, c ] = 数组`，注意此处左边是`[]`

- [字符串解构赋值]()：字符串在存储空间中跟数组是一样的，所以左边用`[]`。`let [ str1, str2 ] = 字符串`，字符串也可以视作数组，可以获取到每一个字母

- ※[对象的解构赋值]()：`let { key1, key2 } = 对象`，无需调用对象属性就可以取出

  Tips：

  - ==左边==必须跟右边赋值对象中属性==同名==

  - 可以用此方法[删除对象属性]()，用[rest参数]()设定一个形参，就会将没有单独提出来的属性装入新对象

    ```js
    let obj = {
        name: 'xxx',
        age: 23,
        aaa:111,
        bbb:222
    }
    let { bbb, ...new_obj } = obj
    console.log(new_obj) 输出{ name:'xxx', age:23, aaa:111 }

- 对于函数中默认参数·**function (params...)**·，也可以使用解构赋值。使用方式·**function ( {解构参数} )**·，甚至还可以·**function (解构参数：{ parms1,parms2 })**·
- 因为实参传递到形参时，就隐性进行了一次`let 形参 = 实参`操作，所以在==函数定义处==就可以使用==解构赋值==，`function ({ a, b }或[ a, b ])`

## 函数参数的默认值

- 形参默认值，即对应位置==没有实参传入==就使用形参中赋的==初值==

- ==形参==赋默认值

  ```js
  function fn( a = 10 ){
      console.log(a)
  }
  fn() //10
  fn(1) //1
  ```

- 与==解构赋值==结合

  ```js
  function fn( { a = 1, b, c } ){	注意！解构赋值对象 设置默认值 用的不是{a:1}而是 {a=1}
      console.log(a,b,c)
  }
  fn({a:'123',b:123,c:'root'}) // '123',123,'root'
  fn({b:'qwe'}) // 1,'qwe',undefined

## 数据代理

- 在ES6之前使用`defineProperty(要代理的对象, 自命名属性, {get(), set()})`，很笨拙，需要知道代理对象的属性名才能检测数据变化，且需要一个一个设置get和set

- 在ES6之后提出了`proxy`，它不用笨拙的对属性一个个进行代理，而是对整个目标对象代理

  - 使用方法：`proxy(target, {get(target, name), set(target, name, value)}, deleteProperty(target，name))`

    - get函数中有两个默认参数：`target`是==要代理的对象==、`name`是==检测到读取的**属性名**==。所以get中的return要这么写`return target[name]`

    - set函数有三个默认参数：前两个target、name和get函数的相同、第三个`value`是==修改传入的值==
      - ==defineProperty==监测不到劫持对象的==增删==，而==proxy==的set加入了监测==**增**==的能力，当`obj.新属性`时，也可以监测的到

    - ==**delete**Property==函数有两个默认参数，同get函数，**必须**要有返回值，`return delete target[name]`

- Tips：

  - 当省略第三个参数==deleteProperty==时，对象可以==直接==删除操作，当写了==deleteProperty==，**必须**要在==return时==执行删除，不然删除无效，其实就是当你删除操作时，js帮你把删除操作放到了==deleteProperty==函数中执行，**同时**帮你**监测**删除动作

  - 没有使用脚手架时，用`<script/>`标签引入等同于==import==导入，同样都可以用vue的==mixins==等导入外部配置项对象

  - 对数据代理本质的理解：对原始数据的操作是无法监测的，而数据代理就像一道筛网，通过另一个对象来映射原始数据，同时在映射对象身上添加了对应各种操作的拦截方法，从而达到修改映射对象时，既可以修改原始数据，同时还能拦截到操作

## Reflect

- 最大的作用是：不需要`try{}catch(error){}`，最典型的一个例子就是==defineProperty==出错时，整个单线程程序都会挂掉，所以封装的框架中通常都要用==try catch==来捕获错误，同时保证程序正常运行。**而**==reflect==则不用，它在使用==defineProperty==时，会返回==执行成功或失败==，而不会使整个代码都挂掉，**同时**==reflect==身上还有==全套的object对象操作方法==
- `Reflect.deleteProperty(obj, '属性名')`：删除对象身上的属性

## 私有属性

- 类外不能访问，只有内部函数能够访问到

- 需要在构造函数外部声明

  ```js
  class test{
      #p
      constructor(a,b){
          this.a = a
          this.#p = b
      }
      fn(){
          console.log(this.#p)
      }
  }
  let t = new test(1,2)
  t.fn()	输出2
  ```

## 可选链操作符

- 用于==对象==属性==是否存在==的判断

- `对象?.属性`：对象不存在或者对象下属性不存在只会返回`undefined`而不会报错。

  - `?.`可理解为查询下一层是否存在，如果存在就`.`获取

- 使用场景：对象有多层结构，且不确定对象下属性是否存在

  ```js
  function fn(params){
      通常做法1
      this.ip = params && params.xxx1 && params.xxx1.ip
      通常做法2
      if(params){
          if(params.xxx1){
              this.ip = params.xxx1.ip
          }
      }
      有了可选链操作符后
      this.ip = params?.xxx1?.ip
  }
  fn({
      xxx1:{
          ip:'qweqweqw',
          port:8080
      },
      xxx2:{
          ip:'qqqqqq',
          port:4050
      }
  })
  ```

## 大数字

- ES10新增的==原始数据类型==，可以表示任意精度的==整数==，超出2的53次方，`Number`只能表示`-(2^53) ~ 2^53`

- 在数字后加n`1024n`或者`BigInt(2 ** 53)`均可以使数字转换成[BigInt]()类型

- 不能跟`Number`类型的数据进行计算！只能同类型计算，如

  ```js
  let b = BigInt(2**53) // 9007199254740992
  console.log(b + BigInt(1)) // 9007199254740993n
  console.log(b + 1) // 报错
  ```

- `Number`类型超过上下限大小后，再进行加减数字==不会变==

- 丢失数字精度问题

  - 在计算0的小数位计算时，会出现`0.1+0.2 // 0.30000004`这样的问题
  - 原因是JS内部使用==双精度浮点数==类型表示==所有==数字类型

- 因为`BigInt`只能是整数，因此计算小数点后的位数会被==舍弃==

  - `console.log((b*BigInt(10) + BigInt(2.2 * 10)) / BigInt(10)) // 9007199254740994n`

## 幂运算符

- 用`底数 ** 指数`代替`Math.pow(底数, 指数)`