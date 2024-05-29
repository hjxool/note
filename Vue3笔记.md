## 变化

- vue3中data、methods配置项，可以声明在`setup(){}`函数中
  - setup函数有两个默认参数——==props==、==context==
    - ==props==用来接收从父级传入的参数，同以前的props配置项

    - ==context==有三个主要的属性
      - ==attrs==：用来兜底，当传入的参数没有用props声明接收，就可以在==attrs==中找到

      - ==**emit**==：触发==父级绑定到子==组件的事件，使用方法同vue2中的**$emit**

      - ==**slot**==：接收父级传入的插槽内容，**注！**vue3中插槽必须用`<template v-slot:name/>`的形式，name才能接收到插槽内容
- 父级给子组件绑定的==自定义**事件**==，==子组件==可以用一个全新的配置项`emits:[ 'event' ]`来声明接收
- vue2中[全局方法]()的==Vue.component/directive/...==，现在使用实例对象==vm.directive/...==就可以实现
- vue2中==安装全局事件总线==的`Vue.prototype.xxx`变为了`vm.config.globalProperties`
- vue3中==data选项==必须写为==函数形式==

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

## 简介

- ==单文件组件==(`.vue`)是Vue特色，JS、HTML、CSS都封装在一个文件，结构如

  ```vue
  <script setup>
      import { ref } from 'vue'
      const count = ref(0)
  </script>
  
  <template>
    <button @click="count++">Count is: {{ count }}</button>
  </template>
  
  <style scoped>
      button {
        font-weight: bold;
      }
  </style>
  ```

- API风格

  - 选项式

    - Vue2的配置项形式，如`data`、`methods`、`mounted`等
    - 选项式本质是基于组合式实现的
    - 更易上手
    - setup作为一个**配置项**，在生命周期中执行顺序在`beforeCreate`**之前**
  
    ```vue
    <script>
    export default {
      data() {
        return {
          count: 0
        }
      },
      methods: {
        increment() {
          this.count++
        }
      },
      mounted() {}
    }
    </script>
    
    <template>
      <button @click="increment">Count is: {{ count }}</button>
    </template>
  
  - 组合式
  
    - 组合式 通常会与 `<script setup>`搭配使用
    - 形式更自由、灵活，需要较深的理解
  
    ```vue
    <script setup>
    // 注: 生命周期等钩子函数都需要引入再用
    import { ref, onMounted } from 'vue'
    const count = ref(0)
    function increment() {
      count.value++
    }
    onMounted(() => {
      console.log(`The initial count is ${count.value}.`)
    })
    </script>
    
    <template>
      <button @click="increment">Count is: {{ count }}</button>
    </template>

## 创建根实例

- 入口页面`index.html`

  - 作为容器的app元素**不会**视为应用的一部分，即不会在应用逻辑中被获取到

  ```html
  <body>
      <div id="app"></div>
      <script type="module" src="/src/main.js"></script>
  </body>

- 入口文件`main.js`

  - `mount`可接收==实际DOM元素==或==CSS选择器==作为参数

  ```js
  import { createApp } from 'vue'
  // 引入 App.vue 根组件
  import App from './App.vue'
  // 传入根组件创建应用
  const app = createApp(App)
  // 将应用绑定到 入口文件 index.html 页面元素上
  app.mount('#app')
  
  // 应用实例app 身上有 .config 对象 可以配置应用级选项
  // 例: 定义应用级错误处理器 捕获所有子组件上的错误
  app.config.errorHandler = (err) => {
      /*处理异常*/
  }
  
  // 全局注册组件
  import MyComponent from './path/MyComponent.vue'
  app.component('MyComponent', MyComponent)
  
  // 如果是多应用实例 则分开传入根组件 绑定 不同 的dom元素
  const app1 = createApp({
    /* ... */
  })
  app1.mount('#container-1')
  const app2 = createApp({
    /* ... */
  })
  app2.mount('#container-2')
  ```

- 根组件`App.vue`

  ```vue
  <template>
  	<!-- 登陆页面等入口组件 -->
      <Login />
  </template>
  <script setup>
      import {ref, provide, onMounted} from "vue";
  	import Login from "./views/其他页面/登录.vue";
      const token = localStorage.token;
      // 调用引入的生命周期函数
      onMounted(()=>{
          // 往 根组件 下的 子组件 传值
          provide('全局token', token)
  	})
  </script>

