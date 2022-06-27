## 变化：

1. vue3中·**<template>**·下不再需要·**根元素**·
2. vue3中不再需要data、methods等函数，而是在一个全新的·**setup(){}**·**函数**中像传统js语句那样去声明；·setup(){}·必须有·**返回值**·，可以写成两种形式——对象形式(主要)、渲染函数；对象形式——·**return { name，function }**·，对象形式中包含的属性和方法均可直接使用，比如“{{name}}”、“@click='function'”

   - Tips：

     - setup函数有两个默认参数——props、context。props用来接收从父级传入的参数，同以前的props配置项。context有三个主要的属性——①attrs：用来兜底，当传入的参数没有用props声明接收，就可以在·attrs·中找到；②**emit**：**触发**父级绑定到子组件的事件，使用方法同vue2中的·**$emit**；③**slot**：接收父级传入的插槽内容，**注！**vue3中插槽必须用·**<template v-slot:name/>**·的形式，slot才能接收到·**{ name:f }**·的插槽内容

     - setup作为一个**配置项**，那生命周期等**平级**配置项中如何调用setup中的数据呢？答案是——用·**this**·，因为·**setup**·执行顺序在·**beforeCreate**·**之前**，因此，在·**setup内部****this还没有指向vm实例对象**·，而是用JS原生作用域的原理取值。注：经过实验，**setup**中**的this**指向**window**对象，**生命周期钩子**中**this指向vm**实例对象，且**拥有setup**中暴露出来的**变量**和**函数**，直接用·**this.xx**·即可调用**
3. vue3中·自己声明·的属性变量·不再具有··**响应性**·，而是要通过一个·**ref函数**·去加工成·**RefImpl对象**·，即引用实例对象

   - 使用方法：

     - 使用前必须要·**import { ref } from 'vue'**·，引入ref组件

     - ·**let xxx = ref( '张三' )**·，使用ref函数处理赋值的变量

     - 修改时使用·**xxx.value = '李四'**·，此处vue3为value属性做了个数据劫持。**※但是**，页面引用时不需要用·.value·，vue自动进行了一个·.value·取值操作

     - **但是**ref在处理对象数据类型时，只会把对象本身用数据劫持绑定get和set方法，其下的属性都是用·**ES6中新语法proxy**·处理，因此在取对象第一层数据时需要·**obj.value.xxx**·，其后的层级数据直接取用
4. 父级给子组件绑定的·自定义**事件**·，·**子组件**·必须要用一个全新的配置项·**emits:[ 'event' ]**·来声明接收
5. vue2中[全局方法]()的==Vue.component/directive/...==，现在使用实例对象==vm.directive/...==就可以实现
6. vue2中==安装全局事件总线==的[Vue.prototype.xxx]()变为了[vm.config.globalProperties]()
7. vue3中==data选项==必须写为==函数形式==


## 创建组件

