## 基础

```dart
// 声明函数
// 与TS不同的是类型声明放在前面
// void fn1 表示函数没有返回值
// int num 表示接收参数类型为int
void fn1(int num) {
    // 打印到控制台 不用console.log
    // $变量 可将变量放到引号当中 同JS中模板字符串`value is ${num}`
    // 语句后必须加; JS中可以省略但是dart不行
    print('value is $num');
}

// 入口文件 程序必须包含main(不能改名)函数 程序会先从main开始执行 同C语言
void main() {
    // 用var关键字声明 类型不固定 的变量
    var num = 11;
    // 用类型作为关键字声明 可以指定变量值类型
    int num2 = 22;
    // 函数调用
    fn1(num2);
}
```

- 命令行中运行`dart .\base1.dart`

### 注释

- 单行注释`// xxx`
- 多行注释`/* xxx */`
- 文档注释`/// xxx`
  - 文档注释支持`markdown`语法，可通过`dartdoc`转成文档

### 变量

- Dart所有都是对象，变量存储的是对象的引用
- 声明变量
  - 明确类型：`int age = 18`
  - 不明确类型：`var age = 18`或`dynamic age = 18`
- 同JS，变量名大小写敏感
  - 如`age`和`Age`是两个变量
- 变量默认值是`null`
  - JS中变量默认值是`undefined`
- 变量不会==隐式转换==
  - 如`null`不会自动转换成`false`

### 常量

- 声明常量
  - `const`
    - 无法将运行时的值赋值给`const`常量，如`const time = Date.now()`会报错
    - 只能赋值==编译==时能取到的值，如`const age = 18`
  - `final`
    - 可以将运行时的值赋值`final`常量，如`final time = Date.now()`成功

## 数据类型

### number

- 关键字

  - `num`
    - 可以表示整数和小数
    - `num age = 18`
  - `int`
    - 必须是整数
  - `double`
    - 表示浮点数，可以是整数和小数，但是整数会带小数位
    - 如`double age = 18`输出`age`为`18.0`

- API

  - [API文档](https://api.dart.cn/stable/3.3.4/dart-core/num-class.html)

  ```dart
  void main(){
      // JS中通过Math.ceil()等调用的API dart中是用数字变量调用
      num n1 = -3.7;
      print(n1.ceil()); // 向上取整 -3
      print(n1.abs()); // 绝对值 3.7
      print(n1.round()); // 四舍五入 -4
      print(10.remainder(4)); // 取余数 2
      
      // 转成字符串 toString 用法和JS一样
      print(n1.toString());
      
      print(n1.toInt()); // 转换成整数 3
      // 不同于JS 可以用数字直接调用API
      print(3.8.toInt()) // 向下取整 3
  }