## 基础

### 模板语法

- vue3中`<template>`下不再需要==根元素==

```vue
<template>
	<!--元素内显示文本-->
	<span>Message: {{ msg }}</span>
	<!--可以写表达式-->
	<span>Message: {{ ok ? 'YES' : 'NO' }}</span>
    <!--可以写函数-->
    <span>Message: {{ msgFn() }}</span>

	<!--插入原始HTML元素 使用v-html指令 将当前span元素的 innerHTML 替换为 html-->
	<span v-html="html"></span>

	<!--属性绑定 v-bind指令-->
	<!--注: 如果绑定值为 null 或 undefined 属性 会从元素上移除-->
    <!--注: 如果绑定值为 布尔值 或 空字符串 属性不会从元素上移除-->
	<div v-bind:id="myId"></div>
	<!--简写形式-->
	<div :id="myId"></div>
	<!--也可以绑定对象-->
	<div :style="myStyle"></div>
	<!--可以绑定函数-->
	<div :style="myStyleFn()"></div>
	<!--绑定函数 传入 动态参数-->
    <div :style="myStyleFn(myId)"></div>
	<!--动态绑定 属性-->
	<a :[attributeName]="myId"> ... </a>
    <!--可以绑定表达式-->
	<button @click="num++">{{ num }}</button>

	<!--修饰符 以.xxx 形式-->
    <!--下例等同于 onSubmit 中 event.preventDefault() -->
	<form @submit.prevent="onSubmit">...</form>
</template>
<script setup>
    import {ref} from 'vue'
	let html = ref('<span style="color: red">This should be red.</span>')
    let myId = ref('1234565')
    let myStyle = ref({
        width: '0px',
    })
    let ok = ref(true)
    function myStyleFn() {
        return {}
    }
    function msgFn(){
        return 'sss'
    }
    let attributeName = ref('hhh')
    let num = ref(1)
</script>
```

### 响应式基础

#### ref

```vue
<template>
    <!--模板中使用 ref 不需要加 .value vue会自动解包-->
	<span>Message: {{ num }}</span>
	<!--改变值-->
	<button @click="num++">{{ num }}</button>
</template>
<script>
    // 用 ref()函数 来声明响应式状态
    import { ref } from 'vue'
    // setup 是一个特殊的钩子 专门用于组合式API
    setup() {
        let num = ref(1)
        // ref() 接收参数，并将其包裹在一个带有 .value 属性的 ref 对象中返回
        console.log(num) // {value: 1}
        num.value++
        console.log(num) // {value: 2}
        // 将 ref变量 暴露给模板
		return {
          num
        }
    }
</script>
```

- `<script setup>`作用

  - 不用手动暴露变量、方法

  ```vue
  <template>
  	<span>Message: {{ num }}</span>
  	<button @click="num++">{{ num }}</button>
  </template>
  <script setup>
      import { ref } from 'vue'
      let num = ref(1)
  </script>
  ```

- `ref`变量在传递给函数时，依然可以保留响应式

- `ref()`函数可以传入任意类型的值

  ```vue
  <script setup>
  	import { ref } from 'vue'
      const obj = ref({
        nested: { count: 0 },
        arr: ['foo', 'bar']
      })
      function mutateDeeply() {
        // 以下都会按照期望工作
        obj.value.nested.count++
        obj.value.arr.push('baz')
      }
  </script>

#### DOM更新时机

- 当修改了响应式变量时，DOM元素显示内容也会自动更新

  - **但是**更新DOM是==异步==的，会在函数执行完，再更新到页面
  - 要等到更新到页面后再执行额外代码，可以使用`nextTick()`方法

  ```vue
  <template>
  	<div class="item" v-for="item in list">{{item}}</div>
  </template>
  <script setup>
  	import { nextTick, onMounted } from 'vue'
      let list = []
      // 例 在数组更新到页面生成列表后 再获取列表元素
      onMounted(async ()=>{
          list = [1,2,3,4]
          // 回调函数写法
          nextTick(()=>{
              console.log(document.querySelector('.item')[1].innerHTML) // 1
          })
          // Vue3中 nextTick 变成了 Promise
          // await nextTick() 后数据就已经更新到页面上了
          await nextTick()
          // Vue2中回调方法的内容可以写在这后面
          console.log(document.querySelector('.item')[1].innerHTML) // 1
      })
  </script>

