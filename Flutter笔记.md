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
  4. 注意第一次运行Flutter项目，还会下载`wrapper`以外其他文件，这时很容易下载失败，因此需要修改DNS为`114.114.114.114 8.8.8.8`才能保证下载，之后就是安装apk
  
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

## 设计理念

- Flutter 的设计哲学与 Web（CSS）不同
  -  Web 中，几乎所有元素都有 `margin` 和 `padding` 属性
  - Flutter 中，UI 是由一个个独立的 Widget 组合而成的。一个 Widget 本身不具有 `margin` 属性，你需要用一个带有 `margin` 功能的 Widget 来包裹它

## 路由

- 何时使用？
  - 用页面索引切换显示适合平级页面
  - 有层级深入的适合路由

### 基础路由

```dart
// 适用于大多数简单页面的跳转 基于堆栈 新页面覆盖在旧页面之上
// 从 A 页面跳转到 B 页面
Navigator.push( // 使用 Navigator.push 方法
  context, // 注意要传入build中的context
  MaterialPageRoute( // 传入 MaterialPageRoute 实例
    builder: (context) => const BPage(), // builder 参数用于构建目标页面
  ),
);
// 从 B 页面返回到 A 页面
Navigator.pop(context);

// 父 -> 子 传值就用构造函数 子 -> 父 返回数据使用pop
// 注意 如果是 StatefulWidget 组件传值 子组件使用 widget. 来获取构造函数传入的值
// B 页面，返回时传递数据
onPressed: () {
  Navigator.pop(context, '从 B 页面返回的数据');
}

// A 页面，接收返回的数据
onPressed: () async {
  // 接收push的返回值
  final result = await Navigator.push(
    context,
    MaterialPageRoute(builder: (context) => const BPage()),
  );
  print('接收到 B 页面返回的数据: $result');
}
```

### 命名路由

```dart
// 当应用页面较多时 管理和维护路由关系 避免页面间的直接依赖
// 定义两个页面
class HomePage extends StatelessWidget { /* ... */ }
class DetailPage extends StatelessWidget { /* ... */ }

void main() {
  runApp(MaterialApp(
    initialRoute: '/', // 初始路由
    routes: { // 管理路由
      '/': (context) => const HomePage(),
      '/detail': (context) => const DetailPage(),
    },
  ));
}
// 跳转到 /detail 路由 使用 Navigator.pushNamed 方法
Navigator.pushNamed(context, '/detail');

// 因为不再用构造函数跳转 因此使用pushNamed参数传值
// A 页面 跳转时传递参数
Navigator.pushNamed(
  context,
  '/detail',
  arguments: '来自 A 页面的参数',
);

// B 页面，接收参数
class DetailPage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    // 获取参数 此处示例因为直接传入Text中作为参数因此加了as String断言
    final String? args = ModalRoute.of(context)?.settings.arguments as String?;
    return Scaffold(
      appBar: AppBar(title: Text('详情页')),
      body: Center(
        child: Text(args ?? '没有参数'),
      ),
    );
  }
}

// B向A返回数据 同样用 await Navigator.pushNamed
```

### 高级路由

- `routes`和`onGenerateRoute`只存在一个

```dart
// 需要复杂逻辑的路由 如：登录验证、权限控制
MaterialApp(
  onGenerateRoute: (RouteSettings settings) {
    // 获取路由名称和参数
    final name = settings.name;
    final arguments = settings.arguments;
	if(检查用户登录状态){
        switch (name) {
          case '/':
            return MaterialPageRoute(builder: (context) => const HomePage());
          case '/detail':
            return MaterialPageRoute(
              builder: (context) => DetailPage(arguments: arguments),
            );
          default:
            // 如果路由不存在，返回一个错误页面
            return MaterialPageRoute(builder: (context) => const UnknownPage());
        }
    } else {
        return MaterialPageRoute(builder: (context) => const LoginPage());
    }
  },
);
```

### StatefulWidget路由更新机制

