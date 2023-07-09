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
- Tips
  - TS中传参数量和形参数量不一致也会报错
  - 函数的返回值，TS也会==自动进行类型判断==

### 类型

- JS中原有的类型：`number`、`string`、`boolean`、`object`、`array`

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

- `unknow`

  - 表示==未知==类型，如`let a: unknow`，后续`a`可以赋值任意类型

  - 不能==直接赋值==其他类型(除了`any`)！

  - 与`any`不同的是：`any`赋值对象也会关闭类型检测，但`unkonw`不会

    ```ts
    let a: unknow = true
    let b: string;
    b = a // 报错 错误原因:unknow类型无法赋值给string类型
    ```

  - Tips

    - `unknow`可以赋值给`unknow`或`any`类型

    - 不能直接赋值，但可以加类型判断后再赋值

      ```ts
      let a: unknow = 'hello'
      let b: string
      if(typeof a === 'string'){
          b = a // 不会报错
      }

#### 联合类型

- 可以用逻辑语句连接==多个类型==
- 如`let a: 'male' | 'female'`，这样`b`赋值`male`或`female`都允许，但是赋值其他值不行
- 又如`let b: number | string`

#### 类型断言

- 告诉解析器，变量的实际类型

  ```ts
  let a: unknow = 'hello';
  let b: string;
  // 语法1: 变量 as 类型
  b = a as string;
  // 语法2: <类型>变量
  b = <string>a