#### reactive

- 声明响应式状态

  ```vue
  <template>
  	<!--在html中使用与ref无异-->
  	<button @click="state.count++">
        {{ state.count }}
      </button>
  </template>
  <script setup>
  	import { reactive } from 'vue'
      // reactive示例
      const state = reactive({ count: 0 })
  </script>
  ```

- 与`ref`的不同

  - `ref`

    - `ref()`返回的是带有`.value`属性的`ref`对象，`.value`里才是`proxy`的对象
    - 可接收==任意类型==值

  - `reactive`

    - `reactive()`返回的是`proxy`对象，不需要`.value`

    - 只能接收==对象类型(对象、Map、Set等)==作为参数，不能对基本类型数据进行处理

    - 不能替换对象

      ```js
      let state = reactive({ count: 0 })
      // 传入新对象后 { count: 0 } 引用将失去响应式
      state = reactive({ count: 1 })
      ```

    - 不能进行解构操作，会丢失响应性

      - 这是由`proxy`数据代理特性所致，`proxy`只能代理对象，是对对象本身及子对象属性代理，修改代理对象身上的属性可以监听到

      ```js
      const state = reactive({ count: 0 })
      // 当解构时，count 已经与 state.count 断开连接
      let { count } = state
      // 不会影响原始的 state
      count++
      // 该函数接收到的是一个普通的数字 并且无法追踪 state.count 的变化
      fn(count)
      // 必须传入整个对象以保持响应性
      fn(state.count)

- `ref`细节

  - `ref`对象作为`reactive`对象的属性被访问和修改时，会自动解包

  ```js
  const num = ref(0)
  const state = reactive({
    // 将ref赋值给reactive不需要加.value
    count: num
  })
  console.log(state.count) // 0
  // 修改reactive属性值
  state.count = 1
  // ref值也变了
  console.log(num.value) // 1
  ```

  - `ref`作为数组或集合类型中的元素被访问时，**不会**自动解包，需要加`.value`

  ```js
  const arr = reactive([ref('aaa'),ref('bbb')])
  // 这里需要 .value
  console.log(arr[0].value) // 'aaa'
  
  const map = reactive(new Map([['count', ref(0)]]))
  // 这里需要 .value
  console.log(map.get('count').value)
  ```

  - 只有最外层` ref` 属性才会被解包

  ```vue
  <template>
  	<!--仅作为 文本插值 不需要加.value-->
  	<div>{{ obj.num }}</div>
      <!--如果是表达式 需要加.value-->
  	<div>{{ obj.num.value + 1 }}</div>
  	<!--解构赋值为最外层属性后 会自动.value-->
  	<div>{{ num + 1 }}</div>
  </template>
  <script setup>
      import { ref } from 'vue'
  	let obj = {num: ref(1)}
      // 用解构赋值将其变为最外层属性
      let { num } = obj
  </script>

### 计算属性

