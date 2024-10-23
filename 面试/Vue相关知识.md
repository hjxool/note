# Vue基础

## 基本原理

- Vue实例创建时，Vue会遍历`data`中的属性，用`Object.defineProperty`(vue3.0使用`proxy`)将它们转为`getter/setter`，并且在内部追踪相关依赖，在属性被访问和修改时通知变化。每个组件实例都有对应的`watcher`实例，它会在组件渲染的过程中把属性记录为依赖，之后当依赖项的`setter`被调用时，会通知`watcher`重新计算，从而使与它关联的组件得以更新

## 双向绑定原理

- 采用**数据劫持**结合**发布者-订阅者模式**的方式，通过数据劫持监听属性的`getter/setter`，在数据变动时发布消息给订阅者，触发相应的监听回调

- 步骤
  1. `observe`对`data`进行递归遍历，包括子属性对象的属性，都加上`setter`和`getter`，当对属性赋值，就会触发`setter`，就能监听到数据变化
  2. `compile`**解析**模板指令，==将模板中的变量替换成数据==，然后初始化==渲染页面视图==，并将每个==指令对应的节点绑定更新函数==，**添加**监听数据的订阅者，一旦数据有变动，收到通知，更新视图
  3. `Watcher`订阅者是`Observer`和`Compile`之间通信的桥梁，主要做的事情是:
     1. 自身==实例化时==往属性订阅器(`dep`)里面添加自己
     2. 自身必须有一个`update()`方法
     3. 待属性变动`dep.notice()`通知时，能调用自身的`update()`方法，并触发`Compile`中绑定的回调
  4. MVVM作为数据绑定的入口，整合`Observer`、`Compile`和`Watcher`三者，通过`Observer`来监听自己的`model`数据变化，通过`Compile`来==解析编译模板指令==，最终利用`Watcher`搭起`Observer`和`Compile`之间的通信桥梁，达到`数据变化 -> 视图`更新；`视图 -> 数据变更`的双向绑定效果

## Vue2数据劫持有什么缺点

- Vue2使用`Object.defineProperty()`来进行数据劫持，在对一些属性进行操作时，这种方法无法拦截，如：
  - 下标方式修改数组
  - 数组的大部分操作都拦截不到，只是 Vue 内部通过==重写函数==的方式解决了这个问题
  - 给对象新增属性
- Vue3.0 中已经不使用这种方式，而是通过`Proxy`进行数据劫持，`Proxy`的可以完美监听到任何方式的数据改变

##  MVVM、MVC、MVP的区别

- MVC、MVP 和 MVVM 是三种常见的软件架构设计模式，主要通过分离关注点的方式来组织代码结构
- MVC
  - 分为`Model`(JS逻辑层)、`View`(视图层)和`Controller`，MVC应用了观察者模式，当 Model 层发生改变的时候它会通知有关 View 层更新页面。Controller 层是 View 层和 Model 层的纽带，即事件触发器，负责用户与页面交互的操作，通过对 Model 的修改，然后 Model 层再去通知 View 层更新
- MVVM
  - 分为 Model、View、ViewModel
    - Model 即数据和业务逻辑
    - View 即视图，负责数据的展示
    - ViewModel 负责监听 Model 中数据的改变并且控制视图的更新，处理用户交互操作
  - Model 和 View 并无直接关联，而是通过 ViewModel 进行联系，Model 和 ViewModel 之间有着双向数据绑定的联系
- MVP
  - MVP 模式与 MVC 唯一不同的在于 Presenter 和 Controller
    - MVC 中使用观察者模式，实现 Model 层数据发生变化时，通知 View 层更新。这使得 View 层和 Model 层耦合在一起，当项目逻辑变得复杂的时候，可能会造成代码混乱以及复用性问题
    - MVP 通过 Presenter 实现对 View 层和 Model 层的解耦。MVC 中的Controller 只知道 Model 的接口，因此没办法控制 View 层的更新。MVP 模式中，View 层的接口暴露给了 Presenter 因此可以在 Presenter 中将 Model 的变化和 View 的变化绑定在一起，以此来实现 View 和 Model 的同步更新

## Computed 和 Watch 的区别

- `Computed`
  - 有缓存，依赖数据发生变化时才会虫重新计算
  - 不支持异步，当`computed`的中有异步操作时，无法监听数据变化

- `watch`
  - 更多的是**观察**作用，**无缓存**，类似于某些数据的监听回调，每当监听的数据变化时都会执行回调进行后续操作

- 适用场景
  - 当需要进行数值计算，并依赖其他数据时，应该用`computed`利用它的缓存特性，避免每次都要重复计算
  - 当需要在数据变化时==执行异步或开销较大==的操作时，应该使用`watch`
    - 如监听输入框，设一个防抖，当停止输入一段时间后调用查询API

## Computed 和 Methods 的区别

- `computed`
  - 基于依赖进行缓存，相关依赖发生改变才会重新求值

- `method`
  - 调用总会执行该函数

## slot 原理和作用

- `slot`插槽，是子组件标签元素，而这一标签元素是否显示，显示什么，由==父组件决定==
  - 匿名(默认)插槽
    - 一个组件内只有有一个匿名插槽
  - 具名插槽
    - 带name属性的插槽，一个组件可以出现多个具名插槽
  - 作用域插槽
    - 匿名插槽、具名插槽的一个变体，可以是匿名插槽，也可以是具名插槽
    - 该插槽的不同点在于子组件渲染作用域插槽时，可以将子组件内部数据传递给父组件，让父组件根据子组件的传递过来的数据决定如何渲染该插槽

