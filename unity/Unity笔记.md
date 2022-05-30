- transform这些都是component（零件）
- transform三要素：位置、旋转、大小
- 如果把一个图形拖动到另一个图形下，就是作为子节点，在改变父节点的时候会同时改变子节点
- 使用select可以将同一模板（prefab）下的所有模型同时改变
- prefab作用：1、重复利用，2、同时修改
- ![**注意，着色器是diffuse**](http://upload-images.jianshu.io/upload_images/6322775-a1821651fbcffaf9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
- 在自己的项目下双击输出的材质包可以使用别人的材质
- mesh决定物体的形状
- material（材质）决定效果
- texture（贴图）
- shader（着色器）
- 在main camera中projection（投射）可以调整是perspective（3D）还是oortho（2D）效果
##### Unity如何通过脚本驱动游戏
- ![](http://upload-images.jianshu.io/upload_images/6322775-f00690fd300b8e43.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
- ![](http://upload-images.jianshu.io/upload_images/6322775-0b55b189c0e95ee4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
##### 如何更新逻辑
- 场景启动时调用所有脚本的Awake()
- 然后调用所有的Start()
- 然后每一帧调用一次Update
- 调用完Update后再调用LateUpdate（Update和Latedate都是一帧调一次）
- 然后再一帧中调用几次FixedUpdate（比如：60帧时一帧会调用1次，30帧时一帧调用2次；这样可以保证单位时间内更新频率一样）
##### 对象销毁
- 调用Destory销毁GameObject（GameObject中都是组件）
- OnDestory吸垢
###### 脚本间通信
- 通过GetComponment找到**同一个物体**上面挂着的其他脚本
- GameObject.Find来找**其他物体**
***
-  Gameobject是一个类，所有的component（组件）都是这个类的对象
- gameobject是一个对象，比如
```
gameObject.GetComponent<Text> (); //通过gameObject获取到Text组件```
- 同样的 Transform是一个类 而transform是Transform类的对象
- ```
transform.forward//是面向的方向``` 

- ```
Time.deltaTime//上一帧到这一帧经历的时间```
- 总结，步数 = 当前位置+一帧的时间x面向方向x速度
- `camera.transform`摄像机位置，同样的`player.transform`是人物位置
- 初始化就是初始化，比如，int a = 1，start()就是做这个的
- GetComponent<>来获得**组件**，例如`GetComponent<SceneCamera>`；而Gameobject.Find()用来获取**游戏体**,例如`Gameobject.Find("Main Camera")`
**注意！获取组件不用空格分开，而获取游戏体需要按原格式书写并且用`""`号括起来**
- ![使用`LookAt`来跟踪人物](http://upload-images.jianshu.io/upload_images/6322775-b592833decd640ee.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
##### 设置用户输入
-  ![首先](http://upload-images.jianshu.io/upload_images/6322775-18006da1fba9fccb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
- ![然后](http://upload-images.jianshu.io/upload_images/6322775-33a0eb98eedb5d84.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
- ![**`Input.GetAxis("")`**是获取函数的状态，然后返回-1到1的**float型数值**](http://upload-images.jianshu.io/upload_images/6322775-02e2ca335de48916.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
- ![**transform.right**表示**左右方向**,**forward**同理，Input返回的是正则向右移动，负值向相反移动](http://upload-images.jianshu.io/upload_images/6322775-656fd688ee13d595.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
- ![速度x方向x每帧的时间](http://upload-images.jianshu.io/upload_images/6322775-41b7f7e114dfea2c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
- 使用`Debug.Log`可以在屏幕左下显示数值
##### 使用鼠标控制角色
![](http://upload-images.jianshu.io/upload_images/6322775-e39bf37ff3361d36.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![this.camera是摄像机这个点引一段射线，**RaycastHit（击中）在场景中什么位置**，foreach循环算出位置，并走到这个点](http://upload-images.jianshu.io/upload_images/6322775-cba2c0d6482be37c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
```

- Object—player—animation（动画）—loop time（循环播放）
##### 状态机（stateMachine）
- 橘黄色表示默认（Default）状态
- 其中一个重要的条件**condition**：播放0到1时间的动画后退出当前状态，跳转到下一状态；拨动刻度指针来调整从一个状态的另一个状态的过渡时间（避免动作僵硬）
- **首先**在**parameter（参数）**里面设置变量，才能在**condition**里面找到这个参数
- conditions（条件）的意义就在于什么条件下**触发**，比如跑动速度speed参数设置为Geater（大于）某一值时触发跑动，还有比如跑动角度
![(当前位置—上一帧位置)/间隔时间](http://upload-images.jianshu.io/upload_images/6322775-8a25c1698d17ae75.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![速度的向量](http://upload-images.jianshu.io/upload_images/6322775-1eebef1aa1abc234.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![Y是垂直方向的速度，X  Z是平面的](http://upload-images.jianshu.io/upload_images/6322775-9ba88e804e2a30f2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
> **Vector3**  表示**三维向量类**

![**magnitude** 表示**向量值**](http://upload-images.jianshu.io/upload_images/6322775-db4410dc853d836b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![H这个函数是自己编写的计算角度的函数](http://upload-images.jianshu.io/upload_images/6322775-5fc428b2b667e0fa.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![`Mathf.Atan2`和`Mathf.Red2Deg`都是系统自带的**Mathf数学方法库**](http://upload-images.jianshu.io/upload_images/6322775-819f2fefc5ecee7e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
-  **静态方法**在访问**本类**的成员时，**只允许**访问**静态**成员（即静态成员变量和静态方法），而不允许访问实例成员变量和实例方法
![更新位置](http://upload-images.jianshu.io/upload_images/6322775-e9030e3ba5356c7c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![设置参数值](http://upload-images.jianshu.io/upload_images/6322775-f4d325f8e5820106.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
- **Any State**按钮代表了所有其他按钮，用它跟**idle(空闲)**按钮连接在一起，设置当速度小于某一值时，**任何按钮**都转换为**idle(空闲)**
- `Vector.up`是Vector3(0, 1, 0)的简写,也就是向y轴
- Unity5中进行了语法规范化，所有组件都要定义一个变量，然后让变量指向组件，简言之——如果要调用类中的成员，必须要声明一个对象，比如`rb.velocity`，但是只要不调用类中的成员，可以不用声明对象，比如`this.gameobject`(**GetComponent<组件>**)
- 脚本文件，**类名**和**文件名**要相同
- 实现某一功能，除了要有相应的脚本还要有相应的组件

-  OnBecameInvisible方法(功能函数)是在游戏对象移动到画面之外不在被绘制时被调用的方法。
-  Destroy(this.gameobject)是删除游戏对象的方法
-  ![](https://upload-images.jianshu.io/upload_images/6322775-b143a532ee5a4045.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
-  ![](https://upload-images.jianshu.io/upload_images/6322775-ec50355ba4fc5edd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
-   ![束缚(constrains)用来限定物体移动和旋转](https://upload-images.jianshu.io/upload_images/6322775-9ef6e731ce360d38.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
-  项目视图中的预设才是本体
-  不管创建何种**材质**都是在项目视图中create
-  ![](https://upload-images.jianshu.io/upload_images/6322775-1f91a276015b39a7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
-  ![限定高度以后用公式计算速度](https://upload-images.jianshu.io/upload_images/6322775-aac66a80f849e167.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
-  只改变视觉位置只要在**层级视图**修改数据就可以；但是绑定了**运动**后需要在**项目视图**中更改
-  ![但是检视面板的Apply按钮，可以把实例的变化反映到预设中](https://upload-images.jianshu.io/upload_images/6322775-0a78905d9946c69a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
- ![](https://upload-images.jianshu.io/upload_images/6322775-0b915ad968dc8c96.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
- `Mathf.RoundToInt`绝对值四舍五入运算；
`Mathf.FloorToInt`(向下舍位取整)可计算负值
- 制作**2D**，纹理图的种类要选择GUI；
材质要选Unlit-transparent(透明做背景)
- 材质里面offset：指明使用贴图的起始位置，取值范围为0-1；
tiling：指明从offset位置处的大小区域，区域的取值范围一般为(-1,1)，超过的话部分会按比例生成新的区域拼接上原先的。
- Update()：每帧被调用一次;
FixedUpdate()：每隔Time.fixedDeltaTime被调用一次
- 一段动画由几个动作完成余数就取几
- 帧数：每秒的画面数，以flappy bird为例，设置为每秒10帧，而每3帧就可以播放完整个动作，所以说每秒可以播放3遍扇翅膀的动作
- float类型必须要加f（0除外）
- >**如果想实现两个刚体物理的实际碰撞效果时候用OnCollisionEnter，Unity引擎会自动处理刚体碰撞的效果。**
>**如果想在两个物体碰撞后自己处理碰撞事件用OnTriggerEnter。**
- >Is Trigger就是触发器不做物理碰撞受力
>如果同时要碰撞和触发就用OnCollisionEnter 
- Awake()是在脚本对象**实例化**时被调用的，而Start()是在对象的第一帧时被调用的，而且是在Update()之前;*Awake会先实例化然后对变量赋值*
- 在脚本中想要获取其他层级视图下物体的位置可以使用**public Transform XX**然后将物体拖拽到XX下
- 在update里使用数字会每次都更新
- ![](https://upload-images.jianshu.io/upload_images/6322775-3c60603205336c96.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![用Transform 组件来获取一个物体的位置](https://upload-images.jianshu.io/upload_images/6322775-e9372ae591b61c03.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
- Main Camera移动到目标下就是离它的相对距离
- OnGUI()函数可以在每帧调用，就像Update( )函数一样

- C#中创建对象必须要用new开辟空间(对象 = new 类名)
- **枚举**只能在**本类**中使用
- 触发检测是OnTrigger，形参是Collider(碰撞机);
碰撞检测是OnCollision，形参是Collision(碰撞);
- **非静态**变量需要用实例来调用，**静态**变量用类名就可以调用
- 在安卓发布的时候注意Package Name(包名)不能使用默认值
- 新版Unity5之后创建GUI Texture需要 层级视图下>create Empty>Add Component>**Rendering**>GUI Texture(注意将XY位置调成0.5，处于画面正中央)
- ![最简单的单例实现方法](https://upload-images.jianshu.io/upload_images/6322775-4f2689aee456f6f9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
- **方法只有设为public才能在外面调用**
- unity3d提供了一个用于本地持久化保存与读取的类——**PlayerPrefs**，
setXX("显示文本"，默认值)是保存数据，而getXX("显示文本"，默认值)是读取数据
- ![](https://upload-images.jianshu.io/upload_images/6322775-12e7727d2b623e43.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![对比差别](https://upload-images.jianshu.io/upload_images/6322775-bdc2b168020dbd2a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
- ![](http://upload-images.jianshu.io/upload_images/6322775-b6c6cac70160928c.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1080/q/50)
- Rect(x,y,w,h)
x表示水平距离，即以左上角为0,0点，距离左边的距离
y表示垂直距离，距离顶部的距离
w表示这个矩形的宽度
h表示这个矩形的高度
- str1.**IndexOf**("字",start,end)；//从str1第start+1个字符起，查找end个字符，查找“字”在字符串STR1中的位置[从第一个字符算起]注意:start+end不能大于str1的长度
- **速度**可以是负值，这样就会从与设定相反的方向运动；
**同一个脚本**可以挂在不同的组件上，起到的作用是相同的
- 可以创建一个空gameObject设置刚体和碰撞检测，而挂在下面的材质则不用管
- ![区分两者目标的不同](https://upload-images.jianshu.io/upload_images/6322775-48d0a5abbdb102e5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
- 数组创建的时候是**类型名[] 变量名**注意中间没有等号；
使用的时候是**变量名[序号或者方法]**
- 如果想要从一个数组中随机的取出一个数据，此时可以用Random函数：
> var element = myArray[Random.Range(0, myArray.Length)];
- 只能用`public static 类型名`来定义静态变量，并且**静态变量不会显示在面板上**
- **静态变量**可以很方便的在其他脚本中用**脚本名.变量名调用**，但是其他变量和方法需要**单例化**才能调用
- `this.transform`意思是：访问到**这个脚本所在**的GameObject上的transform**组件**（**只能访问脚本所挂载的GameObject上的组件**）
- 第二种**访问其他组件**的方式是通过定义一个**Public变量**（`public Transform trans...`），然后在面板中选择。
- 第三种访问其他组件的方式
`public GameObject xxx`
面板上选择GameObject 
`Transform t = xxx.GetComponent<>`