- 简化嵌套结构

  - 返回值是一个`ref`对象
  - 计算属性依赖于源数据，源数据改变，计算属性会跟着变

  ```vue
  <template>
      <span>{{ author.books.length > 0 ? 'Yes' : 'No' }}</span>
  	<!--简化后-->
  	<span>{{ msg }}</span>
  </template>
  <script setup>
      import { reactive, computed } from 'vue'
      // 例
  	const author = reactive({
        name: 'John Doe',
        books: [
          'Vue 2 - Advanced Guide',
          'Vue 3 - Basic Guide',
          'Vue 4 - The Mystery'
        ]
      })
      
      // 使用计算属性简化
      const msg = computed(() => {
        // author.books改变时msg会跟着变
        return author.books.length > 0 ? 'Yes' : 'No'
      })
  </script>

#### 计算属性和方法的区别

- 计算属性有缓存

  - 只会在依赖的数据更新时才重新计算，不会重复执行

  ```vue
  <template>
  	<!--可以得到和计算属性同样的结果-->
  	<span>{{ fn() }}</span>
      <span>{{ msg }}</span>
  </template>
  <script setup>
      import { reactive, computed } from 'vue'
  	const author = reactive({
        name: 'John Doe',
        books: [
          'Vue 2 - Advanced Guide',
          'Vue 3 - Basic Guide',
          'Vue 4 - The Mystery'
        ]
      })
      const msg = computed(() => {
        return author.books.length > 0 ? 'Yes' : 'No'
      })
      function fn() {
          return author.books.length > 0 ? 'Yes' : 'No'
      }
  </script>

#### 修改计算属性

- 计算属性默认是==只读==的

  - 因为默认形式只有`getter`回调方法
  - 通过设置`setter`使其==可修改==
  - 计算属性是派生值，**不要**在`getter`中做==异步操作或修改DOM==
  - 计算属性的派生特性，可以将其看作==临时快照==，每当源数据发生变化，会生成新的快照，尽量避免修改`快照`

  ```js
  import { ref, computed } from 'vue'
  const firstName = ref('John')
  const lastName = ref('Doe')
  
  const fullName = computed({
    // getter
    get() {
      return firstName.value + ' ' + lastName.value
    },
    // setter 对fullName进行修改时触发
    set(newValue, oldValue) {
      // newValue是要修改的值 oldValue是修改前的值
      // 将修改后的值 分割成数组 取第一个元素修改firstName 第二个元素修改lastName
      [firstName.value, lastName.value] = newValue.split(' ')
    }
  })
  ```

### class与style绑定

#### class绑定对象

- 可以给`:class`传递对象

  ```vue
  <template>
      <div :class="{ active: isActive }"></div>
      <!--渲染结果-->
      <div class="active"></div>
  
      <!--可以跟一般的 class 共存-->
      <div
        class="static"
        :class="{ active: isActive, 'text-danger': isActive }"
      ></div>
      <!--渲染结果-->
      <div class="static active text-danger"></div>
  
  	<!--可以直接绑定对象-->
  	<div :class="classObject"></div>
      <!--渲染结果-->
      <div class="static active"></div>
  </template>
  <script setup>
      import {ref, reactive} from 'vue'
  	const isActive = ref(true)
      const classObject = reactive({
        active: true,
        'text-danger': false
      })
  </script>

#### class绑定数组

- 给`:class`绑定数组来渲染多个`css`

  ```vue
  <template>
      <div :class="[activeClass, errorClass]"></div>
      <!--渲染结果-->
      <div class="active text-danger"></div>
  
  	<!--条件渲染-->
  	<div :class="[isActive ? activeClass : '', errorClass]"></div>
  	<!--用嵌套对象简化-->
  	<div :class="[{ activeClass: isActive }, errorClass]"></div>
  	<!--渲染结果-->
      <div class="active text-danger"></div>
  </template>
  <script setup>
      import {ref} from 'vue'
      const activeClass = ref('active')
      const errorClass = ref('text-danger')
      const isActive = ref(true)
  </script>

#### 组件绑定class样式

- 通常，组件样式会根据外层和模板层组合

  ```html
  <!-- 子组件模板 -->
  <p class="foo bar">Hi!</p>
  <!-- 在使用组件时 -->
  <MyComponent class="baz boo" />
  <!-- 渲染结果 -->
  <p class="foo bar baz boo">Hi!</p>
  ```

- `:class`也是同样

  ```html
  <!-- 子组件模板 -->
  <p class="foo bar">Hi!</p>
  <!-- 在使用组件时 -->
  <MyComponent :class="{ active: isActive }" />
  <!-- 渲染结果 -->
  <p class="foo bar active">Hi!</p>
  ```

