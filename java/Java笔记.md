

## 面向对象

- 类是模子，而对象是类的具体数据（好比类是我对一个事物不确定的描述，而对象则是具体的实物）

- ![](http://upload-images.jianshu.io/upload_images/6322775-694eedec576c7b53.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- ![“猫能做的”就是**方法**](http://upload-images.jianshu.io/upload_images/6322775-e40081b2b6a0775b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- 包名推荐命名规范：域名的倒叙

- java中创建类：
  public class 类名{ 
  //成员属性
   string xxx;
   int xxx;
   //方法
   public 返回类型 方法名(){
      代码内容
    }
  }

---

## Java方法

- ![方法](http://upload-images.jianshu.io/upload_images/6322775-84cd6e0481d003a5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- sc.next()是获取输入的字符串

- 静态**方法**是使用**类**来调用

- 方法就是一类问题的代码的有序组合，是一个**功能模块**

- public是**访问修饰符**

- ![](http://upload-images.jianshu.io/upload_images/6322775-bc1aa56c1191d101.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- 访问修饰符：方法允许被访问的权限范围

- 参数之间以`，`号分割

- 对象名.方法名（）调用方法

- 注意定义方法的**返回类型**，错误的返回类型，会导致即使定义对的变量，也**返回不出正确的值**

- ![String用法](http://upload-images.jianshu.io/upload_images/6322775-783867578c403b9f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- ![可变参数列表](http://upload-images.jianshu.io/upload_images/6322775-e55587333a10fbb3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- ![**i**的值是每次从n中取出一个数放到**i**当中，直到所有元素都取完，循环结束](http://upload-images.jianshu.io/upload_images/6322775-092873b5cad357cd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- 可以把数组的值传递给可变参数列表

- **数组**和**可变参数**不可以相同函数名称同时出现，并且**数组可以向可变参数传值，但可变参数不能向数组传值**

- 可变参数列表所在的方法是最后被访问的

- ![以这个开头的是文档注释，可以通过`Javadoc`命令生成帮助文档](http://upload-images.jianshu.io/upload_images/6322775-d68b44bab65eb798.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- 单步调试——一般用于函数内部

  ### 数组

  - 格式一：先声明后创建
    数据类型[ ]   数组名； 
    数组名 = new 数据类型[数组长度]；
         例：   
         int[] arr；
         arr=new int[10]；
  - 格式二：声明的同时创建
  数据类型[ ]  数组名 = new 数据类型[数组长度]；
  ```
  例：int[] arr=new int[10];
  ```
  - 数组属性length表示数组长度，取用时可以a.length
  - ![循环外创建sc对象后，在循环内获取输入内容](http://upload-images.jianshu.io/upload_images/6322775-fdf9613638b175dd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
  - ![数组与变量同类型,每次循环的值储存在n当中](http://upload-images.jianshu.io/upload_images/6322775-1861cc4153d9bb88.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- ![循环条件后的分号不能丢](http://upload-images.jianshu.io/upload_images/6322775-e3caf6d1fddc41db.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- ![](http://upload-images.jianshu.io/upload_images/6322775-34a828df50691538.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- ![](http://upload-images.jianshu.io/upload_images/6322775-3b8261cc3cbec36a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- ![比如for循环，在循环之外输出for条件中定义的变量会提示错误](http://upload-images.jianshu.io/upload_images/6322775-2a069484e07fd830.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- break跳出当前循环

- ![输出空即**换行**](http://upload-images.jianshu.io/upload_images/6322775-d2629a8e41a433f1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- 外重循环执行一次，内重循环要把整个循环的内容执行一遍

- continue结束当前循环，不执行后面的语句，继续下一次循环

- **debug**：看清程序的每一步效果，在需要查看结果的时候，使用debug查看是否与实际结果一致
  第一步：设置断点（双击行号前面）；
  第二布：调试（debug run as）。debug框体显示断点位置，variable框体显示**变量**信息（主要）；
  第三步：单步调试

- if：判断条件是一个范围；
  switch：判断条件是**常量值（圆括号中的值与case后面的值相匹配）**。

- ![注意整型是`nextInt`](http://upload-images.jianshu.io/upload_images/6322775-ef25a2683b4c26f5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
  ![字符串类型`next`后没有Int](http://upload-images.jianshu.io/upload_images/6322775-fabbf4fdb9ed7ed7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- ![表达式应与常量保持类型一致](http://upload-images.jianshu.io/upload_images/6322775-7971f61e4d5270e3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- ![**可以把输入的字符串全部改为大写**](http://upload-images.jianshu.io/upload_images/6322775-061f72bf73245383.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- ![](http://upload-images.jianshu.io/upload_images/6322775-e6d7cd641e1d0fc5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- ![放后面是对原数赋值](http://upload-images.jianshu.io/upload_images/6322775-1fe284778e42610f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- ![Y的值是X不自增的结果](http://upload-images.jianshu.io/upload_images/6322775-1271842eddd72d4b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- ![放前面两个都赋同样的值](http://upload-images.jianshu.io/upload_images/6322775-4855bb577080af82.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- ![字符链接算法，输出结果是两个数字连起来](http://upload-images.jianshu.io/upload_images/6322775-d1f63f77cbdbe05d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- ![使用**Scanner**获取键盘输入的整数](http://upload-images.jianshu.io/upload_images/6322775-8b51c7b0e2b6affe.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- &&是短路运算符：如果前面为false，那么后面的不进行运算；而&两边都要进行运算。

- ||也是短路运算符：只要有一个能决定结果为true，那就不用计算另一个

- ![逻辑非](http://upload-images.jianshu.io/upload_images/6322775-79e3e61b488a59f3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- ![true返回前一个值false返回后一个](http://upload-images.jianshu.io/upload_images/6322775-af8c7fc4c6676208.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- int x=3,y=5;(正确)；int x,y;x=3,y=5(错误)

- 转义字符:`\n换行  \r回车`

- java中常量命名用大写字母，如果是两个单词组成则用`_下划线`连接

- > java中区分大小写

  > **Java**中***print***、***printf\***、***println\***的区别详解:
  >
  >   printf主要是继承了C语言的printf
  >
  >   print就是一般的标准输出，但是不换行
  >
  >   println和print基本没什么差别，就是最后会换行

  > java中布尔类型没有0，1只有true和false

![img](https://upload-images.jianshu.io/upload_images/6322775-d51403d048711ebe.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

JDK是java语言的软件开发工具包

**JVM**（java虚拟机）

![img](https://upload-images.jianshu.io/upload_images/6322775-e0ef672e66277ff3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**JRE**包括**JVM**和**java核心类库**和**支持文件**

**JDK**中附带**JRE**

将普通**记事本**文件保存为java文件可以l另存文件为**xxx.java**，格式选**所有文件**

win10下配置JDK环境变量的时候，在电脑—属性—高级设置—环境变量—找到path—***编辑文本—***使用；号将jJDK安装路径复制到后面

在使用javac生成class后缀的文件后，只需java xxx不需要class后缀

![img](https://upload-images.jianshu.io/upload_images/6322775-5250b4997fb29d96.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

system的开头S必须要大写

![img](https://upload-images.jianshu.io/upload_images/6322775-31d98a172a9e8a4a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

AS中**src**是source的缩写，用于存储**.java的源文件；**bin下存放**.class文件；**

1. [堆排序（python）](https://blog.csdn.net/qq_23936173/article/details/87611687)
2. ![复杂度](https://upload-images.jianshu.io/upload_images/6322775-90cf07f4041ac347.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
3. [消息队列](https://blog.csdn.net/chaliji1845/article/details/100959763)

   [微服务](https://www.zhihu.com/question/65502802/answer/802678798)