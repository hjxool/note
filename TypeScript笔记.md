## 开发环境

1. 安装NodeJS
2. 安装TS工具：`npm i -g typescript`
3. 创建`.ts`文件
4. 使用`tsc xxx.ts`对TS文件进行编译，输出JS文件

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

### 类型

#### JS中原有的类型

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
    // 其他属性，属性名类型为string，值类型未知
    // 可以添加满足条件的任意属性
    let a: {name: string, [key: string]: unknown}

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


#### TS特有

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

  - Tips

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



#### 联合类型

- 可以用逻辑语句连接==多个类型==
- 如`let a: 'male' | 'female'`，这样`b`赋值`male`或`female`都允许，但是赋值其他值不行
- 又如`let b: number | string`

#### 类型断言

- 告诉解析器，变量的实际类型

  ```ts
  let a: unknown = 'hello';
  let b: string;
  // 语法1: 变量 as 类型
  b = a as string;
  // 语法2: <类型>变量
  b = <string>a
  ```

#### 类型别名

- 对于自定义类型用别名来代替，方便复用

  ```ts
  type type1 = 1 | 2 | 3 | 4
  let a: type1
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
          "allowJs": false,
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
      }
  }
  ```
  
  