- 如果是有**多个**==根元素==，且需要**指定**哪个根元素来接收这个class，通过组件的 `$attrs` 属性来指定接收的元素

  ```html
  <!-- 子组件模板 -->
  <!-- MyComponent 使用 $attrs 时 -->
  <p :class="$attrs.class">Hi!</p>
  <span>This is a child component</span>
  <!-- 在使用组件时 -->
  <MyComponent class="baz" />
  <!-- 渲染结果 -->
  <p class="baz">Hi!</p>
  <span>This is a child component</span>

#### style绑定对象

```vue
<template>
    <div :style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>
	<!--或-->
	<div :style="styleObject"></div>
    <!--渲染结果-->
    <div style="color: red;font-size: 30px;"></div>

	<!--不同于Vue2 Vue3支持 font-size 这样的写法-->
	<div :style="{ 'font-size': fontSize + 'px' }"></div>
</template>
<script setup>
    import {ref, reactive} from 'vue'
    const activeColor = ref('red')
    const fontSize = ref(30)
    const styleObject = reactive({
      color: 'red',
      fontSize: '30px'
    })
</script>
```

#### style绑定数组

- 数组元素是==样式对象==，渲染时会将这些样式对象合并

  ```vue
  <template>
      <div :style="[baseStyle, otherStyle]"></div>
      <!--渲染结果-->
  	<div style="font-size: 16px;background: red;"></div>
  </template>
  <script setup>
      import {reactive} from 'vue'
      const baseStyle = reactive({
          fontSize: '16px'0
      })
      const otherStyle = reactive({
          background: 'red'
      })
  </script>

#### style多值

- 可以对同一样式属性提供多个值，浏览器仅会渲染支持的最后一个值(兼容性写法)

  ```html
  <div :style="{ display: ['-webkit-box', '-ms-flexbox', 'flex'] }"></div>

### 条件渲染

#### v-if

- `v-if`如果条件为假，从一开始就不会渲染到页面，在dom节点找不到

- `v-else`

  - `v-else`元素必须紧跟在`v-if`或者`v-else-if`元素的后面，**否则**它将==不会被识别==

  ```html
  <h1 v-if="awesome">Vue is awesome!</h1>
  <h1 v-else>Oh no 😢</h1>
  
  <!--v-else-if 类似else if 可连续多次使用-->
  <div v-if="type === 'A'">
    A
  </div>
  <div v-else-if="type === 'B'">
    B
  </div>
  <div v-else-if="type === 'C'">
    C
  </div>
  <div v-else>
    Not A/B/C
  </div>
  ```

- `<template>`上的`v-if`

  - `template`搭配`v-if`，可以实现==包裹一串标签==的同时，不改变标签层次结构，因为`<template>`不会被渲染出来，但可以用`v-if`来条件渲染
  - 注：不能配合`v-show`，会将`template`包裹的全部节点显示出来

  ```html
  <div class="box">
      <template v-if="isShow">
  		<div>aaa</div>
          <div>bbb</div>
      </template>
  </div>
  <!--渲染结果-->
  <div class="box">
      <div>aaa</div>
      <div>bbb</div>
  </div>
  ```

- **不要**在`v-if`渲染时**立即**去获取节点，使用`this.$nextTick()`，在加载完毕时再获取

#### v-show

- 无论是否显示，在DOM渲染中都会保留该元素，`v-show`仅切换元素的`display`样式
- `v-show` 不支持在 `<template>` 元素上使用，也不能和 `v-else` 搭配使用

#### v-if和v-show

- 涉及到**节点计算**时，**一定**要用`v-if`，因为`v-show`会将节点放入页面**但隐藏**，**隐藏**的节点其宽高等属性为==0或者null==，究其原因是因为`mounted`等生命周期函数只执行一次
- `v-if`是==真实的==按照条件渲染，因为要**确保**条件区块内的事件监听、子组件等都会被==销毁和重建==
- `v-show`要简单得多，无论初始条件，始终会渲染，只有`display`样式属性会切换
- 所以`v-if` 有更高的切换开销，而 `v-show` 有更高的初始渲染开销。因此，如果需要频繁切换，则使用 `v-show` 较好；如果在运行时绑定条件很少改变，则 `v-if` 会更合适

#### v-if和v-for

- **不推荐**同时使用 `v-if` 和 `v-for`，当 `v-if` 和 `v-for` 同时存在于一个元素上的时候，`v-if` 会==首先被执行==

### 列表渲染

#### v-for遍历数组

- `v-for` 指令基于一个数组、对象、字符串、数字来渲染一个列表，需要使用 `item in items` **形式**的特殊语法

  - 其中 `items` 是源数据的数组，而 `item` 是迭代项的**别名**

  ```vue
  <template>
  	<li v-for="value in arr">
        {{ item.message }}
      </li>
  </template>
  <script setup>
  	const arr = ref([{ message: 'Foo' }, { message: 'Bar' }])
  </script>

