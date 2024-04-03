## 变化

- vue3中`<template>`下不再需要==根元素==

- vue3中data、methods配置项，可以声明在`setup(){}`函数中
  - `setup(){}`必须有==返回值==，可以写成两种形式，==对象形式==、==渲染函数==
    - 对象形式：`return { name，function }`。对象形式中包含的属性和方法均可直接使用，比如`{{name}}`、`@click="function"`
    - 渲染函数：`return () => {return h('元素', 内容)}`，想要使用还需要`import {h} from 'vue'`
  
  - setup函数有两个默认参数——==props==、==context==
    - ==props==用来接收从父级传入的参数，同以前的props配置项
  
    - ==context==有三个主要的属性
      - ==attrs==：用来兜底，当传入的参数没有用props声明接收，就可以在==attrs==中找到
  
      - ==**emit**==：触发==父级绑定到子==组件的事件，使用方法同vue2中的**$emit**
  
      - ==**slot**==：接收父级传入的插槽内容，**注！**vue3中插槽必须用`<template v-slot:name/>`的形式，name才能接收到插槽内容
  
  - setup作为一个**配置项**，那生命周期等**平级**配置项中如何调用setup中的数据呢？答案是——用==**this**==，因为==setup==执行顺序在==beforeCreate==**之前**，因此，在==setup内部==**this还没有指向vue实例对象**，而是用JS原生作用域的原理取值。但除了==setup==，其他配置项this都指向vue实例。注：经过实验，==setup==中的==this==指向**window**对象，**生命周期钩子**中this指向==vm实例对象==，且==拥有setup==中暴露出来的**变量**和**函数**，直接用`this.xx`即可调用

- vue3中==自己声明==的属性变量不再具有==响应性==，而是要通过一个==ref函数==去加工成==RefImpl对象==，即引用实例对象
  - 不推荐用`ref()`处理==非基本类型==数据。ref在处理==对象类型数据==时，只会把对象本身用数据劫持绑定get和set方法，其下的属性都是用==ES6中新语法proxy==处理，因此在取对象第一层数据时需要`obj.value.xxx`，其后的层级数据直接取用
  - ==对象类型数据==用`reactive`函数处理
  - `ref`使用方法：
    - 使用前必须要`import { ref } from 'vue'`，引入ref组件
    - `let xxx = ref( '张三' )`，使用ref函数处理赋值的变量
    - ==修改==时使用`xxx.value = '李四'`，此处vue3为value属性做了个数据劫持。**※但是**，页面引用时不需要用`.value`，vue自动进行了一个`.value`取值操作

- 父级给子组件绑定的==自定义**事件**==，==子组件==可以用一个全新的配置项`emits:[ 'event' ]`来声明接收

- vue2中[全局方法]()的==Vue.component/directive/...==，现在使用实例对象==vm.directive/...==就可以实现

- vue2中==安装全局事件总线==的`Vue.prototype.xxx`变为了`vm.config.globalProperties`

- vue3中==data选项==必须写为==函数形式==

- 在单文件组件(SFC)中，可以使用`<script setup>`简化，无需使用`setup(){ return {...} }`省去了手动暴露

  ```vue
  <script setup>
  	import {ref} from 'vue'
      let n = ref(123)
      fn(){...}
  </script>

## 组件

![](https://upload-images.jianshu.io/upload_images/6322775-5708bc097d7f0416.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

<center>要用对象的形式，对象名就是组件标签名称</center>

- 创建vue实例对象，并注册使用组件

![img](https://upload-images.jianshu.io/upload_images/6322775-71d852e4d355f256.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

<center>component 属性挂载需要的组件</center>

- 注意在==组合式API==中`components`配置项和`setup(){}`是**同级**

  ```js
  import componentA from './xxx.js'
  let vm = {
      components:{componentA},
      setup(){...}
  }
  ```

- 子组件接收父组件值

  - 直接在模板中使用即可，只是`setup()`中会接收`props`，并不是说一定要在`setup(prop)`中`return`才能使用

  ```js
  export default{
      props:{msg:''},
      setup(prop){
          //props会作为第一个参数传递给setup()
      }
  }
  ```

- 子组件触发父组件==事件==

  - 与Vue2不同的是，==父组件==在==子组件**标签**==身上监听的事件，需要在==子组件==中**声明**
  - 注意`setup()`中第二个参数使用==解构赋值==取得参数中的`emit()`方法！
  - `emit()`第一个参数是==事件名称==，第二个参数是传递给事件监听器的值，传多个值最好使用数组或对象形式包裹

  ```js
  export default{
      emits:['eventA'],
      setup(props,{ emit }){
          emit('eventA', 'value')// 触发事件
      }
  }

