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

  - 用**类似**箭头函数的**语法**，声明函数==结构==、==类型==

    ```ts
    let a: (p1: number, p2: number) => number
    a = function(a, b){
        return a + b
    }
    a = (e: string, b): string =>{} // 报错 赋值时不能更改类型

- `array`

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