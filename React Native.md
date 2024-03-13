## 文档

- [中文网](https://www.reactnative.cn/)

- 不同版本RN要求的Node、JDK、Android SDK版本不同
  - 如0.73版本的RN要求Node版本大于18，JDK版本为17，Android SDK版本为13。低于0.73版本的RN需要JDK11，而低于 0.67 的需要 JDK 8 版本
  - Android Studio环境配置
    1. 在菜单`Tools`下找到`SDK Manager`，选择`SDK Platforms`选项卡，展开`Android 13 (Tiramisu)`选项，勾选`Android SDK Platform 33`组件，勾选右下角`Show Package Details`
    2. 然后点击`SDK Tools`选项卡。同样勾中右下角的`Show Package Details`，展开`Android SDK Build-Tools`选项，选中RN所需的`33.0.0`版本
    3. 配置`ANDROID_HOME`环境变量
       - 菜单Tools→`SDK Manager`，找到`Appearance & Behavior`→`System Settings`→`Android SDK`查看 SDK 真实路径
       - 打开`控制面板` -> `系统和安全` -> `系统` -> `高级系统设置` -> `高级` -> `环境变量` -> `新建`，创建一个名为`ANDROID_HOME`的环境变量（系统或用户变量均可），指向你的 Android SDK 所在的目录
    4. 安装Android Studio只是为了安卓运行环境
- 参考官网`环境搭建`→`搭建开发环境`配置环境和初始化项目

