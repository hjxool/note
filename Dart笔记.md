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