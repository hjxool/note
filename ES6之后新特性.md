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
          console.log(t) 第二个next中传入的参数作为第一个 yield整体的返回值 此处输出'第1步'
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

- 链式调用思路

  - 将平铺开的回调方法相互关联，比如`fn1`中执行回调`fn2`，`fn2`中执行`fn3`，就可以通过构造对象，`fn1`返回的对象身上记录`fn2`，`fn2`返回对象身上记录`fn3`，从而避免回调地狱，使用函数返回对象的形式平铺需要嵌套的回调方法

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

- Promise传入的两个函数，是[异步]()执行，函数前后的代码都会执行，==状态==**只能**改变**一次**

  - 即时成功函数先执行，依然会走完后续==主线程步骤==再结束`await`

- 除了then里传入两个函数，还可以用[promise对象.catch(函数)]()接收失败回调方法

  ```js
  //第一种 then里传入两个函数 用来接收 成功和失败的值
  new Promise().then(successDate => successDate, errMsg => errMsg)
  
  //第二种 then只传入一个函数 用来接收成功值 用.catch函数接收失败值
  new Promise().then(successDate => successDate).catch(errMsg => errMsg)
  
  //※第三种 then只传入一个函数也没有catch 只能接收成功值
  // then默认接收 成功回调 这点是解决 回调地狱 的关键
  new Promise().then(successDate => successDate)
  

- ※`then()`**返回值**还是==Promise对象==

  - **但是**then里的函数如果没有`return`，`then()`返回的是==值为undefined的Promise对象==
  - 如果`then( res => { return res } )`有return，则返回==值为res的Promise对象==
  - 如果`then(res=>{ return new Promise() })`，则返回==值为新Promise对象的值==，而**不是**promise对象的值为新promise
  - 如果抛出异常`throw new Error('err')`，则返回==值为错误信息==

- ==中断==`.then()`的链式调用

  - `.then()`中`return new Promise(()=>{})`，这样返回的Promise对象状态就是`pending`，就不会继续链式执行

  - 任何一步`.then()`返回的Promise对象状态为==失败==，都会中断后续`.then()`的链式调用，直接进到末尾的失败回调

  - Tips：Promise对象==状态失败==或抛出异常时，没有对应处理失败的回调函数会导致程序报错！

- Promise==异常穿透==

  - 当使用`.then()`链式调用，在链式调用末尾指定失败的回调。前面的`.then()`出现任何异常都会传到末尾的失败回调中处理

  - 思路：`then`中虽然没传失败的回调，但是内部其实设置的默认回调，并通过一层层返回状态失败的Promise对象将错误信息传递到`catch`，`catch`内其实又调用一次`then(undefined,onfail)`方法来处理失败的结果

    ```js
    // 用catch处理                  //用then中的第二个失败回调处理 之前 then的异常
    promiseObj.then(res=>{               promiseObj.then(res=>{
        if(!res){                           if(!res){
           throw new Error('err')              throw new Error('err')
        }                                   }
    }).then(()=>{                         }).then(res =>{
        return promiseObj2                   return 'success'
    }).then(res=>{                        }).then(res =>{
        console.log(res)                     console.log(res)
    }).catch(err=>{                       },err =>{
        console.log(err)                     console.log(err) // 'err'
    })                                    })