- `v-for` 块中可以完整地访问父作用域内的属性和变量

  - `v-for` 也支持使用可选的第二个参数表示当前项的位置索引

  ```vue
  <template>
  	<li v-for="value,index in arr">
        {{ parentMessage }} - {{ index }} - {{ item.message }}
      </li>
  	<!--渲染结果-->
  	<li>Parent - 0 - Foo</li>
      <li>Parent - 1 - Bar</li>
  </template>
  <script setup>
  	const parentMessage = ref('Parent')
      const arr = ref([{ message: 'Foo' }, { message: 'Bar' }])
  </script>
  ```

- `v-for`可以使用解构赋值

  ```html
  <li v-for="{ message } in items">
    {{ message }}
  </li>
  
  <!-- 有 index 索引时 -->
  <li v-for="({ message }, index) in items">
    {{ message }} {{ index }}
  </li>
  ```

- 对于多层嵌套的`v-for`可以访问到==父级作用域==

  ```html
  <li v-for="item in items">
    <span v-for="childItem in item.children">
      {{ item.message }} {{ childItem }}
    </span>
  </li>
  ```

- 可以使用`of`这种更接近JS中`for`语句的语法

  ```html
  <div v-for="item of items"></div>
  ```

- 对于数组中单个或多个，而非全部更新的高性能模式，应当给每一个遍历元素上绑定唯一值`key`

  - 组件因其自身特性，使用`v-for`遍历时，**必需要**绑定`key`

  ```html
  <div v-for="item in items" :key="item.id"></div>
  ```

- 使用`push`等方法能够触发更新

  - 当不用这些方法时，需要将==旧数组替换为新数组==才能触发更新

#### v-for遍历对象

- **第一个**参数表示==属性值==

  ```vue
  <template>
  	<ul>
        <li v-for="value in myObject">
          {{ value }}
        </li>
      </ul>
  </template>
  <script setup>
  	const myObject = reactive({
        title: 'How to do lists in Vue',
        author: 'Jane Doe',
        publishedAt: '2016-04-10'
      })
  </script>
  ```

- 也可以通过**第二个**参数表示==属性名==

  ```html
  <li v-for="(value, key) in myObject">
    {{ key }}: {{ value }}
  </li>
  ```

- 而**第三个**参数才表示==位置索引==

  ```html
  <li v-for="(value, key, index) in myObject">
    {{ index }}. {{ key }}: {{ value }}
  </li>
  ```

#### v-for遍历整数

- 取值范围是`1...n`

  ```html
  <!--渲染结果 1 2 3 4-->
  <span v-for="n in 4">{{ n }}</span>
  ```

#### template与v-for

- 可以在 `<template>` 标签上使用 `v-for` 渲染多个元素块

  ```html
  <ul>
    <template v-for="item in items">
      <li>{{ item.msg }}</li>
      <li class="divider" role="presentation"></li>
    </template>
  </ul>
  ```

#### v-if与v-for

- `v-if` 比 `v-for` 的优先级更高。意味着 `v-if` 无法访问到 `v-for` 内的变量

  ```html
  <!--
   这会抛出一个错误，因为属性 todo 此时
   没有在该实例上定义
  -->
  <li v-for="todo in todos" v-if="!todo.isComplete">
    {{ todo.name }}
  </li>
  <!--解决方案 v-if和v-for放到不同层级-->
  <template v-for="todo in todos">
    <li v-if="!todo.isComplete">
      {{ todo.name }}
    </li>
  </template>
  ```

### 事件
