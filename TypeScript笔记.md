## 开发环境

1. 安装NodeJS
2. 安装TS工具：`npm i -g typescript`
3. 创建`.ts`文件
4. 使用`tsc xxx.ts`对TS文件进行编译，输出JS文件
   - 编译==单个文件==的指令，**都**==不会应用==`tsconfig.json`中的配置

## 基本类型

- JS中只有==值==有类型，==变量==是没有类型的
- 声明类型必须是小写！

### ==变量==声明方式

- 只声明未赋值
  - 如`let a: number`，表示声明一个变量，并指定它的类型
- 声明的同时赋值(不常用)
  - 如`let b: boolean = true`
- 未指定类型直接声明赋值(常用)
  - 如`let c = false`，TS会==自动对变量进行类型限制==，后续修改变量时就只能用同类型的值

### ==函数==声明方式

- ==形参==声明类型
  - `function fn(a: number, b: number){...}`
- 函数==返回值==声明类型
  - `function fn(a: number, b: number): number{...}`
- ==箭头函数==返回值声明类型
  - `(a: number, b: number): number => {...}`

- Tips
  - TS中传参数量和形参数量不一致也会报错
  - 函数的返回值，TS也会==自动进行类型判断==

## JS中原有的类型

- `number`

- `string`

- `boolean`

- `object`

  - `let a: object`和`let a: {}`没区别，但`let a: {name:'hh'}`就变成`字面量`类型

  - 一般用于指定对象下属性类型、个数，对象结构必须==完全一致==

    ```ts
    let a: {name: string}
    a = {name:'hhh'}// 只有值为对象 且有属性name 且name值为字符串才不会报错
    a = {name:'hhh', age:18} // 多了属性也会报错
    let b = {name: string, age: number}
    b = {name:'hhh'} // 少了属性也会报错
    // 属性后加?号，表示属性可选
    let c: {name: string, age?: number}
    c = {name:'hhh'} // 不会报错
    c = {name:'hhh', age:'10'} // 报错 可选属性类型也必须一致
    ```

  - (常用)对象中只对部分属性限制，可以添加任意其他属性时

    ```ts
    // 表示name属性必须有
    // [key: string]: unknown是索引签名
    // 表示除了name 其他属性 属性名类型为string 值类型未知
    // 可以添加满足条件的任意属性
    let a: {name: string, [key: string]: unknown}
    
    // 一般用接口定义对象属性类型
    interface A {
        name: string;
        [key: string]: unknown;
    }
    // 然后再声明对象类型为A 并赋值
    let a: A = {
        name: '张三',
        age: 18
    }
    ```

  - 类存取器声明的属性符合`object`结构类型。即只有`public`、`get xx`声明的属性才能用作`object`结构类型

    ```ts
    class A {
      // 声明类型并赋初值
      private _a: string = 'asd'
      get aa() {
        return this._a
      }
    }
    let t: { _a: string } = new A()// 报错 实例不符合{_a:string}结构类型
    let t: { aaa: string } = new A()// 报错 实例不符合{aaa:string}结构类型
    let t: { aa: string } = new A()// 正确 实例符合{aa:string}结构类型

- `function`

  - 函数结构的类型声明：用**类似**箭头函数的**语法**，声明函数==结构==、==类型==

    ```ts
    let a: (p1: number, p2: number) => number
    a = function(a, b){
        return a + b
    }
    a = (e: string, b): string =>{} // 报错 赋值时不能更改类型
    a = (a) =>{} // 不报错 可以少传参数
    a = (a, b, c) =>{} // 报错 不可以多传参数

