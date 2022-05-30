1. Label控件，显示文字
2. Gizmos
- 点击摄像机图标隐藏
3. sprite创建精灵，显示图片
4. panel 面板，控件容器
5. button想要起作用
- 需要添加Box Colider`Attach--Box Colider`,用Colider检测点击
- 添加点击颜色`Attach—Button script`
- 需要单独建脚本写方法，在On Click上指定脚本和方法
- 在组合按钮中要显示效果，需要建两个`Button script`
6. 创建图集
- 用shift键可以多选图片
- 右击图集的prefab可以编辑
7. Slice
- 只拉伸中间部分
8. 自适应(Anchor)
- 使画面始终居于固定位置
9. Tween动画
10. Slider滑动器
- 右击目标`Attach--Box Colider`(交互控件都需要这么做，添加了Box Colider后就可以添加交互功能了)
- 右击目标`Attach--Slider script`
- 在On Value Change里挂载GameObject来传值
11. 用enum枚举类型制作选项
- ![enum枚举类型的使用](https://upload-images.jianshu.io/upload_images/6322775-07b37ea4c4f381a0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
12. 下拉框
- **Popup List Script**
- 再创建一个Label标签，绑定到`On Value Change`，选择*setCurrentSelection*
13. 单选框(**Toggle script**)
14. NGUI组件上都有绑定脚本的功能(绑定到On Value Change)，可以选择方法
15. 声明一个对象，将GameObject绑定在上面来获取值，再赋值给当前脚本变量
16. 控件上的Label需要绑定到**On Value Change**选择**CurrentSelect方法**
17. ![Switch用法](https://upload-images.jianshu.io/upload_images/6322775-f9179fbb63f12167.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
18. NGUI中经常会有**空格、回车**影响**条件判断**，所以使用**Trim()**去除
19. 界面切换动画
- 添加**Tween--Position**，将不同界面移到显示框外看不见的地方
- 添加**方法**`TweenPosition.PlayForward()/PlayReverse()`播放切换界面动画(声明TweenPosition类型的对象)，绑定到对应按钮**On Click**
20. 技能CD效果
- 添加**两个图片**—**重叠**(用来遮盖的图片**Type**设置成**Filled**)—在**Color Tint**修改透明度—在技能图片下添加脚本
- ![](https://upload-images.jianshu.io/upload_images/6322775-8a717a2f69e2eb32.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
- ![](https://upload-images.jianshu.io/upload_images/6322775-4d37c72491b9a2f1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
- ![](https://upload-images.jianshu.io/upload_images/6322775-57541a1953fd458c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
21. NGUI大部分组件都有可以**绑定脚本选择方法**的地方
22. `int.parse()`方法可以把**字符串类型**转换成**int类型**
23. **手动**添加**Drag脚本**可以给控件添加**拖拽功能**
24. 滚动条(Scroll Bar)
- 和滑动条一样，移动范围要扩展到和背景图一样
25. 聊天窗口(**Text List**)工具
- 将Label与**滚动条关联**起来
- 自带绑定滚动条位置
- 需要手动添加**Text List脚本**
- 手动创建滚动条
26. **Drag Resize**给予附在框体上的部件以**调整框体大小**的功能
- **手动添加Drag Object**给予拖拽框体位置的功能
27. **对话框**
- **UIInput**—**On Return Key**—**submit**—**绑定On Submit方法**
- **注意：**
- ![](https://upload-images.jianshu.io/upload_images/6322775-99ebcedbb583af08.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
- ![string数组](https://upload-images.jianshu.io/upload_images/6322775-4806238dddc51e39.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
28. 拖拽功能和事件监听
- **继承**自哪个**父类**就具有它的**功能**
- **重写：Override**![识别放置位置](https://upload-images.jianshu.io/upload_images/6322775-a3616d4e453c2662.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
- “装备”在“物品栏”中的坐标是**相对坐标**且是**（0，0，0）**
29. **继承方法重写**
- 在**父类**里用**Virtual声明虚函数**，在**继承的子类**里用**Override重写**
- 用**public new 函数名**可以**避免多态**
30. `transform.parent`**改变**上一级**关系
- parent是**父目录**
- ![](https://upload-images.jianshu.io/upload_images/6322775-a146f3fa992a9abe.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
31. 物品拖拽交换
- `transform.childCount`可以显示一个物体下有**多少**个**子物体**
- 添加**标签**，用中间变量**Temp**，交换两个的位置
![](https://upload-images.jianshu.io/upload_images/6322775-f942cf0bf0cc82ce.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
32. 拾取物品
- **string[]**是作为Index来**检索图片**，因为可以根据**spriteName(图片名称)**改变图片
- **item**是**预设**的Prefab
- **NGUITools.AddChild()**可以添加GameObject作为**子物体**
- ![注意：break](https://upload-images.jianshu.io/upload_images/6322775-aaa87373ef6d1d20.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
- 物品**堆叠**：**GetComponentInChildren**是查找**子物体**上的**组件**
- ![](https://upload-images.jianshu.io/upload_images/6322775-93a32dbb3007c469.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
- ![](https://upload-images.jianshu.io/upload_images/6322775-000bb490c4a7f709.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
33. **血条**
- 使用**Progress Bar Script**
- 因为血条不需要**交互**所以**不**需要**Box Collider**
- 使用**HUD Text插件**（NGUI拓展插件）
- **使用说明：**选中血条，添加**FollowTarget脚本**，在人物上方添加一个GameObject
- 不只是血条，二维平面的东西都可以用
- HUD Text里有个**Add方法**可以显示**伤害/治疗效果**