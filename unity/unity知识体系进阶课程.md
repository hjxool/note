1. 自动寻路功能
- **Component—Navigation**
- 在**要移动**的物体上添加**Nav Mesh Agent组件**可以实现自动寻路功能
2. 要实现跟随功能
- 首先：需要一个`public Transform target`和`NavMeshAgent agent`
- 然后：`agent.destination = target.position`
3. 每次设置完障碍物后要**重新设置Mesh面积**
4. Navigation面板—Bake—Off Mesh Link设置**跳跃点**
- **Drop Height：**可跳落高度
- **Jump Distance：**可跳距离
- **Off Mesh Link组件：**创建两个GameObject设置起跳和落点 
5. 只有选中了**Navigation Static**之后才能进行**网格**导航
6. 自定义重力：**Edit**—**project setting**中设置
7. 勾选**is kinematic**就不会受物理系统的影响了
8. 通过判断**OnTriggerStay**可以实现**弹射**
9. **Joint**类的组件可以实现**弹性连接**
10. **constant Force**恒力组件
11. 射线检测(应用于**第一人称拾取物体**等)
- **Camera**下有**ScreenPointToRay()方法**可以从摄像机向鼠标位置射线
- **Debug**下有**DrawRay()方法**绘制射线
- **Physics**下有**Raycast()方法**检测是否在一定范围内碰到东西
- ![](https://upload-images.jianshu.io/upload_images/6322775-bfd6f1caf475a6a2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
- ![拾取功能](https://upload-images.jianshu.io/upload_images/6322775-6604b6c74a92a17b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
12. **开门功能**
- ![hinge.spring = spring是固定用法](https://upload-images.jianshu.io/upload_images/6322775-72eed764ca68ca0c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
- **HingeJoint**有特殊用法，需要配合**JointSpring**使用
- ![Raycast使用RaycastHit是固定用法](https://upload-images.jianshu.io/upload_images/6322775-553dd9ea31b16fe7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
- ![开关门功能](https://upload-images.jianshu.io/upload_images/6322775-635f37cf8e47f8e8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
- ![三元运算符](https://upload-images.jianshu.io/upload_images/6322775-09fdcfe8bb20e560.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
13. 播放**音效**方式
- 一种是跟哪个**物体**有关就在上面添加音效；
另一种是创建一个**类**管理和播放所有音效