- `array`

  - 声明语法：`类型[]`，数组中==不能==出现==声明类型之外==的元素

    ```ts
    let a: string[] // 声明字符串数组
    // 声明语法2
    let a: Array<number> // 声明数字类型的数组


## TS特有

- 字面量

  - 直接用==值==作为类型声明，如`let a: 10`，限制类型为`10`，`a = 11`都会报错

- `any`

  - 表示任意类型，如`let a: any`，后续`a`可以赋值任意类型

  - 隐式写法：只声明不赋值`let a`

  - 注：`any`类型的变量赋值其他变量时会扰乱类型检测

    ```ts
    let a: any = true
    let b: string;
    b = a // 不会报错

- `unknown`

  - 表示==未知==类型，如`let a: unknown`，后续`a`可以赋值任意类型

  - 不能==直接赋值==其他类型(除了`any`)！

  - 与`any`不同的是：`any`赋值对象也会关闭类型检测，但`unkonwn`不会

    ```ts
    let a: unknown = true
    let b: string;
    b = a // 报错 错误原因:unknown类型无法赋值给string类型
    ```

  - `unknown`可以赋值给`unknown`或`any`类型

  - 不能直接赋值，但可以加类型判断后再赋值

    ```ts
    let a: unknown = 'hello'
    let b: string
    if(typeof a === 'string'){
        b = a // 不会报错
    }
    ```
  
- `void`

  - 表示空值，以函数为例`function fn(): void{}`，表示没有返回值的函数，声明为`void`后，函数中再`return 任意值`都会报错

- `never`

  - 表示永远不会返回结果，如抛出异常，程序会立即结束执行，不会有返回值

    - JS函数默认返回值为`undefined`，属于`void`，不属于`never`

    ```ts
    function fn(): never{
        throw new Error('err')
    }
    ```

- 元组`tuple`

  - ==固定长度==的数组。语法：`[类型, ...]`

    ```ts
    let a: [string, string]
    a = ['a', 'b']
    a = [1, 'v'] // 报错 元素类型不符
    a = ['a'] // 报错 数组长度不符
    a = ['a', 'b', 'c'] // 报错 数组长度不符
    ```

