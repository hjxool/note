### 面向对象

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

### Java方法

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

  #### 数组

  - 格式一：先声明后创建
    数据类型[ ]   数组名； 
    数组名 = new 数据类型[数组长度]；
         例：   
         int[] arr；
         arr=new int[10]；
  - 格式二：声明的同时创建
  数据类型[ ]  数组名 = new 数据类型[数组长度]；
  ```
  例：int[] arr=new int[10];```
  - 数组属性length表示数组长度，取用时可以a.length
  - ![循环外创建sc对象后，在循环内获取输入内容](http://upload-images.jianshu.io/upload_images/6322775-fdf9613638b175dd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
  - ![数组与变量同类型,每次循环的值储存在n当中](http://upload-images.jianshu.io/upload_images/6322775-1861cc4153d9bb88.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)