- promise对象的==状态==是==成功==或是==失败==，由==return==后的值决定

  - 其实是promise对象实例当中的一个属性值`PromiseState`

  - `catch`或==最后一个==`then`中处理失败回调不能再返回失败的Promise对象或抛出异常！因为`catch`和`then`中处理失败回调是兜底的，再抛出异常程序都无法继续进行。所以`catch`和`then(null,err=>{})`返回的都是状态成功的Promise对象

  - ```js
    // async函数默认返回promise对象
    async function fn(){
      return 基本数据类型、对象等	 // 返回结果 状态成功
      return new Promise((success,reject)=>{
          success(内容)		  // 返回结果 状态成功
      })
      return new Promise((success,reject)=>{
          reject(内容)		 // 返回结果 状态失败
      })
      throw new Error('xxx')  // 返回结果 状态失败
    }
    let result = fn()
    result.then(value => {
        throw 'error'         // 返回结果 状态失败
    }).then(value=>{
        return new Promise((a,b)=>{
            b('error')        // 返回结果 状态失败
        })
    }).catch(reason => {
        return true           // 返回结果 状态成功
    })


- Promise对象里的值，**只能**通过`p.then( res => {获取res} )`或`let res = await p`才能获取Promise对象里存的值

- Promise构造函数(实例身上没有)方法

  - `Promise.resolve(value)`
    - 参数：任意值，包括基本数据类型、Promise对象
    - 返回值
      - 返回一个状态成功/失败的promise对象
      - 参数为基本数据类型、成功的promise对象状态都是成功。失败的promise也会导致`resolve`结果失败

  - `Promise.reject(value)`
    - 参数：任意值
    - 返回值：返回一个状态失败的promise对象

  - `Promise.all(promiseObjArray)`
    - 参数：==多个==promise对象组成的==数组==
    - 返回值
      - 返回一个新的promise对象，==状态==由参数中==所有==promise状态决定，所有promise状态都==成功==才成功，**只要有一个**==失败==就==直接(意味着不会执行后续promise)==失败

      - 状态成功：值由参数中==所有promise**成功**的结果==组成的==数组==

      - 状态失败：值为参数中==失败==的promise的结果(**不是数组**)

      - 因为返回值是Promise对象，且状态有可能失败，因此必须`Promise.all(...).catch()`
    
  - `Promise.race(promiseObjArray)`
    - 参数：同`all`方法
    - 返回值：返回一个新的promise对象，值为==**第一个**==完成的promise的==结果==及==状态==
    - 同样需要用`.catch`方法

- 对比普通回调和Promise.then的区别

  ```js
  // 普通回调
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
  
  // Promise回调——then方法*返回*的结果还是*Promise对象*，所以可以接着用.then执行后续回调
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
  ```

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


## Class类

- class是function构造函数的==语法糖==，只是为了结构语法更清晰

  ```js
  // function构造函数
  function Person(name){
      this.name = name
  }
  Person.prototype.age = function(){
      console.log(18)
  }
  let hj = new Person('hj') // 得到的对象身上有name属性 以及原型身上的age方法
  
  // class类
  class Person2{	// 注意不能用函数形式
      constructor(name){	// 构造方法 名字不能修改
          this.name = name
      }
      age(){	// 注意!只能用这种形式 不能用ES5的对象形式
          // 类中定义的方法可以访问到自身属性
          console.log(this.name)
      }
      // 不想让实例使用的属性、方法 用static关键字 只有类才能看到、使用这些属性方法
      // 等同 Person2.fn = ()=>{}添加属性方法 其实例对象看不到
      static bbb = 'bbb'
      static fn(){
          // 静态方法里调用另一个静态属性可以用this 这里this是类本身而不是实例对象
          console.log(this.bbb)
      }
  }
  let zh = new Person2('zh') // 同function构造函数得到的对象一样 但是简化了原型身上添加属性方法
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
        // 关键 用call重新矫正this的指向
        // 用当前构造函数的this 相当于在自己实例对象身上 添加其他构造函数的属性 达到复用的目的
        Phone.call(this,price)
        this.color = color
    }
    SmartPhone.prototype = new Phone() // 重新定向原型
    // 因为原型是实例对象 所以没有constructor 这里要弄一个同名的来替代
    SmartPhone.prototype.constructor = SmartPhone
    ```

  - Class继承

    - `static`静态属性不会继承到后代类中
    
    ```js
    function Phone(price){
        this.price = price
    }
    Phone.prototype.fn = function (){
        console.log('打电话')
    }
    Phone.prototype.bbb2 = 123
    // 必须要用extends关键字
    class SmartPhone extends fn{
        // 没写constructor时,extends会自动调用父类构造函数并传递赋值
        // 写了constructor时,必须 写super()调用父类构造函数并传参
        constructor(price,color){
            // super表示父类的constructor 直接传参表示继承使用父类的属性
            super(price) // constructor内才能用super(参数)
            this.color = color
        }
        // 可以在类 方法 中用 super.父类方法 调用 但 不能用super(参数)
        // super就表示父类 注:父类指继承链上当前类之前所有继承的类
        fn2(){
            super.fn()
        }
    }
    // 子类身上就有父类的属性方法了 并且！不用重新定向原型和constructor
    let t = new SmartPhone('xiaomi','red')
    ```
    

- 父类方法的==重写==

  - 即在子类上声明跟父类同名的方法。当子类中有方法和父类方法同名，则子类会覆盖父类同名方法

  - 所以子类中一旦写了`constructor()`，其实就是==重写==了父类的构造函数，父类的构造函数丢失，因此**必须**用`super()`调用父类构造函数

- ==存取器==，class的==getter==和==setter==

  ```js
  class tt{
      // constructor不是必须的 有get和set同样可以读取、设置属性
      constructor(aa){
          this._aa = aa // 设置私有属性来中转数据 否则存取器读写同一属性会无限循环
      }
      get aa(){
          console.log('读取')
          return this._aa
      }
      set aa(newVal){
          console.log('设置')
          this._aa = newVal
      }
  }
  ```

- 基类/抽象类

  - 不能被用作创建实例的类，只能用来继承。如`Phone`等

  - 构造函数中用`new.target`可以==指向当前类==，从而避免抽象类创建实例

    ```js
    class Phone{
        constructor(params){
            if (new.target === Phone){
                throw new Error('Cannot create an instance of an abstract class')
            }
            this.name = params
        }
    }