- 枚举`enum`

  - 用变量存储值，用的时候只需要对比变量，和用变量表示类型，编译时会自动转换成对应值

    - 语法：`enum 枚举对象名{ 枚举项 = 对应值, ...}`

    ```ts
    enum Gender{
        male = 1,
        female = 0
    }
    let a: {name: string, gender: Gender}
    a = {
        name: '柯洁',
        gender: Gender.male // 直观的表示0、1含义
    }
    console.log(a.gender == Gender.male) // 用于判断
    // 对比联合类型声明方式
    let a: {name: string, gender: 0|1} // 表示只能取0或1 但是0和1代表的意思不够直观

- HTML元素`HTMLElement`
  - 值类型为HTML元素

## 联合类型

- 可以用逻辑语句连接==多个类型==
- 如`let a: 'male' | 'female'`，这样`b`赋值`male`或`female`都允许，但是赋值其他值不行
- 又如`let b: number | string`

## 类型断言

- 告诉解析器，变量的实际类型

  ```ts
  let a: unknown = 'hello';
  let b: string;
  // 语法1: 变量 as 类型
  b = a as string;
  // 语法2: <类型>变量
  b = <string>a
  // 函数类型
  let c = a as () => void // 表明a是函数 且 不接收参数 且 没有返回值
  ```

## 类型断言和:指定类型区别

- `:`号

  - 在==初始定义声明时==使用，确保代码一致性和可读性

  ```ts
  let a: string
  ```

- 类型断言

  - 在==变量或方法使用时==使用，用于解决类型推断问题，如：与外部库交互时，无法推断出使用时的类型，就需要用断言

  ```ts
  let a:any = 'aaa'
  // 在变量a使用时 断言它就是string 并取length属性
  let b:number = (a as string).length
  ```

## 类型别名 type

- 对于自定义类型用别名来代替，方便复用

  ```ts
  type type1 = 1 | 2 | 3 | 4
  let a: type1
  ```

## 接口 interface

- 定义一个类的==标准结构==，包括有哪些==属性==和==方法==。所有属性、方法都不能有==实际的值==。类似抽象类

  ```ts
  // 接口
  interface customClass{
      name: string;
      fn(): void;
  }
  // 定义类实现接口 implements关键字
  // 可以增加自定义属性方法 但必须满足接口属性方法
  class Person implements customClass{
      name: string;
    	age: number;
    	constructor(a: string, b: number) {
      	this.name = a
      	this.age = b
    	}
    	fn(): void {
      	console.log(111)
    	}
  }
  ```

- 可以作为类型声明使用。与==类型别名==一样

  ```ts
  let obj: customClass = {
      name: 'xxx',
      age: 11,
  }
  ```

- 同名接口可以重复定义，使用时按全部同名接口结构叠加。==类型别名==不能重复定义

  ```ts
  interface customClass{
      key1: unknown;
      key2: boolean;
  }
  // 必须是全部同名接口累加结构 否则会报错
  let obj: customClass = {
      name: 'xxx',
      age: 11,
      key1: 'asda',
      key2: false,
  }
  ```

- ※==类型导入/导出==

  - 导出类型

  ```ts
  interface Aa {
      aa: number
  }
  export type {Aa}
  ```

- 导入类型

  ```ts
  // import type 为TS特有
  import type {Aa} from './store.ts'
  let t = computed<Aa>(() => store.getters)
  ```

## 类型别名 与 接口 区别

- 相同点

  - 都可以用于定义==对象==结构

  ```ts
  type PersonType = {
    name: string;
    age: number;
  };
  
  interface PersonInterface {
    name: string;
    age: number;
  }
  ```

  - 都可以==继承==其他类型

  ```ts
  // 使用类型别名扩展 使用 =号 和 &符
  type EmployeeType = PersonType & { employeeId: number };
  
  // 使用接口继承 使用 extends 关键字
  interface EmployeeInterface extends PersonInterface {
    employeeId: number;
  }
  ```

- 区别

  - 声明合并
    - `interface`可以进行声明合并，多次声明同一个接口，TS会将它们合并
    - `type`不支持声明合并，会报错

  ```ts
  interface Person {
    name: string;
  }
  interface Person {
    age: number;
  }
  // 合并后包含 name 和 age
  const p: Person = { name: "Alice", age: 30 };
  ```

  - 联合类型和交叉类型
    - `type`可以创建联合类型和交叉类型，`interface`则不能直接创建

  ```ts
  // 联合类型
  type StringOrNumber = string | number
  // 交叉类型
  type PersonWithAddress = PersonType & { address: string }
  ```

  - 元组和其他类型
    - `type`可以用于定义元组、基本类型的别名和其他更复杂的类型

  ```ts
  type StringArray = string[];
  type NumberTuple = [number, number, number];
  type Callback = (a: number, b: number) => void;
  type aa = 'success' | 'error' | 'warning';
  ```
  
  - 匿名类型
    - `interface`只能用于定义对象的结构
    - 而`type`可以用于基本类型或联合类型
  
  ```ts
  type Alias = string; // 基本类型别名
  ```

## 配置

- 命令行

  - 文件保存修改自动编译：`tsc xxx.ts -w`
    - `-w`表示`watch`
  - 根目录下有配置文件，在命令行使用`tsc`指令一键编译目录下所有ts文件
    - 同样可以`tsc -w`监视所有ts文件变更

- 配置文件`tsconfig.json`

  ```json
  {
      // 指定哪些文件需要被编译
      "include": [
          // 路径 **表示任意目录 *表示任意文件
          "./ts/**/*"
      ],
      // 指定哪些文件不需要被编译
      "exclude": [],
      // 指定从哪个文件下继承配置
      "extends": "./configs/base",
      // 指定被编译文件列表 
      "files": ["xx.ts"],
      // 编译器选项
      "compilerOptions": {
          // 指定ts被编译的版本 默认ES3
          "target": "es6", // esnext表示最新版本
          // 指定要使用的模块化规范
          "module": "es6",
          // 指定项目中要使用的库 默认在浏览器环境运行不需要设置
          "lib": ["dom"],
          // 编译后的文件输出目录
          "outDir": "./js",
          // 编译后代码合并为一个文件
          // 仅支持"module":"amd"或"system"
          // 合并同名的全局变量会报错
          "outFile": "./js/main.js",
          // 是否对JS文件进行编译 默认false
          "allowJs": true,
          // 是否对JS文件检查语法规范 默认false
          "checkJs": false,
          // 是否移除注释 默认false
          "removeComments": false,
          // 不生成编译后的文件 默认false 一般用于检查代码
          "noEmit": false,
          // 编译有错误时不生成文件 默认false
          "noEmitOnError": true,
          // 严格检查全开或全关
          "strict": true,
          // 编译后文件是否使用严格模式 默认false
          "alwaysStrict": false,
          // 不允许隐式的any类型 如let a;或(a)=>{} 默认false
          "noImplicitAny": true,
          // 不允许不明确类型的this 如(this: Window)=>{return this} 默认false
          "noImplicitThis": false,
          // 严格检查空值 默认false
          // 如获取页面元素有可能是空 用if判断或者obj?.key的形式
          "strictNullChecks": true,
          // 指定TS如何从 import xx from 模块 中查找文件 默认classic
          // 默认根据相对路径从js文件中找模块 像"npm i vue" "import xx from 'vue'"就无法解析
          // 所以最好设置为Node
          "moduleResolution": "Node"
      }
  }
  ```

- 配置文件遇到的问题

  - webpack打包时，入口是ts文件，却提示没有找到输入，且`tsconfig.json`文件报错
    - 原因：没有配置`"allowJs": true`

## class类

### 与ES6不同点

- TS在class最外层定义属性，在`constructor(){}`构造函数中赋值。而ES是在`constructor`中定义并赋值

  ```ts
  class Person{
      name: string;
      constructor(a: string){
          this.name = a
      }
  }
  ```

- TS在class外层定义属性时，可以用**变量声明方式**设置初始值，然后根据值类型自动进行类型限制

  ```ts
  class A{
      aaa = 'aaa'
      bbb = 222
  }
  let a = new A()// 实例属性等于初始值
  ```

- JS中class可以在==构造函数==中给参数设默认值，但TS不行

  ```ts
  // JS中可以
  class A {
    constructor(a = 10) {
      this.a = a
    }
  }
  let a = new A()
  // TS中不行
  class A {
    // 或是 constructor(a = 10)也不行
    constructor(a: number = 10) {
      this.a = a
    }
  }
  let a = new A()
  // TS中普通函数可以设置默认值
  function fn(a: string = 'aaa'){return a}