![](https://upload-images.jianshu.io/upload_images/6322775-5708bc097d7f0416.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

​	要用对象的形式，对象名就是组件标签名称

​	创建vue实例对象

![img](https://upload-images.jianshu.io/upload_images/6322775-71d852e4d355f256.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

​	新增 `component` 属性挂载需要的组件

​	挂载vue实例：`Vue.**createApp**(vue实例对象名称).**mount**('所要挂载的html元素ID或者class')`

------

vue3方法创建的实例对象，不能直接使用，只有在 `mount()` 挂载后所赋值的变量

![img](https://upload-images.jianshu.io/upload_images/6322775-67f17ff8c53c9978.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

才能取到里面的值，等同于Vue2中的let vm = new Vue

**挂载**标签元素的方法：el="class或id" / vue实例.**$mount**("class或id")

------

使用防抖动函数可以直接用 `_.debounce` 包裹

![img](https://upload-images.jianshu.io/upload_images/6322775-fbc74be1f759fd40.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

使用防抖函数包裹function会改变原有 `xxx(){}` 的写法

## 组件新写法：

![img](https://upload-images.jianshu.io/upload_images/6322775-98fc500bb756ae16.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在 <head> 内写 <script type=“自定义” id="组件挂载id">

​    组件中为什么要把data写成是函数形式——因为如果是data对象，则在不同组件调用时都在用同一个地址引用，即会发生同一组件的复用在更改不同位置的值时，会同时发生变化。而“data”使用“函数形式”，返回的“对象”，在每次调用“data()”时，返回的都是一个新的对象（虽然其内容一样，但是地址不同）

------

![img](https://upload-images.jianshu.io/upload_images/6322775-c175112bac13a75b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

vue时间修饰符

## vue2组件传值进化

vue2.x中的“provide/inject”在vue3中进化了，更增加了**响应性**

![img](https://upload-images.jianshu.io/upload_images/6322775-b14144c00b89de88.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

依靠跟“计算属性”组合

![img](https://upload-images.jianshu.io/upload_images/6322775-0bc7392462742f06.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

“响应式”下，inject的使用稍有不同，注意红框中的内容，传递过来的值是一个对象，需要提取其中的值

![img](https://upload-images.jianshu.io/upload_images/6322775-1fc48098f2331824.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

可以看到打印出来的值是个对象，只有value里面才是传进来的值

​    现在的vue3还不是正式版本，“inject”传入的值需要“.value”，不然会有双引号

- 组件式写法

  - ```js
    上级组件：
    	import {reactive,provide} from 'vue'
    	let xxx = reactive({aaa:'asdas',bbb:'2222'})
        provide('键名'，xxx)
    子级组件：
    	import {inject} from 'vue'
    	let xxx2 = inject('键名')
        直接调用：xxx2.aaa/xxx2.bbb
    ```

## vue3响应式

- 加工对象类型的数据，使用·**reactive函数**·，加工成·**proxy对象**·
  - 使用方法：

    - 使用前·**import {reactive} from 'vaue'**·

    - ·**let obj = reactive({ xxx:'李白'，age：18 })**·

    - reactive**不能**处理·**基础数据类型**·


## toRef/toRefs

- 当想用·**简写**·形式，而不是·对象.xxx.xxx·时，会进行这么个操作·**name:person.name**·，而这个操作取出的数据赋值给新变量，其实只进行了基础数据类型的赋值，就·**失去**·了·**响应性**·。·**toRef**·的作用正在于此，它将对象中的·基础数据类型·与·源对象·进行了一次·桥接，使其进行了关联，操作·**name:toRef(person.name)**·中的·**name**·不是一个·**新的变量**·，而是·响应式·的从·**源对象**·身上得到的
  - ·**toRefs**·使用方法：在setup()返回值中·return { **...**toRefs(**对象**) }·，在HTML结构中用·{{对象中**第一层**的**基础类型数据**}}·，直接取用，而不需要·object.xxx·这样来使用。但是·**仅限第一层**·，其下的对象依然是·proxy·

## 计算属性

- vue3中计算属性也变成了·组合式API·，即需要import导入后才能使用 ，使用方法——在·**setup**·函数中用·**函数**·的形式·**computed()**·，**简写**形式：·**reactive对象.xxx = computed(回调函数)**·，简写形式只能读不能写。**完整**写法：·**reactive对象.xxx =** **computed(对象)**·，对象中写·**get()，set()**·函数，在页面上调用时用·**{{reactive对象.xxx}}**·来使用

## watch监听器

- 使用import导入·**watch方法**·，与computed不同的是watch是基于已有的属性，而不是创建新属性，所以不用赋值操作。使用方法——·**watch(单对象/数组，回调，配置项)**·，第一个参数可以是·**setup**·中的单个属性，也可以是·**一组**·属性，比如·**watch([ sum,name ])**·，可以同时监视多个对象；第二个参数是回调函数，里面默认有·**修改前、修改后**·两个参数，比较特殊的是，在监听·一组·数据时，回调中的参数变成·数组形式·，即修改前/后两个参数值变成·**数组**·
  - Tips：
    - **注！**监听·**对象**·时，因为对象是引用类型，虽然监听到了变化，但是·**newValue、oldValue**·是对象的·**引用地址**·，对于目前JS来说，除了自己手写循环遍历，是无法简单的用API实现对象的深拷贝的，因此·newValue、oldValue·都指向同一个地址，oldValue和newValue的·**值也是一样的**·
    - 对于·**reactive对象本身**·，默认开启深度监听，对于·reactive对象**下的某一层级对象**·，则需要手动开启监听·watch(xxx，回调，**{ deep:true }**)·
    - 监听·reactive**对象下**的**某一**/些**属性**·，得用·watch(**()=>object.xxx**，回调)·因为vue3中的watch只能监听·**ref、reactive、数组**·


## vue3新增watchEffect：

  不指定监听对象，只指定回调，当使用和依赖数据变化时会触发回调

  使用方法：·**watchEffect(()=>{ const a = 外部数据 触发时执行逻辑 })**·，当·**a**·依赖的外部数据变化时，执行**通用**逻辑，这点跟**计算属性**很像。

  与**watch**的**不同**：watch是每个监听对象，都可以有不同的回调；而·watchEffect·是共用回调

## vue3新增hook函数：

  简单来讲就是将主干部分写的复用功能放到外部js文件中，再通过import导入使用，乍看似乎就是封装方法复用，平平无奇，但是在vue3之前或者JS原生方法中，封装的方法无法做到响应性

  Tips：

​    1、因为export导出的是一个函数，因此必须要有返回值以供其他组件使用

## vue3新增teleport组件

- 作用：将[<teleport>标签]()包裹的内容用`<teleport to="body/#id/.class">`传送到指定容器下，teleport内的标签内容，不论在什么元素内，都已==to==后传送的位置为==父节点==来计算定位

## vue3新增suspense组件

- 当使用异步加载组件

  ``````js
  import {defineAsyncComponent} from 'vue'
  let child = defineAsyncComponent(()=>import('./components/child'))
  ...
  components:{child}
  ``````

  时，未加载出来的组件会影响已加载组件的使用

  这时用`suspense`标签包裹异步组件，可以在未加载出来时显示其他内容

  ```html
  <Suspense>
  	<!--template是必写的-->
      <template v-slot:default>
      	<child/>
      </template>
      <template v-slot:fallback>
      	<h3>加载中。。。</h3>
      </template>
  </Suspense>

## Vue脚手架创建：

![img](https://upload-images.jianshu.io/upload_images/6322775-a3fc6b3b9e436def.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 尚硅谷课程方向

![img](https://upload-images.jianshu.io/upload_images/6322775-45d8605da3191c36.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**打包**vue文件**生成**可读**html文件**使用·**npm run build**(与npm run serve截然相反)·