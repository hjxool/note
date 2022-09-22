### 文件结构

#### 	public：

​		存放html文件以及ico图标

##### 		TIps：

​			1、小技巧将外部引入的文件库放在public里，因为这里有HTML文件，通过`**<link>**`引入的样式就不会被vue`严格审查`，从而避免添加一些不需要的附属文件库

#### 	src：主目录

​    	存放组件文件和静态资源、图片，文件入口

​		放`main文件`和组件管家`app组件`

​    	**assets:**放图片等静态资源

​    	**components：**子组件存放位置

​		**.gitignore：**git上传时要忽略的文件

​		**babel.config.js：**babel语法转换(?)，不用管，就是es6转es5的配置功能

​		**package.json：**版本、使用的依赖库、常用命令等信息，使用npm就有这些东西

​		**package-lock.json：**包管理文件，用于锁定依赖版本

------

### Vue API：

​	mixins()：在`**main.js**(全局配置引入的地方)`中用`**Vue.mixin**`导入外部文件暴露的内容，即全局方法和属性；在`**.vue**`文件中用`**mixins:[ ]**`将外部文件混合进当前组件；或者`Vue.mixin({ data(){return { } } })`

​	**use()：**(很强大！)在外部文件中定义一个`**关键字install()**`的函数，在main文件中`import xxx`，再使用`Vue.use(xxx)`时，会给这个函数传入一个`**默认参数**`，这个参数就是`**vue构造函数**`，因此可以在外部定义的这个函数中用`**默认参数.API**`定义`**全局**`的方法

------

### 样式：

​	因为组件都是分开定义书写，所以难免出现class名冲突的情况，你在这个组件中定义的在其他组件可能意外重复，所以就需要`**style标签中加入scoped关键字**`，它可以将样式划分成作用域，只服务于当前组件内的标签

------

### 组件间传参：

#### 	最基础的方法

​		父亲给儿子传，子组件用`props`接收，而儿子给父亲传，则需要父亲先将一个函数传入子组件，子组件中再调用该方法，将值传入，因为在js中，**函数**也是`**对象**`，传递的函数实际上是传递`**引用地址**`，表明它们`**维护同一块内存地址**`，一方修改另一方也会改变

#### 	全局事件总线

#### 	vuex

##### 		安装：

​			**npm i vue@3**(现在默认是vue3，而vue2.x只能用vuex3，vue3使用的是vuex4)——在·**src**·文件夹下创建·**store/index.js**·——创建三个对象(建议叫**actions**、**mutations**、**state**)，依次用于发起commit请求、实际操作、存储公共数据——import vue和vuex(因为要用到use()应用插件)——·**Vue.use(Vuex)**·只有应用插件后才能·new Vuex·——export defalut ·**new Vuex.Store({ actions，mutations，state** **})**·(属性和变量名相同触发简写)，到此就已经准备好使用了

##### 		基础用法：

​			1、当需要通过commit进行判断、请求时：actions中定义判断方法，其中会传入两个默认参数，context和value，前者是vuex组件的迷你版，里面有一些常用的方法，后者是互动功能传入的值——判断方法中执行·**context.commit('mutations中定义的函数**(建议同名大写跟actions中的方法做区分)**'，value)**·——数据传递到**·mutations·**中，在这里定义一个方法用来接收，默认传入两个参数，**state**和**value**，前者是·**state**·对象，后者是·**commit**·方法传递进来的参数(如果commit传进来的不是value是自定义值，那么**mutations**接收到的就是自定义值)——使用state中保存的值进行计算——在组件中使用·**this.$store**(自定义vuex对象)**.dispatch('actions中的方法名'，值)**·

​			2、当需要略过commit直接进行操作：直接在组件中写·**this.$store.commit('mutations中的方法名'，值)**·

##### 		getters：

​			跟计算属性同样功能的对象，里面的方法必须用·**return**·返回一个值，默认传入的参数是·**存储公共数据的state对象**·

##### 		归类命名空间：

​			当公用空间很杂时，用不同的对象保管**actions**等对象更便于维护

###### 			归类时会有以下不同：

​				1、归类的对象中必须各自拥有**actions**等功能对象，且必须用关键词·**namespaced：true**·

​				2、·**new Vuex.Store**·中不再使用actions等属性接收自定义对象，而是用·**modules:{ 自定义名：归类对象，... }**·来管理

​				3、取用·**state**·中的值不再需要·**$store.state.xxx**·，而是·**$store.state.a.xxx**·（注意a下没有state）

​				4、使用·**commit**·等方法时，不再是·commit('mutations中的函数'，value)·而是·**commit('归类对象/**mutations中的函数**',value)**·

​				5、·mutations·和·getters·中的函数默认参数时·**归类对象下管理的state**·

------

### 解决跨域问题：

​	通过代理服务器

![img](https://upload-images.jianshu.io/upload_images/6322775-4bc19d088d9b4e1c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在vue.config.js中配置

​	**`/xxx**`是必须要写的，不写代理会查找自己自身的文件，而不会去发出代理。**但是**这就有一个问题——代理发给服务器的是`**/xxx/服务器路径**`的路径，而服务器上可能只有`**/服务器路径**`，这种情况下就需要`**pathRewrite:{' 正则查询条件 ':' 查询到后要修改的值 '}**`，将代理路径`**/xxx/服务器路径**`在**发送到服务器时**改为`修改后的值(可以为空)**/服务器路径**`。

#### 	TIps：

​		1、当然也可以在`/api`这块位置直接写服务器**根路径**，就不用重写地址了

​	`**proxy**`中可以配置多个服务器，以对象的形式配置参数

​	`**target**`只写**端口号**和**IP地址**

​	在发送请求时，直接写`**本地IP:本地端口/xxx/服务器路径**`

------

### 切换显示vue router：

#### 	安装：

​		·npm i vue-router@3(默认vue3，安装未指定版本会自动下载router@4的版本)·——在·**src**·下创建·**router/index.js**·，导入·**VueRouter**·，以及想要·切换显示的**组件**·，再暴露·**new VueRouter**·，写入配置项·**routes:[ { path：'/xxx'，component：'导入的组件' } ]**·——在·main.js·导入·**VueRouter**·以·**router/index.js**·——此时·**new Vue**·对象中就多了一个新的配置项·**router**·，将导入的·**router/index.js**·配置到其中

#### 	使用：

​		在导航栏处用·**<router-link to="/xxx" active-class="选中时样式">**·书写——在需要·切换显示·的地方用·**<router-view>**·——当点击不同的·**<router-link>**·的就可以随意切换

#### 	传参：

​		在·**<router-link :to="`/xxx?id=${params}`"**·中用·query·形式传参，即问号后跟参数。传过去的参数在·**this.$router.query**·中保管

#### 	命名：

​		在配置路由路径时，使用·**name**·属性，可以简化书写路径的代码，vue会自动读取name代表的路径组件。

##### 		使用方法：

​			在html页面用·**name:xxx**·来使用，注意！必须在·**:to="{ name: }"**·to的对象形式中才能使用