- 声明静态属性。ES6中没有类型声明

  ```ts
  class Person{
      static name: string;
  }
  ```

- TS中存取器`set`不能声明返回值类型

  ```ts
  class A{
      // 报错 set不能声明返回值类型
      set key(val: number): void{...}
  }
  ```

- TS中类可以当作类型声明

  ```ts
  class A{
      xxx: string
  }
  let a: A = {xxx:'asd'}
  ```

  - 但是有`private`等属性时==不能用==这种方式

    ```ts
    class A{
        protected xxx: string
        private xxx2: string
    }
    // 不写属性会报错缺少类中声明的属性 写了会报错私有属性不能在类外使用
    // 所以有私有、被保护属性时，写不写都有问题
    let a: A = {
        xxx: 'aaa',
        xxx2: 'bbb'
    }
    ```

  - 静态属性`static`可以用这种方式

    ```ts
    class A {
        static xxx: string = 'sss'
        xxx2: string
    }
    let a: A = {xxx2: 'qqq'}
    ```

### TS中特有==只读属性==关键字readonly

- 生成对象后只读，不能修改属性


```ts
class Person{
    readonly name: string;
}
```

- 关键字可以叠加使用

  ```ts
  class Person{
      static readonly name: string;
  }
  ```


### TS中特有==基类==关键字abstract