## Symbol、迭代器

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

  - 不会出现在`for...in`和`for...of`、`Object.entries`中

    - 只能用`Object.getOwnPropertySymbols(obj)`和`Reflect.ownKeys(obj)`获取

- 迭代器

  - 迭代器(iterator)是一种==接口==，即一个==函数方法==，接口就是为不同的数据提供统一的操作处理，而迭代器这一接口的作用是提供统一的访问操作

  - 目的：实现自定义遍历方式

  - 原理

    1. 创建一个==指针==对象，指向当前数据结构的==起始位置==
       - 这个对象指用`[Symbol.iterator](){}`函数返回的对象
    2. ==第一次==调用`next()`方法，会自动指向第一个元素
    3. 接下来不断调用`next()`方法，指针一直向后移动，直到最后一个元素
    4. 每次调用`next()`返回一个包含对象`{value: '元素值', done: false或者true}`
       - `done`表示是否遍历完

  - 应用

    - ==对象自定义遍历==

      - 通常来说对象不能用==for...of==方法遍历，这个方法只能用于数组这种，对象里自带==Symbol.iterator(迭代器)==方法，要想遍历对象就需要添加同样的方法，还可以自定义返回的数据

        ```js
        let t = {
            array:[1,2,3,4],
            [Symbol.iterator](){
                let index = 0
                return {
                    // next函数是固定写法
                    next:()=>{
                        if(index < this.array.length){
                            let a = this.array[index]
                            index++
                            // return的对象也是固定写法
                            return { value:a, done:false }
                        }else{
                            // undefined 和 done是固定属性写法
                            return { value:undefined, done:true }
                        }
                    }
                }
            }
        }

- ==登记注册==

  - `Symbol('')`创建的原始值都是不相等的，但是用`Symbol.for('')`可以创建一个共享的`Symbol`，如果存在共享的`Symbol`，则会返回已有的`Symbol`
    
    - 首先在==全局搜索==被登记的Symbol是否有==该字符串==作为名称的Symbol值，如果有即返回该值，没有则新建，并登记
    
    
    ```js
    let t = Symbol('a')
    let t2 = Symbol.for('a')
    t === t2 // false
    let t3 = Symbol.for('a')
    t2 === t3 // true
    
  - `Symbol.keyFor(symbol)`查询一个==已登记==的Symbol的==key==

    ```js
    let t = Symbol.for('a')
    Symbol.keyFor(t) // 'a'
    let t2 = Symbol('p')
    Symbol.keyFor(t2) // undefined

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

  - 注！`default`暴露时，`import xx from path`会帮你把default层里面的值取出来，但用解构赋值`import {key} from path`的方式去取默认暴露的值是不行的


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
  import * as 别名 from 'path'// 注！这种方式导入的模块会多一层default，即 别名.default.t
  import {default as 别名} from 'path'// default不能直接使用必须用别名
  ```

