## 环境搭建

- [文档](https://flutter.cn/docs/get-started)

- 安装flutter会自带dart环境，无需单独安装dart环境
- 注意！使用VSCode安装Flutter SDK时，必须使用编辑器中打开的文件目录，如当前打开的是`D:\test`文件夹，就不能安装到`E:\`下
- 注意！官方文档没有写要安装JDK，但是实际运行项目必须要安装JDK，flutter`3.19.6`版本需要JDK 11版本
- 注意！如果使用真机调试，必须安装对应ADK，在AndroidStudio中打开`Virtual Device Manager`查看当前连接设备的`API Level`，即表示所需ADK相关所有`platform tool`都得是对应版本
- 在命令行中通过`dart --version`检查

- 创建项目，以`VSCode`为例

  1. 在编辑器中使用`ctrl + shift + P`打开命令行
  2. 输入`flutter`
  3. 选择`Flutter: New Project`创建新项目
  4. 会弹出提示`Which Flutter Project`(运行什么类型项目:PC应用、移动应用、网页)，选`Application`
  5. 为新项目选择一个文件夹存放
  6. 输入项目名
  7. 等待项目初始化创建完成
  8. 检查开发配置
     1. 运行`cmd`
     2. 执行`flutter doctor`

- 运行项目

  1. 编辑器下使用`ctrl + shift + P`打开命令行

  2. 输入`flutter`

  3. 选择`Flutter: Select Device`

     - 进行这步前先将设备或模拟器运行起来

  4. 选择设备

  5. 按`F5`运行项目

     - 运行项目会自动下载`gradle`，但因为墙的原因，压缩包往往都是损坏的，可以通过手动下载解决

       1. [gradle全版本下载](https://services.gradle.org/distributions/)

       2. 根据报错信息找到文件目录
          - 如`C:\Users\admin\.gradle\wrapper\dists\gradle-7.6.3-all\aocdy2d2z8kodnny3rsumj8i8`

       3. 将下载的压缩包在对应目录下解压

     - 运行项目进度卡在`Running Gradle task 'assembleDebug'`，解决办法

       - 因为核心构建库Gradle在国外，使用阿里云镜像

       1. 修改项目中`android\build.gradle`文件

          ```gradle
          buildscript {
              repositories {
                  // google()
                  // mavenCentral()
                  // 注释掉改成如下内容
                maven { 
                  allowInsecureProtocol = true // 屏蔽安全警告
                  url 'https://maven.aliyun.com/repository/google'
                }
                maven { 
                  allowInsecureProtocol = true
                  url 'https://maven.aliyun.com/repository/jcenter'
                }
                maven { 
                  allowInsecureProtocol = true
                  url 'http://maven.aliyun.com/nexus/content/groups/public'
                }
              }
              dependencies {
                  classpath("com.android.tools.build:gradle:7.3.0")
              }
          }
          
          allprojects {
              repositories {
                  // google()
                  // mavenCentral()
                maven { 
                  allowInsecureProtocol = true
                  url 'https://maven.aliyun.com/repository/google'
                }
                maven { 
                  allowInsecureProtocol = true
                  url 'https://maven.aliyun.com/repository/jcenter'
                }
                maven { 
                  allowInsecureProtocol = true
                  url 'http://maven.aliyun.com/nexus/content/groups/public'
                }
              }
          }
          ```

       2. 修改`flutter SDK`安装目录下的`packages\flutter_tools\gradle\flutter.gradle`文件

          - 新版Flutter SDK配置不在`flutter.gradle`，迁移到了`packages\flutter_tools\gradle\src\main\groovy\flutter.groovy`

          ```gradle
          buildscript {
              repositories {
                  // google()
                  // mavenCentral()
                  // 同样将其中的google等注释掉 添加如下内容
                  maven { 
                    allowInsecureProtocol = true
                    url 'https://maven.aliyun.com/repository/google'
                  }
                  maven { 
                    allowInsecureProtocol = true
                    url 'https://maven.aliyun.com/repository/jcenter'
                  }
                  maven { 
                    allowInsecureProtocol = true
                    url 'http://maven.aliyun.com/nexus/content/groups/public'
                  }
              }
              // 保留
              dependencies {
                  classpath("com.android.tools.build:gradle:7.3.0")
              }
          }
          // 将DEFAULT_MAVEN_HOST变量修改为"https://storage.flutter-io.cn"
          class FlutterPlugin implements Plugin<Project> {
          	private static final String DEFAULT_MAVEN_HOST =    
          	"https://storage.flutter-io.cn";
          }
          ```

       3. `Running Gradle task 'assembleDebug'`失败后报错缺少`Build Tools revision 30.0.3`

          - 在Android Studio 的`SDK Tools`中安装

       4. 运行Android项目时，需要运行Android SDK中的`adb(Android Debug Bridge)`命令行工具

          - 如果安装过Android SDK，配置环境变量，在`Path`中添加`%ANDROID_HOME%\platform-tools`和`%ANDROID_HOME%\tools`
            - 注意！不要用网上`path1;paht2`的形式在window11中配置环境变量，会导致无法识别`adb`命令

       5. 执行`Installing ...apk`时报错

          - 这是因为真机调试时手机USB调试权限未全部开启，导致apk应用无法安装到手机