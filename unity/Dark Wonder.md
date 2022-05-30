1. 所有的component（游戏内置的，脚本）通过.gameobject访问到该component所挂在的游戏物体
2. update函数中用`Time.deltaTime` 上一帧到这一帧的时间间隔
- 计时功能
3. 如何查找GameObject
- 直接拖拽
- gameobject.Find(最耗时,通过名字查找)
- FindGameObjectWithTag(通过标签查找，标签集合)
![](https://upload-images.jianshu.io/upload_images/6322775-3b13ba0997710397.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
- FindWithTag（单独标签时使用）
> 返回值是GameObject
4. `float`类型格式化方式`.ToString("0.00")`表示保留小数点后两位
5. 总面板下的GameObject都是世界坐标（**position**）
GameObject下的**子物体**是本地坐标（**localposition**）
6. { get; set; }
- get;说明只能取值
- set;说明只能赋值
7. forward
- Vector3.forward 是(0,0,1)Z轴为1的前向量
- transform.forward 是当前GameObject所朝向的‘前’向量
8. Quaternion四元数
- 表示旋转，如果是`Quaternion.identity`则表示不进行旋转
9. ![](https://upload-images.jianshu.io/upload_images/6322775-273c539be405e7c0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
- Positive Button表示按键（半轴）
Alt Positive Button表示额外按键
10. 把脚本中的**变量**设为**静态**可以在外部调用
- 或者把**脚本**做成**单例**也可以
11. 在被摧毁的对象底下不能使用摧毁它的代码
12. 获取组件的方法除了直接在GameObject下创建脚本来获取组件，还可以声明变量来绑定
13. GUI不显示在Scene视图下，而是在game视图下调整
14. GameObject下的方法`activeInHerarchy`可以获取到是否处于激活状态
15. 任何一个**数字类型**加上一个`""`返回的都是字符串
16. 可以通过获取组件后反过来获取GameObject的控制
17. 使用
```
screen.width
screen.height
```
获取屏幕的高度和宽度
18. 
```
GameObject.SendMessage(string methodname)
```
会搜索这个GameObject下所有Component中叫做methodname的方法然后调用
19. **OnTrigger方法的形参other指的是碰撞双方中没有携带OnTrigger方法的一方**
- 如果，脚本和触发器不在一个GameObject下，OnTrigger方法是没有用的
- 脚本和触发器不在一个GameObject下，OnTrigger检测的会是只有触发器的GameObject
20. 跟踪方法
- 选择一个方法，按F12
21. 播放音乐
- 声明多个audio变量，来绑定音频文件
- 写一个脚本放置播放音乐的方法，在其他脚本中调用