- ==动态引入==：

  - 在需要用到的地方调用`import('地址').then(传入的模块 =>{模块.里面暴露的方法})`

  - import函数，其返回值是promise对象

  - 动态引入`.then`内执行的实际上是`import * as 形参名`，所以`export default`时，需要`形参.default`才能取到里面的属性方法

- 要引入**css文件**，直接使用`import 路径`

- ==引入Npm包==：`import 别名 from '包的名称'`即可使用，例：`import $ from 'jquery'`=>`$(body).css('color','red')`


## async、await

- 使==异步==代码像==同步==代码一样执行

- async==函数==

  - 在==函数**声明处**==使用！不能`fn(async this.fn2())`

  - 可以使用`await`关键字

  - ※`async`函数默认返回一个promise对象，即使没写return也会返回值为undefined的promise对象

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
      ```

    - 如果返回的是一个Promise对象，则==返回值==状态由Promise对象状态决定，且==值==是Promise对象的值(值传递)

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

  - await后可以跟==Promise对象==，及==任意==数据类型

    - 但是跟==Promise对象==，会自动帮你把Promise对象中的==值==取出来返回

      ```js
      async fn(){
          await new Promise((success,fail)=>{}) // 状态为待处理，不会执行后续代码
          // 状态为失败都也会结束await继续往后执行 状态失败必须要有错误处理回调！不然会抛异常
          await new Promise((success,fail)=>{fail('err')}).catch(()=>{})
          // 状态成功 继续往后执行 不需要 成功处理回调！
          await new Promise((success,fail)=>{success('ok')})
          console.log('后续')
      }
      ```

    - 当需要promise返回值时，不能`await let data = new Promise().then(res=>res)`

      - 而是`let data = await new Promise().then(res=>res)`

  - [await]()==返回值==是==promise对象==状态==成功==的**值**

    ```js
    async function fn(){
        let result = await new Promise((resolve, reject)=>{
            resolve('ok')
        })
        console.log(result) // 'ok'
        let result2 = await new Promise((resolve, reject)=>{
            reject('err')
        })
        console.log(result2) // 'err'
    }

  - ==await后==跟异步方法返回的==promise对象！==会等该异步方法执行返回Promise对象状态成功后==才会继续==执行后续的代码

- Tips

  - ```js
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

  - `async`、`await`其实是`生成器函数`的语法糖，本质是将`async`声明的函数体放入生成器函数，`await`的位置相当于`yield`，将`await`前后内容分隔，再在`await`后的Promise对象追加一个`.then(()=>{ genObj.next() })`，从而得到`await`后Promise对象状态成功后才会==往后执行==的现象


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

- ==数组的解构赋值==

  - `let [ a, b, c ] = 数组`，注意此处左边是`[]`

  - 如果想取数组中某几位的元素`list = [1,2,3,4,5]`

    - 用`,`号占位：`let [, , c, , e] = list`就可得到`c=3 e=5`

    - 通过数组下标：`let [2:c, 4:e] = list`

  - 设置==默认值==：`let [list2='为空'] = list`，当`list`为空则`list2`为默认值，不为空则==拷贝==`list`

  - 遍历==Map==

    ```js
    let map = new Map()
    map.set('key1', 'xxx')
    map.set('key2', 'xxx2')
    map.set('key3', 'xxx3')
    for(let [,val] of map){
        console.log(val) // 输出'xxx' 'xxx2' 'xxx3'
    }