- 声明为==抽象类/基类==的类**不能**用来创建对象，专门用来继承

- ==抽象类==中**才可以**声明==抽象方法==，即没有内容的函数，执行内容**必须**由派生类决定，派生类**必须**对抽象方法进行==重写==

  ```ts
  abstract class Phone{
      name: string;
      constructor(name: string){
          this.name = name;
      }
      // 定义抽象方法 用 abstract 关键字
      abstract callSomeOne(): void;
  }
  class SmartPhone extends Phone{
      color: string;
     	constructor(name: string, color: string){
          super(name);
          this.color = color;
      }
      callSomeOne(): void{
          console.log('重写')
      }
  }
  let p = new Phone('诺基亚') // 报错 基类无法创建实例
  ```

### TS特有==修饰符==

#### public

- 公共属性，可以在任意位置访问(包括子类)修改，定义属性时的默认值。如`class xx{name: string}`等于`class xx{public name: string}`

#### private

- 私有属性，只能在==当前类内部==进行访问修改。但是可以通过定义方法暴露==属性值==的方式**访问**

  ```ts
  class Person{
      private _name: string;
      private _age: number;
      constructor(a: string, b: number) {
          this._name = a;
          this._age = b;
      }
      // ES class关键字get、set存取器
      get name() {
          return this._name
      }
      set name(value: string) {
      	this._name = value
    	}
  }
  let a = new Person('aaa', 111)
  a.name = 'bbb'
  console.log(a.name)
  ```

#### protected

- 受保护属性，只能在**当前类**和**子类**==内部==访问修改

  ```ts
  class A{
      protected key: string;
      constructor(a: string) {
          this.key = a;
      }
  }
  class B extends A{
      fn() {
          return this.key; // 子类中可以访问
      }
  }
  let a = new A('a')
  a.key // 报错 不能访问
  let b = new B('b')
  b.key // 报错 同样不能访问
  ```

### TS class语法糖

- 注：使用语法糖`public`不能省略

  ```ts
  class Person{
      constructor(public _name: string, private _age: number) {}
  }
  // 等价于
  class Person{
      private _name: string;
      private _age: number;
      constructor(a: string, b: number) {
          this._name = a;
          this._age = b;
      }
  }

## TS特有==泛型==

- 定义==函数==或==类==时，遇到==类型不明确(※)==的可以使用泛型

  ```ts
  // <>内xxx就是泛型名称 声明类型也是xxx
  function fn<xxx>(params: xxx): xxx{
      return params
  }
  fn(10) // 不指定泛型,ts可以自动判断类型
  fn<string>('text') // 指定泛型类型,传入值类型必须与指定泛型相同
  ```

- 可以同时指定多个泛型

  ```ts
  function fn<aa,bb>(p1: bb, p2: aa): aa{
      return p1
  }
  fn(1, '2') // 同样可以自动判断类型
  fn<string, number>('1', 2) // 指定多个泛型
  ```

- 泛型可以通过继承==类==或==接口==指定范围

  ```ts
  class A{
      constructor(public length: number){}
  }
  interface A{
      length: number;
  }
  // extends是固定写法,即使实现接口也不能用implements
  function fn<T extends A>(p: T): number{
      return p.length;
  }
  ```

- 泛型和`:string`类型声明的区别

  - 简单来说，泛型是==动态的类型声明==，拓展性更强，`:`类型声明是把类型写死

  ```ts
  // 泛型声明动态类型
  // 注意 其中T只是占位符 不是固定写法 也可用A X等字符代替
  function fn<T>(arg: T): T {
      return arg
  }
  // 使用时动态指定类型
  fn<string>('hello')
  fn<number>(123)
  
  // 接口使用泛型
  inerface Box<A> {
      xxx: A
  }
  let box: Box<string> = {xxx: 'xxx'}
  ```

## 函数参数

### 基本传参方式

```ts
function add(a: number, b: number): number {
    return a+ b
}
```

### 可选参数

- 使用?号标记

```ts
function fn(name: string, age?: number): string {
    return age? `${name}的年龄为${age}` : `hello,${name}`
}
```

### 默认参数

- 参数的默认值，如果调用时未传递该参数，则使用默认值

```ts
function fn(name: string, age: number = 30): string {
    return `${name}的年龄为${age}`
}
```

### 剩余参数

- 用`...`接收不定数量的参数，参数会被收集到一个数组里

```ts
// 声明一个数组 元素类型为number
function sum(...list: number[]): number {
    // 从0开始累加
    retrun list.reduce((pre, cur) => pre + cur, 0)
}
```

### 命名参数

- 通过对象解构来传递参数，使代码更加清晰和易于维护
- 用`{}`包裹参数
- 不同于Dart，TS中参数指定类型不是必须赋初值

```ts
// 方式1
function fn(name: string, {age: number = 30}): string {}
// 也可以结合其他传参方式 如可选参数
function fn(name: stirng, {age?: number})
fn('张三', age: 18)