```dart
// StatefulWidget 在更新时 只会重新创建 class MainApp extends StatefulWidget 实例
// 而会复用 class _MainAppState extends State<MainApp> 因此 StatefulWidget 作为路由页
// 在重新加载时不会更新内部状态
// 注意！但是像 StatelessWidget 一样直接widget.是会更新的
// 因此就需要实现 didUpdateWidget 方法来更新内部状态
class _MyCounterPageState extends State<MyCounterPage> {
  int _count = 0; // 内部状态
    
  @override
  void initState() {
    super.initState();
    _count = widget.initialCount; // 内部状态初始值
  }

  // 当父组件重建并传入新的 MyCounterPage 实例时，此方法会被调用
  @override
  void didUpdateWidget(covariant MyCounterPage oldWidget) {
    super.didUpdateWidget(oldWidget);
    
    // 检查新的值和旧的值是否不同
    if (widget.initialCount != oldWidget.initialCount) {
      // 如果值发生变化，更新子组件的内部状态
      setState(() {
        _count = widget.initialCount;
      });
    }
  }

  @override
  Widget build(BuildContext context) {
    return Text('当前计数: $_count');
  }
}
```

### 返回多级路由

```dart
// 使用 popUntil 第二个参数只要为 true 就停止
Navigator.popUntil(context, ModalRoute.withName('/b')); // 路由出栈 当找到 withName 相匹配的停止
Navigator.popUntil(context, (route) => route.isFirst); // 回到根路由
Navigator.popUntil(context, (route) => route.settings.name == '/b');
```

## StatefulWidget与StatelessWidget

### context的区别

- 因为`StatefulWidget`需要`extends State<T>`，而`State`对象有一个内置的`context getter`，因此在`build`方法外也可以直接使用`context`
- 而`StatelessWidget`没有，只能在`build`方法中才能获取到`context`

## 架构范例

- 目录结构

```
lib/
├── main.dart                  // 应用入口
├── DataModels/                // 数据层（Repository）
│   └── API/
│   └── models/
│       └── user.dart
├── ViewModels/                // 业务逻辑层
│   └── user_notifier.dart
├── View/                      // UI 层（页面和组件）
│   ├── pages/
│   │   └── user_page.dart
│   └── widgets/
│       └── user_view.dart
├── Providers/                 // Riverpod Provider 注册
```

- 数据模型（user）

```dart
class User {
  final String id;
  final String name;

  User({required this.id, required this.name});
  // 加factory(工厂构造函数)关键字是为了能返回User构造函数 否则 构造函数必须初始化当前类的属性
  // Dart中单例模式就是用factory
  factory User.fromJson(Map<String, dynamic> json) =>
      User(id: json['id'], name: json['name']);
}
```

- 数据层（Repository）

```dart
class UserRepository {
  Future<User> fetchUser() async {
    await Future.delayed(Duration(seconds: 1)); // 模拟网络延迟
    return User(id: '001', name: 'Alice');
  }
}
```

- 业务逻辑层（ViewModel）

```dart
class UserNotifier extends StateNotifier<AsyncValue<User>> {
  final UserRepository repository;

  UserNotifier(this.repository) : super(const AsyncValue.loading()) {
    loadUser();
  }

  Future<void> loadUser() async {
    try {
      final user = await repository.fetchUser();
      state = AsyncValue.data(user);
    } catch (e) {
      state = AsyncValue.error(e);
    }
  }
}
```

- 状态管理注册

```dart
final userRepositoryProvider = Provider((ref) => UserRepository());

final userNotifierProvider =
    StateNotifierProvider<UserNotifier, AsyncValue<User>>(
        (ref) => UserNotifier(ref.watch(userRepositoryProvider)));
```

- 页面和组件

```dart
// 页面
class UserPage extends ConsumerWidget {
  @override
  Widget build(BuildContext context, WidgetRef ref) {
    final userState = ref.watch(userNotifierProvider);

    return Scaffold(
      appBar: AppBar(title: Text('用户信息')),
      body: userState.when(
        data: (user) => UserView(user: user),
        loading: () => Center(child: CircularProgressIndicator()),
        error: (err, _) => Center(child: Text('加载失败: $err')),
      ),
    );
  }
}
// 组件
class UserView extends StatelessWidget {
  final User user;

  const UserView({required this.user});

  @override
  Widget build(BuildContext context) {
    return Center(child: Text('欢迎你，${user.name}'));
  }
}
```