- ==字符串解构赋值==

  - 字符串在存储空间中跟数组是一样的，所以左边用`[]`。`let [ str1, str2 ] = 字符串`，字符串也可以视作数组，可以获取到每一个字母

- ※对象的解构赋值

  - `let { key1, key2 } = 对象`，无需调用对象属性就可以取出


  - ==左边==必须跟右边赋值对象中属性==同名==

  - 可以用此方法[删除对象属性]()，用[rest参数]()设定一个形参，就会将没有单独提出来的属性装入==新对象==

    ```js
    let obj = {
        name: 'xxx',
        age: 23,
        aaa:111,
        bbb:222
    }
    // 使用rest参数
    let { bbb, ...new_obj } = obj
    console.log(new_obj) // 输出{ name:'xxx', age:23, aaa:111 }
    // 可以在解构赋值时重新命名
    let {name:newName, age:newAge} = obj
    console.log(newName, newAge) // 输出'xxx' 23
    // 如果是数组中取值作为命名
    let keys = ['name']
    let { [keys[0]]:newName } = obj // 注:属性名要加中括号
    // 如果解构的对象没有对应属性
    let {eee: newEee = 333} = obj // obj内没有eee，则newEee为新值
    
    // 还可以嵌套解构
    let obj2 = {
        a: {
            b:{
                c: '111'
            },
            d: {
                e: '222',
                f: '333'
            }
        }
    }
    let {a: {d: {f: newF}}} = obj2
    // 等于
    let {d: {f: newF}} = obj2.a
    let {f: newF} = obj2.a.d
    let newF = obj2.a.d.f

- 对于函数中默认参数`function (params...)`，也可以使用解构赋值。使用方式`function ( {解构参数} )`，甚至还可以`function (解构参数：{ parms1,parms2 })`
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

## proxy数据代理

- 在ES6之前使用`defineProperty(要代理的对象, 自命名属性, {get(), set()})`，很笨拙，需要知道代理对象的属性名才能检测数据变化，且需要一个一个设置get和set

- 在ES6之后提出了`proxy`，它不用笨拙的对属性一个个进行代理，而是对整个目标对象代理

  - `Object.defineProperty`监测不到劫持对象的==增删==，而==proxy==的set加入了监测==**增**==的能力，当`obj.新属性`时，也可以监测的到
  - ※Proxy不是==深拷贝==，只是==浅拷贝==，即如果是多层结构的对象，代理这个对象==无法检测==到里层属性值的变化

  ```js
  let obj = {
      a: 1,
      // Proxy代理数据能捕获到数据变化的只有第一层 此处代理的是b 地址值
      b: {
          c: 2
      }
  }
  // proxy代理普通对象
  let proxy_obj = new Proxy(obj, {
      get(target, key, receiver){
          // target 目标对象，即obj
          // key 要访问的键名，如obj.a，key就是'a'
          // receiver 代理对象或继承代理的对象，本例中是proxy_obj
          console.log('触发读取')
          return target[key]
      },
      set(target, key, value, receiver){
          // value 修改属性传入的值
          // receiver 通常是代理对象，但也有可能set方法在原型链上，或以其他方式间接调用，因此不一定是代理对象
          // 如xx.name='Alex'，xx不是代理对象，且自身不含name属性，但它的原型链上有一个代理对象，那么这个代理对象的set()会触发，此时receiver接收到的是xx而不是代理对象
          console.log('触发修改')
          target[key] = value
          // 应当返回一个boolean值 true表示修改成功 在严格模式下false会抛出异常
          return true
      },
      // 拦截删除对象属性操作
      // 必须 要有 返回值
      deleteProperty(target, key){
          return delete target[key]
      }
  })
  // 代理函数对象
  function fn(a, b) {
      return a + b
  }
  function create_proxy(obj, config) {
      return new Proxy(obj, config)
  }
  let config = {
      // 拦截函数调用
      apply(target, thisArg, params){
          // target 目标函数
          // thisArg 调用者
          // params 调用函数传入的参数数组
          console.log(thisArg)
          return target(...params) * 10 // 结果乘10
      }
  }
  let p = {
      fn: create_proxy(fn, config)
  }
  console.log(p.fn(4, 5)) // 90 thisArg为对象p
  // 拦截new操作符
  function fn(a) {
      this.age = a
  }
  let p = new Proxy(fn, {
      // new newTarget时触发 必须要返回一个对象
      construct(target, params, newTarget) {
          // target 目标对象
          // params new实例时传入的参数数组
          // newTarget 最初被调用的构造函数 本例中是p
          // 不能用new newTarget不然会死循环一直触发construct
          return new target(...params)
      }
  })
  console.log(new p(55)) // {age: 55}
  // 拦截in操作符
  let obj = {
      name: 'xxx',
      age: 22
  }
  let p = new Proxy(obj, {
      has(target, prop) {
          // target 目标对象
          // prop 检测是否存在的属性名
          if (prop in obj) {
              // 没有return 默认返回false
              return true
          } else {
              return false
          }
      }
  })
  console.log('age' in p) // true
  ```

- 对比`Object.defineProperty`

  ```js
  let obj = {a: 1}
  let proxy_obj = {}
  Object.defineProperty(proxy_obj, 'a', {
      get(){
          console.log('触发读取')
          return obj.a
      }
  })

