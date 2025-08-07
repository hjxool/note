## 文档

- [dart生态依赖包](https://www.pub.dev)
  - `pubspec.yaml`类似JS项目中的`package.json`

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

    - 明确类型声明的变量，在==使用前==**必须**赋初值

      ```dart
      var i;
      print(i); // null
      List arr;
      List arr2 = [...?arr]; // error

  - 不明确类型：`var age = 18`或`dynamic age = 18`

    - ==不明确类型可以不赋初值==
    
    ```dart
    // 未赋初值时 可以随时改变值
    var a;
    a = 1;
    a = 'asd';
    dynamic b;
    b = 1;
    b = 'qwe';
    
    // var 赋了初始值 类型就已经确定 不能再改变
    var a = 1;
    a = 'asd'; // error
    
    // dynamic 赋了初值 也可以改变后续赋值类型
    dynamic a = 1;
    a = 'asd'; // 可以运行
    
  
- 同JS，变量名大小写敏感
  - 如`age`和`Age`是两个变量

- 变量默认值是`null`
  - JS中变量默认值是`undefined`

- 变量不会==隐式转换==
  - 如`null`不会自动转换成`false`

- Dart中类型是明确的，所以没有`===`号，只有`==`号

### 常量

- 声明常量
  - `const`
    - 无法将运行时的值赋值给`const`常量，如`const time = Date.now()`会报错
    - 只能赋值==编译==时能取到的值，如`const age = 18`
    - 用`const`声明时，**必须**==赋初值==
  - `final`
    - 可以将运行时的值赋值`final`常量，如`final time = Date.now()`成功
    - `final`声明时，可以不赋初值，**但是**赋过值后都不能再修改

- 声明常量，可以和变量类型一起使用

  ```dart
  // 声明数字类型常量
  final num x = 1;
  
  // 不能和var一起使用 但是可以和 dynamic 一起用
  const var x = 1; // 报错
  final var x = 1; // 报错
  const dynamic x = 1; // 可以使用
  final dynamic x = 1; // 可以使用
  
  // final 可以不赋初值
  final x;
  final num x;
  // const 必须赋初值
  const x; // error
  const String x; // error

### 正则

- 不能像JS中`/^abc$/`这样，必须用`RegExp()`方法

- 示例

  - Dart中正则必须要加`r`，如`RegExp(r'\w+')`

  ```dart
  void main() {
      // 基本
      var p = RegExp(r'\b\w+\b'); // 匹配一个或多个单词字符
      var text = 'Dart is awesome!';
      var matches = p.allMatches(text);
      for(final match in matches) {
          print(match.group(0));
      }
      // 输出：
    	// Dart
    	// is
    	// awesome
      
      // 替换
      var p2 = RegExp(r'\bDart\b'); // 匹配单词 Dart
      var text2 = 'Dart is not just a dart';
      // 替换第一次出现的
      var modified2 = text2.replaceFirst(p2, 'Flutter'); // Flutter is not just a dart
      // 替换所有出现的
      modified2 = text2.replaceAll(p2, 'Flutter'); // Flutter is not just a Flutter
      
      // 分割字符串
      var p3 = RegExp(r'\s+'); // 匹配任何空白字符
      var text3 = 'Dart is fun';
      print(text3.split(p3)); // [Dart, is, fun]
      
      // 查找字符串中第一个匹配项
      var p4 = RegExp(r'\b\w+\b'); // 匹配一个或多个单词字符
      var text4 = 'Parse my string';
      var match4 = print(p4.firstMatch(text4));
      print(match4?.group(0)); // Parse
  }

### import

- 虽然关键字都是`import`但是书写不同于JS

  - `package`是固定写法，表示第三方库
    - 如果是Dart核心库，使用`dart`表示
  - `package:包名/包中运行的具体文件`
    - `dart:包名`
  - `as 别名`

  ```dart
  // 引入安装的第三方库
  import 'package:http:http.dart' as http;
  
  // 引入Dart核心库 不需要别名 直接调用
  import 'dart:convert';
  // 调用核心库中的方法
  jsonDecode(JSON字符串);

## 数据类型

### number数字

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
      print(10.compareTo(12)); // 比大小 0相同 1大于 -1小于
      print(12.gcd(18)); // 最大公约数 6
      
      // 转成字符串 toString 用法和JS一样
      print(n1.toString());
      
      print(n1.toInt()); // 转换成整数 3
      // 不同于JS 可以用数字直接调用API
      print(3.8.toInt()); // 向下取整 3
  }
  ```

### String字符串

- 关键字

  - `String`

- API

  ```dart
  vodi main() {
      // 声明字符串
      var str1 = 'ssss';
      String str2 = 'ssss';
      
      // 三引号声明 需要换行的字符串
      String str3 = '''
      Hello
      World
      '''; 
      // 输出
      // Hello
      // World
      
      // 字符串拼接
      print(str1 + str2);
      // 字符串分隔
      print(str2.split(''));
      print('qwre'.split(''));
      // 字符串裁剪 去掉两边的空格
      print('  abc  '.trim());
      print('  abc  '.trimLeft()); // 去掉左空格
      print('  abc  '.trimRight()); // 去掉右空格
      // 判断字符串是否为空 属性不是方法
      print(''.isEmpty); // true
      print(''.isNotEmpty); // 判断字符串是否不为空 false
      // 字符串替换 同JS中replace方法 但是正则
      print(str.replaceAll('好', 'hello'));
      // 查找字符串
      print(str.contains('s')); // true
  }
  ```

### Boolean布尔值

- 关键字

  - `bool`

- API

  - Dart中只能显式的进行判断，不能用JS中`if(变量)`来判断

  ```dart
  // 显式进行判断
  var f1;
  if(f1 == null) {
      print('真');
  } else {
      print('假');
  }
  // 因为都是明确类型 且不能转换类型 所以没有JS中Number(str)
  // 判断是否为NaN
  var n1 = 0 / 0;
  print(n1.isNaN); // true
  ```

### List数组

- 声明方式

  - 字面量方式

    ```dart
    // 不限制数组元素类型
    List arr = [];
    // 限制元素的类型
    List arr = <int>[1, 2, 3]; // 用<>号包裹类型关键字 并放在数组值前
    ```

  - 构造函数方式

    ```dart
    // 不限长度的空列表
    List arr = new List.empty(growable: true); // growable表示数组是否有扩展性
    // 往数组中添加元素
    arr.add('1');
    // 指定长度的填充列表
    List arr = new List.filled(3, 0); // 创建数组[0, 0, 0]
    ```

- 扩展操作符`...`

  ```dart
  List arr = [1, 2, 3];
  List arr2 = [0, ...arr]; // [0, 1, 2, 3]
  // ...?扩展运算符变种 展开前先判断要展开的变量是否存在
  var arr3; // 要使用没有初始值的变量 必须用var声明
  arr2 = [...?arr3]; // []
  ```

- API

  ```dart
  List arr = [1, 2, 3];
  // 数组反转
  // 注！Dart中数组反转不改变原数组！与JS中不一样
  print(arr.reversed); // (3, 2, 1) 不是数组 是一个可迭代对象
  // 调用toList 转成数组
  print(arr.reversed.toList(growable: true)); // [3, 2, 1]
  
  // 添加元素 addAll(可迭代元素List、Set、Map)
  // 改变原数组
  arr.addAll([4, 5, 4]); // [1, 2, 3, 4, 5, 4]
  
  // 删除元素 remove()
  // 改变原数组
  arr.remove(4); // [1, 2, 3, 5, 4] 只删除了第一个匹配到的元素
  
  // 根据下标删除元素
  // 改变原数组
  arr.removeAt(1); // [1, 3, 5, 4]
  
  // 根据下标添加元素 insert(index, element)
  // 同JS 添加到索引位置前
  arr.insert(1, 9); // [1, 9, 3, 5, 4]
  
  // 清空数组
  arr.clear(); // []
  print(arr.isEmpty); // true
  
  // 数组合并成字符串
  arr = ['a', 'b', 'c'];
  print(arr.join('-')); // 'a-b-c'
  
  // for...in 不同于JS 循环遍历的每一项是数组元素 不是对象的key或数组的索引
  for(var item in arr){
      print(item); // 分别输出 'a' 'b' 'c'
  }
  
  // forEach方法同JS
  
  // map方法 遍历处理数据 返回新列表 不同于JS 返回的是可迭代对象
  arr = [1, 2, 3];
  // 注1: 只能 包含单个表达式
  var b = arr.map((e) => e + 1);
  print(b); // (2, 3, 4)
  // 注2: 不支持多行代码
  // 如下例 使用{}包裹多条代码是 不允许的
  var b = arr.map((e) => {
      return e + 1;
  });
  print(b); // error
  // 注3: 如果想写多行代码 就不能用箭头函数
  var b = arr.map((e) {
      if (e == 1) {
        return e + 1;
      } else {
        return e + 2;
      }
  });
  print(b); // (2, 4, 5)
  // 注4: 返回的是迭代对象 用toList()方法转成数组
  print(b.toList()); // [2, 4, 5]
  
  // where 返回符合条件的元素 类似JS中filter
  // 用法规则同map
  // 示例: 判断数字是否为奇数
  // 定义 返回值为 bool类型 的函数
  bool isOdd(n) => n % 2 == 1; // 箭头函数
  var b = arr.where((e) => isOdd(e));
  print(b); // (1, 3)
  
  // any 只要有一项满足条件 返回true
  // 用法规则同map
  // 注1: 传入箭头函数时 return返回判断条件
  print(arr.any(item => item == 2)); // true 说明有2
  print(arr.any(item => item == 5)); // false 说明没5
  // 传入函数时 不能写() 会自动调用传入的函数
  print(arr.any(isOdd)); // true 说明有奇数
  
  // every 判断每一项是否满足条件 都满足返回true
  // 用法规则同any
  print(arr.every(isOdd)); // false 其中有不是奇数的
  
  // expand 对数组降维
  arr = [[1, 2], [3, 4]];
  arr.expand((e) => e).toList(); // [1, 2, 3, 4]
  
  // fold(初始值, (preValue, current) => 计算表达式) 累计 类似JS中reduce
  arr = [1, 2, 3];
  var result = arr.fold(2, (preValue, current) => preValue * current);
  print(result); // 2 * 1 * 2 * 3 = 12

### Set集合

- 特点

  - Set类似JS中的`Symbol`，是一个无序，元素唯一的集合
    - 重复元素会自动过滤
  - 无法通过下标取值
  - 有一些特有操作
    - 如：求交集、并集、差集等

- API

  ```dart
  // 字面量方式声明1
  var nums = <int>{1, 2, 3}; // 元素为整数的集合
  // 字面量方式声明2
  Set nums = <int>{1, 2, 3};
  // 元素唯一 如下例 有重复元素
  var nums = <int>{1, 2, 2, 3};
  // 重复元素会被自动去掉
  print(nums); // {1, 2, 3}
  
  // 特点 有length属性
  print(nums.length); // 3
  
  // 构造函数方式声明
  // 注意 new Set()不能传入任何参数
  var arr = new Set();
  // 如果想根据其他列表生成Set 进行去重 使用Set.from()
  arr = Set.from([1, 2, 1])
  // 添加元素
  arr.add('aaa');
  arr.add('bbb');
  print(arr); // {'aaa', 'bbb'}
  // 添加多个元素
  arr.addAll(['aaa', 'bbb']); // {'aaa', 'bbb'}
  // 移除元素
  arr.remove('bbb');
  
  // 数组转换为集合
  // 不改变原数组
  List arr = [1, 2, 3];
  var r = arr.toSet();
  print(r); // {1, 2, 3}
  
  // 求交集
  arr = [1, 2, 3];
  var set1 = arr.toSet();
  arr = [2, 4, 5];
  var set2 = arr.toSet();
  print(set1.intersection(set2)); // {2}
  
  // 求并集
  print(set1.union(set2)); // {1, 2, 3, 4, 5}
  
  // 求差集
  print(set2.difference(set1)); // {4, 5}
  
  // 返回第一个元素
  print(set1.first); // 1
  
  // 返回最后一个元素
  print(set1.last); // 3
  ```

### Map字典

- 特点

  - 无序的键值对`key-value`映射

- API

  ```dart
  // 字面量方式声明 没有关键字 用{}声明
  var map = {
    'name': 'xxx',
    'age': 18,
    1: 2
  };
  
  // 特点 有length属性
  print(map.length); // 2
  
  // 构造函数方式声明
  var map = new Map();
  // 新版Dart可以省略new关键字
  var map = Map();
  map['name'] = 'xxx';
  map['age'] = 18;
  
  // 访问属性
  print(map['age']); // 18
  // 修改
  map['name'] = 'sss';
  
  // 判断Map中的key是否存在
  print(map.containsKey('name')); // true
  // 判断Map中的value是否存在
  print(map.containsValue(18)); // true
  
  // 赋值
  // putIfAbsent 如果 key 不存在才 赋值
  map.putIfAbsent('gender', () => '男');
  print(map); // {'name': 'sss', 'age': 18, 'gender': '男'}
  // 已经存在不进行赋值
  map.putIfAbsent('gender', () => '女');
  print(map); // {'name': 'sss', 'age': 18, 'gender': '男'}
  
  // 获取map中所有的 key
  print(map.keys); // (name, age, gender)
  // 获取map中所有value
  print(map.values); // ('sss', 18, '男')
  
  // 根据条件删除 removeWhere((key, value) => 条件)
  // 改变原值
  map.removeWhere((key, value) => key == 'gender');
  print(map); // {'name': 'sss', 'age': 18}
  ```

### 其他类型

#### Runes符文

- Runes对象是一个32位字符对象，可以把文字转换成==符号表情==
  - 例：`print('\u{1f44d}')`
  - 用`str.runes.length`可以**正确**表示符号表情的==字符长度==
- [可转换符号字典](https://copychar.cc)

#### Symbol

- 不同于JS中表示==唯一标识==，Dart中是使用`#`开头的标识符

  ```dart
  // 字面量形式
  var a = #abc;
  // 构造函数形式
  var a = new Symbol('abc');
  print(a); // Symbol('abc')
  
  // Dart中不具有唯一性
  print(a == new Symbol('abc')); // true

#### dynamic

- 关键字，用其声明的变量是==动态数据类型==

  ```dart
  dynamic a = 'sss';
  a = 789;
  print(a); // 789 不会报错
  ```

## 运算符

- Dart特有运算符

  - 地板除`~/`

    - 对除法运算结果进行向下取整

    ```dart
    print(7 ~/ 4); // 1

  - 类型判断运算符`is`、`is!`

    - 类似JS中`instance`

    ```dart
    List arr = [];
    print(arr is List); // true
    print(arr is! List); // false

  - 避空运算符`??`、`??=`

    - `??`类似JS中`a || b`

    ```dart
    print(1 ?? 3); // 1 因为1不为空
    print(null ?? 3); // 3
    // 0不算空
    print(0 ?? 1); // 0 且会警告
    // 和直接用||的区别
    print(0 || 1); // 会报错 因为 &&和|| 两边只能放bool类型 而??可以像JS中||一样无类型判断
    // ?? 通常用于判断空 如声明的变量没有值时才会返回另一个结果
    var num;
    print(num || 0); // 0
    
    // ??=等同if赋值语句
    var a;
    if(a == null) {
        a = 3;
    }
    // 等同
    a ??= 3;
    print(a); // 3
    // 此时a已经有值
    a ??= 6;
    print(a); // 3 不会进行赋值
  
  - 条件属性访问`?.`
  
    - 同ES6
  
  - 级联运算符`..`
  
    - 书写形式类似JS中`Promise`
  
    ```dart
    myObject.myMethod(); // 返回 myMethod 的返回值
    myObject..myMethod(); // 返回 myObject 对象的引用
    
    // 示例
    Set s = new Set();
    s.add(1);
    s.add(2);
    s.add(3);
    s.remove(2);
    print(s); // {1, 3}
    // 用 .. 重写
    Set s = new Set();
    // 因为每步执行返回的是调用者 因此可以 链式书写
    s..add(1)
     ..add(2)
     ..add(3)
     ..remove(2); // 只用在末尾写;号
    print(s); // {1, 3}
    ```

## 函数

### 声明函数

#### 直接声明

- 形如JS，但是不需要`function`关键字，且需要指定返回类型
- 没有函数声明提升

```dart
void main() {
    // 声明和调用
    void fn() {}
    fn();
    
    // 没有函数声明提升
    // 报错
    fn();
    void fn() {}
}
```

#### 箭头函数

- 形如JS，**但是**==函数体只能写**一行**且不能带有结束的;号==
- 只是作为==普通函数==的简写形式，没有JS中那样特殊的用法

```dart
List arr = [1, 2, 3];
// 只能写一行
// 形式1 用{}包裹 但是只写一句
arr.forEach((item) => {
    print(item) // 不能写;号
});
// 形式2 return的简写形式
arr.forEach((item) => print(item));
```

#### 匿名函数

- 同JS

```dart
var fn = (a) {
    print(a);
}; // 注1 因为是赋值语句 声明的匿名函数 语句末尾要加;号

// 示例
List arr = [1, 2, 3];
arr.forEach(fn);
```

#### 立即执行函数

- 同JS

```dart
// 不用写返回函数类型
((int n){
    print(n);
})(17);
```

### 函数参数

- 必填参数

  - `参数类型 参数名`

  ```dart
  String fn(String name) {
      return `你好: $name`;
  }
  String res = fn('张三');

- 可选参数

  - 放在必传参数后
  - 用`[]`包裹
  - 如果指定类型，则必须在类型后加`?`号

  ```dart
  String fn(String name, [int? age, str, str2 = 'asd', String? str3 = '111']) {
      return `你好: $name, 年龄: $age`;
  }
  print(fn('张三', 20));

- 命名参数

  - 用`{}`包裹

  ```dart
  // 通常形式 指定参数类型 就必须赋初值
  String fn({String name = '张三'}) {
      return '你好: $name';
  }
  print(fn()); // 你好: 张三
  print(fn(name: '李四')); // 你好: 李四
  
  // 不指定类型 就可以不赋初值 而且可以传入不同类型的值
  // 但是不建议这样用
  String fn({name}) {
      return '你好: $name';
  }
  print(fn()); // 你好: null
  print(fn(name: '张三')); // 你好: 张三
  print(fn(name: 111)); // 你好: 111
  
  // 不指定类型 赋初值 不会指定形参类型
  String fn({name = '张三'}) {
      return '你好: $name';
  }
  print(fn(name: 111)); // 你好: 111 不会报错
  
  //当有其他参数时
  String fn(String name, {int age = 1}) {
      return '你好: $name, 年龄: $age';
  }
  // 注意 传命名参数 时 要以 key: value的形式
  print(fn('张三', age: 20));

- 函数参数

  - 将函数作为参数传入
  - 同JS

  ```dart
  var fn = (value) {
      print(value);
  }
  List arr = [1, 2, 3];
  arr.forEach(fn);

### 异步函数

- JS中异步通过`Promise`实现
  - `async`函数返回一个`Promise`。`await`用于等待`Promise`

- Dart中通过`Future`实现
  - `async`函数返回一个`Future`，`await`用于等待`Future`

  ```dart
  // 安装 引入 http库
  import 'package:http/http.dart' as http;
  
  // 引入Dart核心库
  import 'dart:convert';
  
  // 声明 返回值 为 Future类型 的函数
  Future fn() {
      return http.get(url);
  }
  void main() {
      // 同JS一样链式调用
      fn()
       // 请求回来的结果在body中 以JSON字符串形式存储
       // 将JSON转换为对象 需要用 Dart核心库中的方法
       .then((res) {
           var data = jsonDecode(res.body);
           print(data);
       })
       // 注: Future中不是catch而是catchError
       .catchError((error) => print(error));
  }
  
  // 声明 async函数
  // 注: async关键字放后面
  Future fn() async {
      final res = await http.get(url);
      return res
  }
  void main() async {
      try {
          final data = await fn().then((res) => jsonDecode(res.body))
          print(data);
      } catch(err) {
          print(err);
      }
  }

## 类与对象

### 类

- 概念同ES6

- JS中多是用一个个函数调用处理逻辑，即==面向过程编程==
  - JS中的`class`完全是语法糖，和Dart中的类是两个不同的概念
  - 而Dart更像Java，是基于==类和对象==的编程方式

- 示例

  - 类不能定义在函数中，如`main`函数
  - Dart中类可以省略`new`，也能创建实例对象
  
  ```dart
  // 声明类
  class Person {
      // 定义属性
      String name = '张三';
      // 定义方法
      void getName() {
          // 注: 类方法中可直接访问类中定义的属性
          print('我是 $name');
      }
  }
  void main() {
      Person p = new Person();
      // 访问实例属性
      print(p.name); // '张三'
      // 调用实例方法
      p.getName();
      // 可以省略new
      Person p = Person();
  }
  ```

### 构造函数

#### 默认构造函数

- 用`类名()`作为构造函数，会在==实例化==时，==第一个被调用==

  ```dart
  class Point {
      num x = 0;
      num y = 0;
      Point(num a, num b) {
          // 有两种形式
          // 1、用this.形式
          // 使用场景 构造函数的形参命名和实例身上的属性命名冲突时 必须用this. 所以更推荐用这种形式
          this.x = a;
          this.y = b;
          // 2、直接赋值
          x = a;
          y = b;
      }
      // 构造函数简写 接收参数的同时赋值
      Point(this.x, this.y);
  }
  void main() {
      var p = new Point(1, 1);
      print(p.x); // 1
  }

#### 命名构造函数

- 用`类名.函数名()`形式，可以存在多个命名构造函数，提供额外清晰度

- 任意类型的构造函数都可以存在，一个类中不是只能写一种类型构造函数

  ```dart
  class Point {
      num x = 0;
      num y = 0;
      // 书写形式和普通构造函数相同
      Point.first(num x, num y) {
          this.x = x;
          this.y = y;
      }
      
      // 用 命名参数 给形参赋 默认值
      Point.second({int x = 1, int y = 1}) {
          this.x = x;
          this.y = y;
      }
  }
  void main() {
      var p1 = new Point.first(1, 1);
      print(p1.x); // 1
      
      var p2 = new Point.second(x: 10);
      print(p2.x); // 10
      print(p2.y); // 1
  }

#### 常量构造函数

- 使用场景：类生成的对象不会改变时

  ```dart
  class Point {
      // 属性 必须 用 final 声明
      // 用final声明可以不用赋初值
      final num x;
      final num y;
      
      // 常量构造函数 必须通过 const 声明
      const Point(this.x, this.y);
      
      // 注: 常量构造函数 不能有函数体!
      // 以下形式 会报错
      const Point(num x, num y) {
          this.x = x;
          this.y = y;
      }
  }
  void main() {
      // 常量构造函数 可以 当作普通构造函数使用 但是就失去了其原本的意义
      var p1 = new Point(1, 2); // 正常运行
      var p2 = new Point(1, 2);
      print(p1 == p2); // false
      
      // 声明不可变对象 必须 通过 const 关键字
      var p3 = const Point(1, 2);
      var p4 = const Point(1, 2);
      print(p3 == p4); // true
  }
  ```

#### 工厂构造函数

- 不能进行实例化，因此不能使用`this`

  ```dart
  class Person {
      String name;
      // 静态 Person类型 的变量
      static Person instance;
      // 工厂构造函数
      factory Person([String name = '张三']) {
          // 不能用this
          print(this.name); // error
      }
  }
  void main() {
      // 不能做实例化操作
      Person p1 = new Person('李四');
      print(p1.name); // error
  }
  ```

- 示例

  - `mapObj.putIfAbsent(key, () => value)`方法
    - 返回值：Map中的`value`值
    - 用法：根据传入的`key`，查询`mapObj`中是否有`key`
      - 如果有，则直接返回`key`对应的`value`
      - 如果不存在，则**先**在`mapObj`中创建对应的`key: value`,**再**返回`() => value`的`value`

  ```dart
  class Person {
      String name = '';
      // 作用类似 单例模式 的 全局变量
      static final Map instances = {};
      
      factory Person(String name) {
          // 不能实例化 因此没有this 用 类.属性/方法 的形式调用
          // 工厂构造函数 必须 有return 不然会报错
          return Person.instances.putIfAbsent('first_instance',() => new Person.createInstance(name));
      }
      
      // 可以有多个工厂函数
      // factory是关键字 可以加在其他构造函数前 使其变为工厂函数
      factory Person.createInstanceByMap(Map obj) {
          return new Person(obj['person_name']);
      }
      
      // 定义用于 实例化 的 命名构造函数
      Person.createInstance(this.name);
  }
  void main() {
      // 实例化操作
      Person p1 = new Person('张三');
      print(p1.name); // 张三
      
      Person p2 = new Person('李四');
      // 因为实例唯一(单例模式) 读取的还是旧实例
      print(p2.name); // 张三
      
      print(p1 == p2); // true
      
      // 用另一个工厂构造函数创建实例
      Person p1 = new Person.createInstanceByMap({'person_name': '张三'});
      print(p1.name); // 张三
  }

### 访问修饰符

- Dart不同于TS，没有访问修饰符`public`、`private`等，而是用==属性==或==方法名==表示与访问修饰符同样的意思
  - `public`：默认就是`public`
  - `private`：==属性==或==方法名==以下划线`_`开头
    - Dart的私有属性不同于TS等语言，它的==私有是对文件而言==，即==当前文件内都可以访问到类内的私有属性==、方法。要==真正私有==，得将类放到另一个文件
      - 例：在目录下创建`lib/Person.dart`，再在需要用的地方`import 'lib/Person.dart'`引入类文件

  ```dart
  class Person {
      // 公有属性
      String name = '';
      // 私有属性
      num _money = 100;
      // 读取私有属性
      num getMoney() {
          return this._money;
      }
      // 私有方法
      void _myName() {
          print('我是 $name');
      }
      // 字符串内$ 同样可以获取私有属性
      String fn() {
          return '$name 有 $_money 元';
      }
  }
  void main() {
      var p = new Person('张三');
      print(p.getMoney()); // 100
      print(p.fn()); // 张三 有 100 元
  }