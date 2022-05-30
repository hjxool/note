1.  范型：实例化的时候指定类型
2. 创建属性的意义
- ![创建属性](https://upload-images.jianshu.io/upload_images/6322775-4fa7a79fad0de433.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
- 其实**自定义变量**已经**默认实现**了属性，只是简单的读写而已，我们**定义的属性**要在**读写**的基础**上加**上更多**限制**，以后你学的多了就会发现， 很多东西都是为了**数据安全**而加入的，说白了就是限制，例如接口，接口中的方法你必须(注意是必须，强制性的)要实现
- **get访问器**有些像类的方法，不过它返回的是属性的值
- console.write("内容");//打印语句
-  `using`这个关键字的用途.是导入命名空间
- Console.WriteLine()与Console.Write()不同,打印之后换行;书写格式
```
Console.WriteLine("XXX{0}，AA{1}。",{0}对应变量名,{1}对应变量名)
{0}~{3}表示将会输出4个变量的值，而四个变量依次写在字符串后面```
- char和string的区别：char用`''单引号`括起来的**一个**字符；string用`""双引号`括起来的一串字符
- 数组声明：
```
数据类型[] 数组名 = new 数据类型[长度];```
- 查找算法的共性：
```
循环访问每一个数据（循环条件）```
```
对每一个数据进行验证（筛选条件）```
- ![关键字foreach](http://upload-images.jianshu.io/upload_images/6322775-ec20d4d6c9270ceb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
- ```
int[,] arr = new int[2,3]; //包含2个一维数组，每个一维数组包含3个变量，总共2*3=6个数组元素```
- 从键盘输入是`console.readline()`**但是接受的只能是字符串，所以要存储到其他数据类型需要`int.parse()`来将数字内容的字符串转为int类型**
- sqlconnection con = new sqlconnection(constr);
sqlcommand cmd = new sqlcomand();
cmd.connection = con;
con.open();cmd.commandText = "xxx";
cmd.ExecuteNoQuery();//如果是查询则要赋值，如下
cmd.CommandText
sqlDateReader dr = cmd.ExcuteReader();
while(dr.read())
{
       print
}
3. C#里**静态方法**里**不能**访问**非**静态的变量或方法
- **类名.方法/变量**叫**直接**访问
- **对象.方法**叫**间接**访问
4. 引用参数**ref**
- 引用参数相当于**取地址符&**带此关键字的**形参**的任何操作**直接**作用于**实参**
- 引用是引用，指针是指针（引用等号右边是变量**说明是这个变量的别名**，指针等号右边是地址）
5. 输出参数**out**
- 引用参数的一种，**ref**和**out**传递的都是**参数**的**地址**
- **区别**：**ref**使用前一定要在主函数中**初始化**，而且一般用于传递**数组**参数，**不能**传递**string**
- **out**使用时**不初始化**，但是**不能**传递**属性**
6. **代理和事件**
- 代理与对象的引用(`类名 对象 = new 类()`)；
**不同**的是**代理**指向的是**方法**；
**对象的引用**指向的是**实际的对象**
- **方法标识：**包括**返回类型**及传入**参数**
7. 使用**delegate**声明一种类型的代理，例如：
```
delegate int MyDelegate(int p, int q);
MyDelegate arithMethod;
arithMethod = new MyDelegate(function);
合并：MyDelegate arithMethod = new MyDelegate(function);
```
- 返回值类型和参数与方法相同
8. **同一个**代理引用可以指向**不同**的**方法**，只要这些**方法**具有与**代理定义**相同的**方法标识**
9. 代理其实就是**System.Delegate**的**子类**，所以不能在方法内定义(`delegate int MyDelegate(int p , int q);`其他可以在方法内声明)
- 指向的是哪个方法(`arithMethod = new MyDelegate(Add)`)，调用(`int  r = arithMethod(3,4)`)的时候就用那个方法
10. 事件
- 跟**代理**类似`public delegate void TimerEvent(object o, EventArgs e)`
- 但是多一个**event关键字**（`public event TimerEvent Timer;Timer(this,null)`）等同于`MyDelegate arithMethod = null`
- 添加方法也是相同`Timer += new TimerEvent(方法名)`
- 事件是**多重代理**类型
11. **override**重写
- 必定是子类里
- 父类有**abstract抽象类**必定使用**override**（因为抽象方法不能实例化，也不能写具体方法）
12. **类型兼容规则**
- 需要父类的任何地方可以使用**公有**派生类对象来替代
- 替代后可以使用从父类继承的成员
13. **接口**
- 接口所有成员都具有**public、abstract**的默认属性，所以接口所有方法都必须在**子类**中进行实现
- 实现的时候必须是**public**类型
- **接口实现**的时候不使用**override**