- `new Proxy(obj, handle)`还有以下处理函数

  - [defineProperty()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Proxy/Proxy/defineProperty)：拦截对对象的`Object.defineProperty`操作

  - [Object.getOwnPropertyDescriptor](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Proxy/Proxy/getOwnPropertyDescriptor)：`Object.getOwnPropertyDescriptor`调用劫持

  - [getPrototypeOf()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Proxy/Proxy/getPrototypeOf)：拦截对象**原型**的读取操作

  - [isExtensible()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Proxy/Proxy/isExtensible)：拦截对对象的`Object.isExtensible()`

  - [ownKeys()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Proxy/Proxy/ownKeys)：`Object.getOwnPropertyNames`和`Object.getOwnPropertySymbols`的调用劫持

  - [preventExtensions()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Proxy/Proxy/preventExtensions)：对`Object.preventExtensions()`的拦截

  - [setPrototypeOf()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Proxy/Proxy/setPrototypeOf)：拦截`Object.setPrototypeOf()`

- ==可撤销==代理

  ```js
  let obj = {
      name: 'sss'
  }
  // 通过Proxy函数对象上的方法创建
  let revocable = Proxy.revocable(obj, {
      // 配置方法同new Proxy
  })
  // 返回值不同于new Proxy
  // 代理对象在proxy属性中，并且有revoke函数
  console.log(revocable) // {proxy: proxy, revoke: revoke()}
  // 创建的代理
  let proxy_obj = revocable.proxy
  // 撤销代理
  // 一旦撤销就不能恢复 与它关联的目标对象以及代理对象都有可能被垃圾回收
  // 重复revoke()不会有任何效果，也不会报错
  // 撤销后任何 拦截操作 都会抛出异常
  revocable.revoke()

- Tips：

  - 当省略`deleteProperty(){}`时，对象的删除操作，其实有一个默认的`deleteProperty`执行

  - 没有使用脚手架时，用`<script/>`标签引入等同于==import==导入，同样都可以用vue的==mixins==等导入外部配置项对象

  - 对数据代理本质的理解：对原始数据的操作是无法监测的，而数据代理就像一道筛网，通过另一个对象来映射原始数据，同时在映射对象身上添加了对应各种操作的拦截方法，从而达到修改映射对象时，既可以修改原始数据，同时还能拦截到操作

  - `Proxy`没有`prototype`原型对象，因为，其实例对象**仅仅是**目标对象的一个**代理**，不需要初始化属性和方法

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