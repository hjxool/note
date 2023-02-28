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
    function * gen(){
        console.log(1)
        yield 123;
        console.log(2)
        yield 456;
        console.log(3)
    }
    let t = gen()
    t.next() // 1
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
          // p1('成功')
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

- 在一个**js文件**中想`**暴露**`和`**引入**`一些方法和属性，有几种对应形式：
  1. 通用形式：`import * as 别名 from "文件路径"``
  2. ``export 对象；export function`或者`export{ 对象，函数名 }`——`import {暴露的对象和方法名} from 路径`
  3. 引入默认暴露时不能直接用default表示引入的对象，必须用`as 别名`
  4. `export default { 写入属性和方法 }`——`import {default as xxx} from 路径`或者`import xxx from 路径`
     - `import {xxx as aaa,obj1,fnc} from '../xxx.js'`
  5. 动态引入：在需要用到的地方调用`**import('地址').then(传入的模块 =>{模块.里面暴露的方法})**`，import函数，其返回值是promise对象
  
- 要引入**css文件**，直接使用`**import 路径**`

## 扩展运算符和rest参数

- [rest参数]()：`function (...args)`，只能用在==形参==。rest表示“剩余的部分”，既，如果`function (a, b, ...args)`，那么超出`a、b`个数的==实参==将==组合成**数组**==赋值给`args`变量

  Tips：

  - rest参数必须要放到==形参==列表==末尾==

  - rest参数==只能有一个==

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
  function fn( { a = 1, b, c } ){
      console.log(a,b,c)
  }
  fn({a:'123',b:123,c:'root'}) // '123',123,'root'
  fn({b:'qwe'}) // 1,'qwe',undefined

## 数据代理

  在ES6之前使用·defineProperty(要代理的对象，自命名属性，{ get()，set() })·，很笨拙，需要知道代理对象的属性名才能检测数据变化，且需要一个一个设置get和set

  在ES6之后提出了·**proxy**·，它不用笨拙的对属性一个个进行代理，而是对整个目标对象代理

​    使用方法：

​      1、·**proxy(target，{ get(target，name)，set(target，name，value) }，deleteProperty(target，name))**·

​      2、get函数中有两个默认参数：`**target**`是·**要代理的对象**·、·**name**·是·**检测到读取的属性名**·，·**string类型**·。所以get中的return要这么写·return **target[name]**·

​      3、set函数有三个默认参数：前两个target和name和get函数的相同、第三个·**value**·是·修改传入的值·

​        Tips：原版·defineProperty·监测不到劫持对象的·**增删**·，而·**proxy**·的set加入了监测·**增**·的能力，当使用·**obj.新属性时**·，也可以监测的到

​      4、deleteProperty函数有两个默认参数，同get函数，**必须**要有返回值，返回·return **delete target[name]**·

​        Tips：

​          1、当·**没使用**·deleteProperty函数时，对象数据是可以直接删除操作的，当写了以后，**必须**要在·**return时**·执行删除，不然会删除失败，其实就是当你删除操作时，js帮你把删除放到了·**deleteProperty**·函数中执行，**同时**帮你**监测**删除动作

​          2、没有使用脚手架时，用·**<script/>**·标签引入等同于·**import**·导入，同样都可以用vue的·**mixins**·等导入外部对象配置项

​          3、对数据代理本质的理解：对原始数据的操作是无法监测的，而数据代理就像一道筛网，通过另一个对象来映射原始数据，同时在映射对象身上添加了对应各种操作的拦截方法，从而达到修改映射对象时，既可以修改原始数据，同时还能拦截到操作

## Reflect

- 最大的作用是——不需要·**try{}catch(error){}**·，最典型的一个例子就是·defineProperty·出错时，整个单线程程序都会挂掉，所以封装的框架中通常都要用·try catch·来捕获错误，同时保证程序正常运行。**而**·**reflect**·则不用，它在使用·defineProperty·时，会返回·**执行成功或失败**·，而不会使整个代码都挂掉，**同时**·**reflect**·身上还有全套的object对象操作方法
- [Reflect.deleteProperty(obj,  '属性名')]()：删除对象身上的属性