- 组件新写法

  - 在 `<head>` 内写 `<script type="自定义" id="组件挂载id">`

    ![img](https://upload-images.jianshu.io/upload_images/6322775-98fc500bb756ae16.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- 使用外部组件库
  1. 创建实例对象`const app = Vue.CreateApp(...)`
  2. 注册组件`app.use(xxx)`
  3. 挂载页面`app.mount('#app')`

## 创建根实例对象

- vue3方法创建的实例对象，不能直接使用，只有在 `mount()` 挂载后所赋值的变量，才能取到里面的值，等同于Vue2中的`let vm = new Vue`

  ![img](https://upload-images.jianshu.io/upload_images/6322775-67f17ff8c53c9978.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- 挂载vue实例：

  ```html
  <!-- 通过CDN使用，Vue大写 -->
  <script src=""https://unpkg.com/vue@3/dist/vue.esm-browser.js></script>
  <script>
  	let vm = {...}
  	Vue.createApp(vm).mount('所要挂载的html元素ID或者class')
      或者
      let { createApp } = Vue
  </script>
  
  <!-- 通过ES模块，因为export {...}，所以必须是路径 -->
  <script type="module">
  	import { createApp } from 'https://unpkg.com/vue@3/dist/vue.esm-browser.js'
      createApp(vm).mount('#app')
  </script>
  
  <!-- 通过Import maps，自定义名 -->
  <script type="importmap">
  	{
  	  "imports":{
  	    "hhh": "https://unpkg.com/vue@3/dist/vue.esm-browser.js"
  	  }
  	}
  </script>
  <script type="module">
  	import { createApp } from 'hhh'
      createApp(vm).mount('#app')
  </script>

## 常用修饰符

![img](https://upload-images.jianshu.io/upload_images/6322775-c175112bac13a75b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- `keydown.13`等数字形式的修饰符被废弃

## vue2组件传值进化

- vue2.x中的`provide/inject`在vue3中进化了，更增加了**响应性**

![img](https://upload-images.jianshu.io/upload_images/6322775-b14144c00b89de88.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

<center>依靠跟“计算属性”组合</center>

![img](https://upload-images.jianshu.io/upload_images/6322775-0bc7392462742f06.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

<center>“响应式”下，inject的使用稍有不同，注意红框中的内容，传递过来的值是一个对象，需要提取其中的值</center>

![img](https://upload-images.jianshu.io/upload_images/6322775-1fc48098f2331824.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

<center>可以看到打印出来的值是个对象，只有value里面才是传进来的值</center>

- ==组合式==写法

  ```js
  祖先组件：
  	import {reactive,provide} from 'vue'
  	let xxx = reactive({aaa:'asdas',bbb:'2222'})
      provide('键名'，xxx)
  后代组件：
  	import {inject} from 'vue'
  	let xxx2 = inject('键名')
      // 第二个参数是未接收到参数时的默认值
      let xxx3 = inject('键名', '默认值')
      直接调用：xxx2.aaa
  ```

## reactive

- 加工==对象类型==的数据，使用==reactive函数==，加工成==proxy对象==

- reactive**不能**处理==基础数据类型==

- 使用方法：

  ```js
  import {reactive, createApp} from 'vue'
  createApp({
      setup(){
          let obj = reactive({ xxx:'李白'，age：18 })
          return {obj}
      }
  }).mount('#app')
  ```

- 用==扩展运算符==展开`reactive`加工的对象，其属性不再具有==响应式==！


## toRef/toRefs

- 当想用==简写==形式，而不是`对象.xxx.xxx`时，通常使用`name:person.name`将值取出赋值给==新变量==，但这样就**失去**了==响应性==
- `toRef(对象, 键名)`
  - ==toRef==的作用正在于此，它将对象中的==基础数据类型==与==源对象==进行了一次==桥接==，使其进行关联`name:toRef(person,'name')`，其中的`name`不是一个**新的变量**，而是响应式的从==源对象==身上得到的
  - 只能处理单个
  - 当需要暴露一个对象中个别属性时可用
- `toRefs(对象)`(重要)
  - 返回值是==对象==`{key1:..., key2:...}`
  - 在`setup()`返回值中`return {...toRefs(对象)}`，在HTML模板中用`{{对象中第一层的基础类型数据}}`，直接取用，而不需要`object.key`这样来使用，但是里层的属性依然需要`{{ key1.key2... }}`
- `toRefs`与`reactive`区别
  - `reactive`加工的对象使用==扩展运算符==展开后不具有响应式
    - 这是由于ES6`proxy`数据代理特性所致，`proxy`只能代理对象，修改代理对象身上的属性可以监听到，但是没有对基本类型的属性监听，所以`reactive`只是对对象本身及子对象属性代理，展开后浅拷贝的基本类型属性会丢失响应式
  - `toRefs`专门批量生产响应式变量，加工后的对象展开后，属性依然具有响应式

## computed计算属性

- vue3中计算属性也变成了==组合式API==，即需要import导入后才能使用 
  - 使用方法：在==setup函数==中用==函数==形式`computed()`，该方法会返回一个响应式变量
    - 创建的计算属性是`ref`，这意味着取值需要`.value`
  - 简写形式：`reactive对象.xxx = computed(()=>{})`，简写形式只能读不能写
  - 完整写法：`reactive对象.xxx = computed({get(), set()})`，在==页面上==调用时用`{{reactive对象.xxx}}`来使用


## watch监听器

- 使用import导入==watch方法==，与computed不同的是watch是基于已有的属性，而不是创建新属性，所以不用赋值操作。
  - 使用方法：`watch(单对象/数组, callback, 配置项)`
    - 第一个参数可以是setup中单个属性，也可以是==一组==属性，比如`watch([sum, name])`，可以同时监视多个对象
    - 第二个参数是回调函数，默认有`newValue`、`oldValue`两个参数，比较特殊的是，在监听==数组==时，回调函数中的参数也变成==数组形式==，即修改前/后两个参数值变成数组
    - 第三个参数是==配置项==对象，如开启深度监听`{deep:true}`，可省略
  - vue3中的watch只能监听==ref==、==reactive==、==数组==，**不包括**reactive下的==属性值==！所以要用函数形式
    - 监听==对象==下==单个==属性：`watch(()=>{return obj.key}, callback)`。第一个参数得用==函数**返回值**形式==
    - 监听==对象==下==多个==属性：`watch([()=>obj.key, ()=>obj.key2, ...], callback)`
      - `()=>返回值`是箭头函数的简写形式，当只有==一行==时可以省略`return`和`{}`
- Tips：
  - **注！**监听==对象==时，因为对象是引用类型，虽然监听到变化，但是`newValue`、`oldValue`是对象的==**引用地址**==，因此`newValue`、`oldValue`值一样
  - 对于`reactive对象`，默认开启==深度监听==，对于==reactive对象**下的某一层级对象**==，则需要手动开启监听`watch(xxx, callback, {deep:true})`
  - watch监听的是一个==结构==，不是==值==，所以监听时不能用`watch(ref对象.value)`，只有==修改==ref对象**值**的时候才需要`ref对象.value`
    - **但**如果监听的是`let obj = ref(obj2)`的`obj`，则需要`watch(obj.value)`，因为如果是`watch(obj)`，监听的是`obj`的地址值，因为`obj`是一个`RefImpl`对象，`obj.value = obj2地址值`，相当于在`obj2`外包了一层，修改`obj`里的属性值`obj2`的地址值不会改变，因此监测不到

## 注册模板引用ref

- 在==Vue2==和==选项式API==中，`<div ref="dom">`ref引用会被注册在`this.$refs`对象里

- **而**在==组合式API==中，ref引用存储在与名字匹配的ref里

  ```vue
  <template>
  	<div ref="dom"></div>
  </template>
  
  <script>
  	import {ref} from 'vue'
  	setup(){
      	let dom = ref()
      	return {
          	dom
      	}
  	}
  </script>


## vue3新增watchEffect

- 不指定监听对象，只指定回调，当使用和依赖数据变化时会触发回调

- 使用方法：

  ```js
  watchEffect(()=>{
      let params1 = 外部数据 如 obj.name
      let params2 = 外部数据 如 obj.age
      触发时执行逻辑
  })
  ```

  - 当`params`依赖的外部数据变化时，执行==通用==逻辑，这点跟**计算属性**很像

- 与**watch**的**不同**：watch是每个监听对象，都可以有不同的回调，而watchEffect是==共用回调函数==。本质是为了方便代码复用，当一堆监听对象都有一样的执行回调方法，就可以watchEffect写到一起

## vue3新增hook函数

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

## 生命周期

- ```ts
  <script>
  import { ref } from 'vue'
  
  export default {
    setup() {
      const count = ref(0)
      // 返回值会暴露给模板和其他的选项式 API 钩子
      return {
        count
      }
    },
  
    mounted() {
      console.log(this.count) // 0
    }
  }
  </script>
  
  <template>
    <button @click="count++">{{ count }}</button>
  </template>