- 原理

  - 子组件实例化时，获取到父组件传入的slot标签的内容，存放在`vm.$slot`中，如匿名插槽为`vm.$slot.default`，具名插槽为`vm.$slot.xxx`

  - 当组件执行==渲染函数==时候，遇到slot标签，使用`$slot`中的内容进行替换，**此时**可以为插槽传递数据，若存在数据，则可称该插槽为==作用域插槽==

## 如何保存页面的当前状态

- 组件会被卸载
  - 将状态存在`LocalStorage / SessionStorage`
    - 在组件即将被销毁的生命周期中用`JSON.stringify()`将组件状态存在`LocalStorage / SessionStorage`中
  - 路由传值
    - 通过组件的`props`
  - 使用`<keep-alive>`
    - 注意组件==第一次加载==时，会在`mounted`后调用`activated`缓存组件，==之后再次进入组件，只会触发==`activated`
    - 使用了`deactivated`就不会调用`beforeDestroy`和`destroyed`
- 组件不会被卸载
  - 单页面渲染
    - 要切换的组件作为子组件**全屏**渲染，父组件中页面状态不会变

## 常见事件修饰符

- `.stop`
  - 阻止事件==冒泡==
  - 等同于 JavaScript 中的`event.stopPropagation()`

- `.prevent`
  - ==阻止默认行为==
  - 等同于 JavaScript 中的`event.preventDefault()`
- `.capture`
  - 与事件冒泡相反，事件捕获由外到内
- `.self`
  - 只会触发自己的事件，==不含子元素==

- `.once`
  - 只触发一次

## v-if、v-show、v-html原理

- `v-if`会调用`addIfCondition`方法，生成`vnode`的时候会忽略对应节点，`render`的时候就不会渲染
  - 编译过程
    - `v-if`切换有一个局部编译/卸载的过程，切换时会销毁和重新加载内部的事件监听和子组件，这也是为什么`v-if`可以刷新组件内容
- `v-show`会生成`vnode`，`render`的时候会渲染成真实节点，只是在渲染过程中会修改节点上的`display`属性

- `v-html`会先移除节点下的所有节点，调用`addProp`方法设置`innerHTML`值

## v-model如何实现

- 用在表单元素上

  ```html
  <input v-model="message" />
  <!-- 等同于 -->
  <input 
      :value="message" 
      @input="message=$event.target.value"
  >
  ```

- 用在自定义组件上

  - 会为组件添加`props:['value']`和绑定`@input`事件

  ```html
  <child 
         :value="message"
         @input="onmessage"
  ></child>
  
  methods:{
      onmessage(e){
          $emit('input',e.target.value)
      }
  }
  ```

## keep-alive如何实现，具体缓存的是什么

- keep-alive的三个属性
  - `include="字符串/正则"`：只有名称匹配的组件会被缓存
  - `exclude="字符串/正则"`：名称匹配的组件不会被缓存
  - `max="数字"`：最多可以缓存多少组件实例

- 流程
  1. 判断组件`name`，不在`include`或者在`exclude`中，直接返回`vnode`，说明该组件不被缓存
  2. 获取组件实例`key`，如果有，使用已有`key`，否则重新生成
  3. `key`生成规则，`cid +"∶∶"+ tag`，仅靠`cid`是不够的，因为相同的构造函数可以注册为不同的本地组件
  4. 如果缓存对象内存在，则直接从缓存对象中获取组件实例给`vnode`，不存在则添加到缓存对象中
  5. 最大缓存数量，当缓存组件数量超过`max`值时，清除`keys`数组内第一个组件

- 实现

  ```js
  // 接收：字符串，正则，数组
  let patternTypes: Array<Function> = [String, RegExp, Array]
  
  export default {
    name: 'keep-alive',
    abstract: true, // 抽象组件,它自身不会渲染成DOM元素，也不会出现在父组件链中
  
    props: {
      include: patternTypes, // 缓存
      exclude: patternTypes, // 不缓存
      max: [String, Number], // 缓存组件的最大实例数量, 由于缓存的是组件实例(vnode)，数量过多的时候，会占用过多的内存
    },
  
    created() {
      // 用于初始化缓存虚拟DOM数组和vnode的key
      this.cache = Object.create(null)
      this.keys = []
    },
  
    destroyed() {
      // 销毁缓存cache的组件实例
      for (const key in this.cache) {
        pruneCacheEntry(this.cache, key, this.keys)
      }
    },
  
    mounted() {
      // 监控include和exclude的改变，根据最新的include和exclude的内容，来实时削减缓存的组件的内容
      this.$watch('include', (val) => {
        pruneCache(this, (name) => matches(val, name))
      })
      this.$watch('exclude', (val) => {
        pruneCache(this, (name) => !matches(val, name))
      })
    },
  }
  ```

- `render`函数

  ```js
  function render() {
      // 获取默认插槽
      let slot = this.$slots.default
      // 获取第一个子组件
      let vnode = getFirstComponentChild(slot)
  }
  ```

  