// 方式2 用接口
interface Address {
    city: string
}
interface Person {
    age: number;
    address: Address; // 可以嵌套
}
function fn(name: string, {age, address: {city}}: Person): string {}
```

## 函数重载

- 定义多个函数签名，以处理不同类型或数量的参数

```ts
// 方式1
// 定义重载签名
function fn(): void // 不接收参数 无返回值
function fn(name: string, age: number): string // 接收参数 返回值为字符串
function fn(num: number): number
// 实现函数
// 注意！因为有不同重载形式 因此实现函数具体内容时参数要用?号表示可选参数
// 有不同类型返回值 或 同一参数位置有不同类型参数时
// 函数的实现必须用|号全部覆盖
function fn(nameOrNum?: string | number, age?: number): void | string | number {
    if(typeof nameOrNum == 'string' && typeof age == 'number') {
        // 对应fn(name: string, age: number): string的实现
    } else if(typeof nameOrNum == 'number') {
        // 对应fn(num: number): number
    } else {
        // 对应fn(): void
    }
}

// 方式2 用接口
interface GetInfo {
    (name: string): string;
    (age: number): string;
}
// 用这种方式必须用变量来接收函数 因为要声明类型
let fn: GetInfo = function (value: string | age): string {}
// 箭头函数形式
let fn: GetInfo = (value: string | number): string => {}
// 对象中 必须再用一个接口来声明
interface Obj {
  fn: GetInfo;
}
let obj: Obj = {
  // 简写形式
  fn(value: string | number): string {
    return ''
  },
  // 箭头函数形式
  fn: (value: string | number): string => {
    return ''
  },
  // 匿名函数形式
  fn: function (value: string | number): string {
    return ''
  }
}
// 不能在对象中声明的同时创建函数 如
let obj = {
    // 会报错 因为:号已经被占用了
    fn: GetInfo: (value: string | number): string => {
        return ''
    }
}
```

## 复杂类型的声明

- `Promise`

  - 返回值或参数是`Promise`对象时，声明类型要用`<>`表示值的类型

  ```ts
  interface http请求 {
    (a: string): Promise<string>; // Promise对象值类型为string
  }
  // Promise对象值类型为数字
  const fn:Promise<number> = new Promise((resolve) => {
      resolve(11)
  })
  ```

- 数组

  - 复杂对象数组

  ```ts
  interface User {
    name: string;
    age: number;
    address: {
      street: string;
      city: string;
      zipcode: string;
    };
  }
  // 返回值类型为数组 且 数组元素结构类型符合User
  function fn(): User[]
  ```
