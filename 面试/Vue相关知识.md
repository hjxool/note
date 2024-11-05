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
      // ...
      // 获取默认插槽
      let slot = this.$slots.default
      // 获取第一个子组件
      let vnode = getFirstComponentChild(slot)
      // 组件参数
      let componentOptions = vnode && vnode.componentOptions
      // 判断是否有组件参数
      if(componentOptions) {
          // 获取组件名
          let name = getComponentName(componentOptions)
          let {include, exclude} = this
          if((include && (!name || !matches(include, name))) || (exclude && name && matches(exclude, name))) {
              // include不匹配 或 exclude匹配 则直接返回组件实例
              return vnode
          }
          let {cache, keys} = this
          // 获取组件对应的key 有则读取 无则生成新的
          let key = vnode.key == null ? componentOptions.Ctor.cid + (componentOptions.tag ? `::${componentOptions.tag}` : '') : vnode.key
          if(cache[key]) {
              // 有缓存 则渲染缓存的实例
              // 注 组件初次渲染的时候componentInstance为undefined
              vnode.componentInstance = cache[key].componentInstance
              // LRU缓存策略 即最近操作过的放数组末尾
              remove(keys, key) // 从原来的位置移除
              keys.push(key) // 再添加到末尾
          } else {
              // 没有缓存 则先存起来
              cache[key] = vnode
              keys.push(key) // 放在数组末尾
              if(this.max && keys.length > this.max) {
                  // 新添加组件缓存后 如果超出max设置的数量上限
                  // 则把缓存中旧的记录删除 即数组头部的为最长时间不用的
                  pruneCacheEntry(cache, keys[0], keys, this._vnode)
              }
          }
          // 将组件的keepAlive属性设置为true
          // keepAlive判断是否要执行组件的created、mounted
          vnode.data.keepAlive = true
      }
      // 最后将配置了缓存的vnode返回
      return vnode || (slot && slot[0])
  }
  ```

- 首次渲染

  - 判断组件的`abstract`属性，才往父组件里面挂载DOM

  ```js
  function initLifecycle(vm) {
      // 找到父级节点 将当前节点挂载
      let parent = vm.$options.parent
      // 判断组件的abstract属性
      if(parent && !vm.$options.abstract) {
          // 父级是抽象组件 且 还有父级 就继续向上找
          while(parent.$options.abstract && parent.$parent) {
              parent = parent.$parent
          }
          parent.$children.push(vm)
      }
      // 添加vm属性
      vm.$parent = parent // 指定父节点
      vm.$root = parent ? parent.$root : vm // 指定根节点
      vm.$children = []
      vm.$refs = {}
      vm._watcher = null
      // ...
  }
  ```

  - 判断当前`keepAlive`和`componentInstance`是否存在，来决定使用先前的组件实例还是创建新的组件实例

- LRU缓存策略

  - 记录数据，比如用数组，依据核心思想"**如果数据最近被访问过，那么将来被访问的几率也更高**"，将最近访问过的数据放在数组末尾，最久没被调用过的数据则放在数组首位，如果数组中有数据被访问，则将其从数组中移除再添加至数组末尾来更新数据最近访问记录
    - 例：链表的实现思路
      - 新数据插入到链表头部
        - 因为链表都是从头开始检索，放在头部是最容易快速访问的
      - 每当缓存数据被访问，将数据移动到链表头部
      - 链表满的时候，将链表尾部丢弃

## $nextTick 原理及作用

- 原理
  - 本质是利用JS**事件循环**机制，用如`Promise`、`setTimeout`等原生方法来模拟微/宏任务的实现，等主线程更新完DOM树后，异步回调执行时对更新后的DOM进行处理

- 作用
  - 数据变化后的操作，需要使用随数据变化而变化的DOM结构的时候
  - Vue生命周期中，如果在`created()`钩子进行DOM操作
    - 因为在`created()`时，页面DOM还未渲染，这时候没法操作DOM，所以，此时如果想要操作DOM，必须将操作的代码放在`nextTick()`的回调函数中

## Vue2中如何封装数组方法

- 简单来说，通过==重写数组原生方法==，在数组方法调用时，获取对象身上的`__ob__`，如果有新的值，则递归设置数据代理，并手动调用`__ob__`的`dep`通知`watcher`执行模板渲染

  ```js
  // Object.create不同于构造函数
  // 在创建 对象 的同时 还会设置其继承目标对象
  // 即原型链arrayMethods.__proto__ === Array.prototype
  export const arrayMethods = Object.create(Array.prototype)
  // 要重写的方法
  const methods = [
    "push",
    "pop",
    "shift",
    "unshift",
    "splice",
    "sort",
    "reverse"
  ]
  for(let name of methods) {
      // 用闭包缓存 原生数组方法
      let 原方法 = Array.prototype[name]
      def(methods, name, function (...args) {
          // 注意 def是在其他地方定义的方法
          // 传入的回调函数并不是在此处执行
          // 而是将传入的回调函数使用bind等方式改变this指向
          // 所以此处this指向为 调用该函数的数组对象
          let result = 原方法.apply(this, args) // 执行原生方法得到结果
          let 插入参数
          switch (name) {
              case "push":
              case "unshift":
                  插入参数 = args
                  break
              case "splice":
                  // splice(index, n, args) 所以从传入参数中的第三个开始获取
                  插入参数 = args.slice(2)
                  break
          }
          if(插入参数) {
              // 类似Object.defineProperty拦截数据变化时
              // 当触发set 要深度递归对新值进行代理一样
              // 此处数组中有了新插入的值 同样递归进行代理
              this.__ob__.observeArray(插入参数)
          }
          // 数组发生了变化 手动调用 通知watcher更新
          this.__ob__.dep.notify()
          // 返回原生方法结果
          return result
      })
  }
  ```

## Vue template到render过程

1. 执行`compileToFunctions`将`template`转化为AST
   - AST是一种用JS对象的形式描述模板
   - 解析过程：利用正则表达式顺序解析`template`中字符串内容，当解析到==开始标签、闭合标签、文本==的时候都会分别执行对应的==回调函数==，从而构造AST树
     - AST元素节点分为三种类型
       - `type==1`：普通元素
       - `type==2`：表达式
       - `type==3`：纯文本
2. 调用`optimize(ast,options)`深度遍历AST
   - 对静态节点打标记，后续渲染时，==直接跳过静态节点==，从而优化渲染速度
3. `generate(ast, options)`生成代码
   - `generate`将AST编译成，形如`<div>{{...}}`的字符串，并将**静态部分**放到`staticRenderFns`中，最后通过`new Function('...')`生成`render`函数

## Vue data中属性值发生变化后视图会同步渲染吗

- 不会同步渲染，而是侦听到数据变化，然后开启一个队列，缓冲在==同一事件循环中==发生的所有数据变更，其中同一个`watcher`多次触发会被**去重**

## Vue自定义指令

- 全局定义：`Vue.directive("xxx",{})`

- 局部定义：`directives:{focus:{}}`

- 生命周期

  - `bind`：只调用一次，指令第一次绑定到元素时调用
    - 可以进行一次性的初始化设置
  - `inSerted`：被绑定元素==插入父节点==时调用
    - 仅保证父节点存在，但不一定已被插入文档中

  - `update`：组件的`VNode`更新时调用
    - 只要`VNode`变化就会调用，指令的值可能没有改变，==可以通过比较触发前后的值来忽略不必要的模板更新==
  - `ComponentUpdate`：指令所在组件的`VNode`及其子`VNode`全部更新后调用
  - `unbind`：只调用一次，指令与元素==解绑时调用==

- 使用场景
  - 需要操作DOM时
    - 如自定义指令实现==图片懒加载==

## 子组件可以改变父组件数据吗

- 不可以，这是为了维护父子组件的单向数据流，即父级更新通过`props`流向子组件，反过来则不行，这是为了防止意外改变父组件状态，使得数据混乱，变得难以理解，子组件可以通过`$emit`触发父组件事件，由父组件自行修改自己状态

## React 和 Vue 之间的异同

- 相同点

  - 都使用虚拟DOM提高**重绘**性能
  - 都有`props`的概念，允许组件间传值
  - 提倡==组件化==，将应用拆成功能模块，提高复用性

- 不同点

  - Vue支持数据==双向绑定==，而React提倡==单向==数据流

  - 使用虚拟DOM方面
    - Vue会跟踪每个组件的依赖关系，不需要重新渲染整个组件树
    - React则是当属性值被改变时，全部子组件都会重新渲染
      - React可以通过`shouldComponentUpdate`生命周期方法进行优化
  - 组件书写方式(最大不同点)
    - Vue
      - 使用常规HTML模板
      - 数据都挂载在`this`上，所以`import`组件后，还需要在`components:{}`属性中再==声明后使用==
    - React
      - 用JS的语法扩展——JSX书写
      - React中`render`函数支持闭包特性，所以`import`引入的组件在`render`中可以==直接调用==
  - 数据监听原理不同
    - Vue通过数据劫持，能精确知道数据变化，且具有良好性能
    - React通过==比较引用==的方式，不进行`shouldComponentUpdate`优化，可能会导致大量不必要的DOM重新渲染
  - 高阶组件不同
    - React支持==用JSX直接写函数式组件==，并用Hook拓展函数组件
    - Vue使用HTML模板创建组件，并需要==额外的转换操作==才能生成`render`函数(见`template到render过程`)

## assets和static的区别

- 相同点
  - 都用来存放==静态资源==文件。如图片、样式等
- 不同点
  - `assets`中存放的静态资源文件在项目打包时，会进行压缩
  - `static`中的静态资源会原样上传到服务端
- 建议
  - 将==样式文件、js文件==等放在`assets`中，进行代码压缩和文件压缩
  - 而第三方资源如`iconfoont.css`等文件可以放置在`static`中，因为文件已经处理过，可直接上传

## Vue性能优化

- 编码阶段

  - 尽量减少data中数据

    - data中数据都会增加`getter`和`setter`，以及收集对应的`watcher`

  - `v-if`和`v-for`不能连用

  - 减少监听事件。用事件代理取代`v-for`给每项元素绑定事件

    - 就是在`v-for`外层容器添加**单个**事件监听，通过`event.target`获得点击目标信息

    ```vue
    <template>
        <ul  @click="fn">
          <li v-for="(item, index) in items" :data-index="index">
            {{ item }}
          </li>
        </ul>
    </template>
    <script>
    export default {
      data() {
        return {
          items: ['Item 1', 'Item 2', 'Item 3']
        };
      },
      methods: {
        fn(event) {
          if (event.target.tagName === 'LI') {
            let index = event.target.dataset.index;
            console.log('Clicked index:', index);
            console.log('Clicked value:', this.items[index]);
          }
        }
      }
    };
    </script>
    ```

  - SPA页面采用`<keep-alive>`缓存组件

  - 大多数情况下，用`v-if`而非`v-show`

  - `v-for`绑定`key`。避免重复渲染

  - 路由懒加载

    - 定义：组件在==需要时加载==，而非==启动时一次性加载==
    - 即在用户访问到对应组件内容时，使用`import()`动态加载

    ```js
    let routes = [
        {
            path: '/home',
            component: () => import('./views/Home.vue')
        }
    ]
    import Router from 'vue-router'
    export default new Router({routes})
    ```

  - 异步组件

    - 定义：同**路由懒加载**都是==按需加载==，比路由懒加载==更灵活==，因为在==组件内定义异步逻辑==，而不仅限于路由
    - 同样用`import()`动态加载
    - `.js`中使用

    ```js
    Vue.component('xxx', {
        // 需要加载的组件
        component: import('./components/xxx.vue'),
        // 加载时使用的组件
        loading: LoadingComponent,
        // 加载失败组件
        error: ErrorComponent,
        // 延迟加载时间
        delay: 200,
        // 超时时间
        timeout: 3000
    })
    ```

    - `.vue`中使用`defineAsyncComponent`

    ```vue
    <template>
    	<async-component></async-component>
    </template>
    
    <script>
    import { defineAsyncComponent } from 'vue';
    
    export default {
      components: {
        AsyncComponent: defineAsyncComponent(() => import('./xxx.vue'))
      }
    };
    </script>
    ```

  - 防抖、节流
  - `import()`按需导入第三方模块
  - 图片懒加载、长列表滚动到==可视区域==动态加载

- SEO优化

  - 预渲染

    - 定义：由服务端返回==静态HTML文件==
    - 适用于内容不常变化的页面，如博客文章
    - 常用工具
      - `Next.js`：React发布的框架，支持预渲染和SSR
      - `Nuxt.js`：Vue发布的框架，提供静态站点生成功能

  - 服务端渲染SSR

    - 定义：根据**请求**==动态生成HTML内容==
    - 适用于内容变化的页面，如用户仪表盘、社交媒体应用

    - 缺点：只支持`beforeCreate`和`created`两个钩子

- 打包优化

  - 同Webpack优化方式优化Vite

- 用户体验

  - 骨架屏

  - PWA

# 生命周期

## Vue生命周期

1. `beforeCreate`：事件监听、`watcher`、数据代理都还没开始，无法访问`data`、`computed`、`watch`、`methods`数据
2. `created`：实例创建完成。可以访问`data`、`computed`、`watch`、`methods`数据。**但**因为未挂载到页面，不能访问`$el`
3. `beforeMount`：挂载开始之前。已完成模板编译，根据`data`数据和`template`生成VNode
4. `mounted`：根节点被VNode替换，并挂载到实例上
5. `beforeUpdate`：响应式数据更新时调用。此时虽然数据更新了，但真实DOM还没有被渲染
6. `updated`：VNode重新渲染后调用。此时DOM已更新，可以执行DOM操作。但是应避免在此时更改状态，会导致更新无限循环
7. `beforeDestroy`：实例销毁之前调用。此时实例仍可用，一般在这时清理监听事件和定时任务
8. `destroyed`：实例销毁后调用。`v-bind`、`v-on`事件监听会被移除，但是`$bus.$on`等事件不会自动移除

## 子组件和父组件执行顺序

- 渲染过程
  1. 父组件`beforeCreate`
  2. 父组件`created`
  3. 父组件`beforeMount`
  4. 子组件`beforeCreate`
  5. 子组件`created`
  6. 子组件`beforeMount`
  7. 子组件`mounted`
  8. 父组件`mounted`
- 更新过程
  1. 父组件`beforeUpdate`
  2. 子组件`beforeUpdate`
  3. 子组件`updated`
  4. 父组件`updated`
- 销毁过程
  1. 父组件`beforeDestroy`
  2. 子组件`beforeDestroy`
  3. 子组件`destroyed`
  4. 父组件`destoryed`

## 一般在哪个生命周期请求数据

- 可以在`created`、`beforeMount`、`mounted`中进行请求，因为`data`已创建，可以进行数据赋值

## keep-alive 中的生命周期

- `keep-alive`是 Vue 内置组件，以实例对象形式保存在内存中，防止重复渲染DOM
- 用`keep-alive`包裹的组件会多出两个生命周期：`deactivated`、`activated`。同时`beforeDestroy`和`destroyed`不会再被触发，因为组件不会被真正销毁
- 初次熏染时
  - `... -> mounted -> activated`
- 组件被换掉时
  - `-> deactivated`
- 再被切回来时
  - `-> activated`

# 组件通信

## props/$emit

- `props`
  - 只能父向子传值，单向下行绑定，==子组件数据随父组件更新==
  - 可接收各种数据类型，如**函数**
  - 命名规则：若`props`中使用驼峰形式，则模板中需使用短横线形式

- `$emit`
  - 父组件在子组件上绑定事件监听，子组件通过`$emit`触发，并可以传递参数给父组件

## 事件总线

- 适用于==任意组件间通信==

  1. 创建事件总线

     - 注意，Vue3开始==组合式API==中没有`onMounted`之前的钩子函数，因此不适合像Vue2中在`beforeCreate`中添加==全局Vue实例==到`$bus`

     ```js
     // event-bus.js
     import Vue from 'vue'
     export const EventBus = new Vue()
     ```

  2. 监听事件，接收参数

     ```vue
     <template>
       <div>求和: {{count}}</div>
     </template>
     <script setup>
     // 组件中 按需引入 全局vue实例对象
     import { EventBus } from './event-bus.js'
     import {ref, onMounted} from 'vue'
     let count = ref(0)
     onMounted(()=>{
         EventBus.$on('addition', param => {
           count.value = count.value + param.num;
         })
     })
     </script>
     ```

  3. 发送事件

     ```vue
     <template>
       <div>
         <button @click="add">加法</button>    
       </div>
     </template>
     <script>
     import {EventBus} from './event-bus.js'
     import {ref} from 'vue'
     let num = ref(0)
     function add() {
         EventBus.$emit('addition', {
             num: num.value++
         })
     }
     </script>
     ```

- 注意，用这种方式通信，`EventBus.$on`绑定的监听事件并不会随组件销毁而消失，因为存于全局Vue实例中，因此需要在组件销毁钩子函数中将事件注销`EventBus.$off('eventName')`，且全局事件总线会==导致后期维护困难==

  - 注：`$off`可以传入第二个参数，如`EventBus.$off('eventName', this.处理事件方法)`，表示==移除监听同一事件的具体事件处理函数==，从而精确控制哪些处理函数被移除，因为同一事件可能有多个组件监听，只清除被卸载组件的处理方法。
  - 如果只传入事件名称`EventBus.$off('eventName')`，可能会误删其他监听器

## 依赖注入provide/inject

- 用于**父子组件之间的通信**，在==组件嵌套层数很深的情况下==，可以用这种方法进行传值，就不用一层一层的传递了

- `provide / inject`是Vue提供的两个**钩子函数**，和`data`、`methods`同级

  - 父组件

  ```js
  // 选项式
  provide() { 
      return {     
          num: this.num  
      }
  }
  // 组合式API
  import {provide, ref} from 'vue'
  let num = ref(0)
  provide('num', num.value)
  ```

  - 子组件

  ```js
  // 选项式
  inject: ['num']
  // 组合式API
  import {inject} from 'vue'
  let num2 = inject('num')
  ```

- 响应式数据注入，建议响应数据的变化保持在组件自身内

  - 父组件

  ```js
  import { provide, ref } from 'vue'
  let num = ref(0)
  function fn(n) {
    num.value = n
  }
  provide('add', {
      fn, // 传入函数而非响应式数据值
      num
  })
  ```

  - 子组件

  ```vue
  <template>
    <button @click="fn(2)">{{ num }}</button>
  </template>
  <script setup>
  import { inject } from 'vue'
  let { fn, num } = inject('add')
  </script>
  ```

## $parent / $children

- `$parent`让组件访问==上一级==父组件的实例

- `$children`让组件访问子组件的实例
  - 但是==并不能保证顺序==，且访问的==数据也不是响应式==的

# 路由

## Router懒加载如何实现

- 非懒加载

  ```js
  import List from '@/components/list.vue'
  let router = new Router({
    routes: [
      { path: '/list', component: List }
    ]
  })
  ```

- 懒加载

  - 方式一：`import`动态加载

  ```js
  let List = () => import('@/components/list.vue')
  let router = new Router({
      routes: [
          { path: '/list', component: List }
      ]
  })
  ```

  - 方式二：`require`动态加载

  ```js
  // 注意 与import 写法 传参 区别
  let List = resolve => require(['@/components/list.vue'], resolve)
  let router = new Router({
      routes: [
          { path: '/list', component: List }
      ]
  })
  ```

## 路由的hash和history区别

- `hash`模式

  - **简介**：路由的==默认模式==，由`#`号后的值构成，如`www.abc.com/#/vue`，`hash`值为`#/vue`
  - **特点**：`hash`值虽然在URL里，但不会出现在HTTP请求中。所以改变`hash`值，==不会重新加载页面==

  - **原理**：主要通过`onhashchange()`事件实现

    - 优点：页面hash值变化时，无需向后端发起请求，前端就可以监听事件的改变，按规则加载相应的代码
    - 注意：`hash`值变化对应的URL会被浏览器记录下来，浏览器能实现页面的前进和后退

    ```js
    window.onhashchange = function(event) {
        let hash = location.hash.slice(1) // #号后的部分
    }
    ```

- `history`模式

  - **简介**：`history`模式的URL没有`#`号，使用传统的路由分发模式，即用户输入一个URL时，服务器会接收这个请求，并解析这个URL
  - **特点**：没有`#`号，看起来比`hash`模式URL好看一点。但是`history`模式需要后台配置支持，如果后台没有正确配置，访问时会返回404
    - 缺点：刷新页面时，如果没有相应的路由或资源，会404

  - **API**

    - 修改历史状态：`pushState()`、`replaceState()`，用这两个API修改url，浏览器不会立即向后端发送请求

    - 切换历史状态：`forward()`、`back()`、`go()`，对应浏览器的前进、后退、跳转

  - 要启用`history`模式，进行如下配置

    ```js
    let router = new Router({
        mode: 'history',
        routes: [...]
    })
    ```


## 如何获取页面hash变化

- 监听`$route`的变化

  ```js
  // 路由发生变化的时候执行
  watch: {
      $route: {
          handler: function(val, oldVal) {},
          deep: true, // 深度监听
      },
      // 简写
      $route(val, oldVal) {}
  }
  ```

- `window.location.hash`读取`#`值
  - `window.location.hash`可读可写，写入时可以在不重载网页的前提下，添加一条历史访问记录

## $route 和 $router 区别

- `$route`是“==路由信息对象==”，包括`path`，`params`，`hash`，`query`等路由信息属性

- `$router`是“路由==实例对象==”，包括路由的跳转方法，钩子函数等

## 动态路由 及 动态参数的获取

- `param`方式

  - 路由定义

  ```js
  // APP.vue中
  // 方式1 路径后拼接动态参数
  <router-link :to="'/user/' + userId">用户</router-link>
  // 方式2
  <router-link :to="{name: 'user', params: {id: ...}}">用户</router-link>
  
  // index.js中
  {
      // 用:加名称的方式 按顺序 接收参数
      path: '/user/:id',
      component: User, // 表示只要是/user/:id的形式都是跳转User组件
  }
  ```

  - 路由跳转

  ```js
  // 方式1
  <router-link :to="..."></router-link>
  // 方式2
  this.$router.push({name: 'user', params: {id: '123'}})
  // 方式3
  this.$router.push('/user/' + '123')
  ```

  - 参数获取
    - 通过`$route.params.id`获取传递的值

- `query`方式

  - 路由定义与跳转

  ```js
  // 方式1
  <router-link :to="{name: 'user', query: {id: '...'}}"></router-link>
  // 方式2
  <router-link :to="{path: '/user', query: {id: '...'}}"></router-link>
  // 方式3
  <button @click="fn"></button>
  fn() {
      this.$router.push({
          path: '/user',
          query: {id: 'id'}
      })
  }
  ```

  - 参数获取
    - 通过`$route.query.id`获取传递的值

- `name`和`path`跳转区别

  - 例多层级路由配置

  ```js
  let routes = [
      {
          path: '/user',
          name: 'user',
          conponent: User,
          children: [
              {
                  path: 'profile',
                  name: 'user-profile',
                  component: Profile
              }
          ]
      }
  ]
  ```

  - `name`属性表示多层级路径

  ```vue
  <router-link :to="{name: 'user-profile'}"></router-link>
  ```

  - `path`表示多层级路径

  ```vue
  <router-link :to="{path: '/user/profile'}"></router-link>
  ```

- `params`与`query`区别
  - `query`==刷新不会丢失==`query`里面的数据
  - `params`==刷新会丢失== `params`里面的数据

## 路由守卫和钩子函数

### 路由守卫

- 作用：植入导航过程的操作逻辑。如登录权限验证，进行路由跳转前先验证是否登录，否则跳转登录页

### ==全局==路由守卫

```js
// 前置守卫 跳转前
router.beforeEach((to, from, next) => {
    // 判断登录信息是否存在
    let info = Vue.prototype.自定义属性和方法('userData')
    if(info) {
        // 有用户信息 允许跳转
        next()
    } else {
        if(to.path == '/login') {
            // 跳转的是登录页 允许跳转
            next()
        } else {
            // 其他 跳转到登录
            alert('请重新登录')
            window.location.href = '跳转路径'
        }
    }
})
// 后置守卫 跳转后
router.afterEach((to, from) => {
    // 例 跳转后滚动条回到顶部
    // scrollTo(x, y)窗口滚动到指定位置
    window.scrollTo(0, 0)
})
```

### 路由==单独配置==守卫

```js
export default [
    {
        path: '/',
        name: 'login',
        component: Login,
        // 单独配置路由守卫
        beforeEnter: (to, from, next) => {
            next()
        }
    }
]
```

### 组件内的钩子函数

- 注意与单独配置的路由守卫的区别，它是与生命周期函数同级的配置参数

```js
export default {
    name: 'Login',
    // 进入组件前触发
    beforeRouteEnter(to, from, next) {
        // 注意 beforeRouteEnter时 组件实例还没创建因此无法访问this
        next(vm => {
            // 需要在next进行下一步时才能访问到
            vm.fn()
        })
    },
    // 组件更新时触发
    beforeRouteUpdate(to, from, next) {},
    // 离开组件调用
    beforeRouteLeave(to, from, next) {}
}
```

### 生命周期等钩子函数触发顺序

- 假设从A组件离开，进入B组件

1. `beforeRouteLeave`：离开路由前触发，==可取消离开路由==
2. `beforeEach`：触发全局前置守卫，可用于登录验证、全局路由loading等
3. `beforeEnter`：触发路由独享守卫
4. `beforeRouteEnter`：进入组件前的钩子函数
5. `beforeResolve`：全局解析守卫
6. `afterEach`：此时已经进入组件创建流程，触发全局后置守卫

- 此时才正式开始创建组件实例

7. `beforeCreate`：B组件生命周期
8. `created`
9. `beforeMount`

- 注意！B组件走到`beforeMount`，此时才执行组件A离开生命周期

10. `deactivated`：离开组件A，或者触发组件A的`beforeDestroy`和`destroyed`
11. `mounted`：组件B挂载
12. `activated`：缓存组件B

- 注意！此时`beforeRouteEnter`的回调函数才开始执行

13. 执行`beforeRouteEnter`的`next(callback)`回调函数

## 路由跳转和location.href区别

- `location.href`跳转简单，但会刷新页面
- `history.pushState`==不刷新页面==，静态跳转
- Vue-router跳转，使用`diff`算法，减少了dom消耗
  - `hash`模式，使用`#/xxx`形式，不会引起页面加载
  - `history`模式，使用`history.pushState`

# Vuex

## Vuex原理

- 定义
  - Vuex是专为Vue开发的状态管理模式。核心就是`store`(仓库)。`store`可以视作一个**容器**，包含应用中大部分的`state`(状态)
- 特点
  - Vuex的状态存储是响应式的。若`store`中的状态发生变化，依赖对应状态的组件也会更新
  - 改变`store`中状态的唯一途径就是通过`mutation`，这是为了便于跟踪状态变化，==类似于数据劫持==

## action与mutation区别

- `mutation`

  - 执行一系列同步操作，用于修改state中变量值

  - 定义一个`mutations`

    ```js
    let store = new Vuex.Store({
        state: {
            num: 1
        },
        mutations: {
            // 当触发该mutations方法时改变count属性值
            add(state, args) {
                state.num += args.n
            }
        }
    })
    ```

  - `commit`触发`mutation`方法

    ```js
    methods: {
        fn() {
            this.$store.commit('add', {n: 2})
        }
    }
    ```

- `action`

  - 可以包含==异步==操作，但==不能直接操作State==

  - `action`提交的是`mutation`，而不是像`mutation`直接修改值

    ```js
    let store = new Vuex.Store({
        state: {
            num: 1
        },
        mutations: {
            add(state, args) {
                state.num += args.n
            }
        },
        actions: {
            add2(context) {
                // context是state父级
                setTimeout(() => {
                    context.commit('add')
                })
            }
        }
    })
    ```

  - `dispatch`触发`action`方法

    ```js
    methods: {
        fn() {
            this.$store.dispatch('add2', {n: 2})
        }
    }
    ```

## Redux 与 Vuex 区别

- 区别
  - Vuex改进了Redux中的`Action`和`Reducer`函数，以`mutations`变化函数取代`Reducer`，无需`switch`，只需在对应的`mutation`函数里改变`state`值即可
  - Vuex由于Vue自动重新渲染的特性，无需重新订阅渲染函数，只要生成新的`State`即可
- 相同点
  - ==单—==数据源
  - `state`数据变化拦截

## 为什么要用Vuex 或 Redux

- 对于**多层嵌套**的组件传参会非常繁琐，**兄弟组件**间的状态传递要么繁琐，要么后期维护困难
- 所以使用Vuex或Redux，把组件的共享属性抽出来，以一个全局单例模式管理。这种情况下，不管在哪个位置，任何组件都能获取共享属性或者触发行为修改共享属性

## Vuex有哪些属性

- `state`： 存放基本数据
- `getters`： 由基本数据派生出的数据，可以理解为计算属性
- `mutations`： 更改数据的方法，==同步==执行
- `actions`： 包裹`mutations`，可以==异步==触发`mutations`中的方法
- `modules`： 模块化Vuex，类似Vue`data`中声明不同对象管理不同组的数据

## Vuex 与 全局变量 的区别

- Vuex存储的状态是响应式的，状态改变，引用Vuex状态的组件内容也会更新。而全局变量不会触发Vue响应式更新模板
- Vuex中`store`不能直接被改变，只能通过`this.$store.commit()`触发`mutation`从而修改`state`，这样可以拦截修改`state`的操作

## 为什么mutation不能异步

- 因为要拦截`state`修改操作，就不能异步，因为异步的一大缺点就是不知何时执行

## 严格模式有什么用 如何开启

- 严格模式下，任何不是由`mutation`函数引起的变更，都会抛出错误

- 开启严格模式

  ```js
  const store = new Vuex.Store({
      strict: true
  })
  ```

## 如何在组件中批量使用Vuex的getter属性

- 使用Vuex的`mapGetters`函数、Vue的`mixins`混入配置项，以及扩展运算符

  ```js
  // xxx.js
  // 在组件中引入vuex方法
  import {mapGetters} from 'vuex'
  export default {
      computed: {
          value1() {
              return this.value2 + this.value3
          },
          // 使用扩展运算符将mapGetters结果展开 作为computed配置项
          ...mapGetters(['xxx1', 'xxx2', ...])
      }
  }
  ```

## 组件中如何简写提交mutation

- 使用Vuex的`mapMutations`函数将`mutation`方法重命名，作为`methods`配置项

  ```js
  // xxx.js
  // 组件中引入vuex方法
  import {mapMutations} from 'vuex'
  methods: {
      fn1() {...},
      ...mapMutations({
          fn2: 'add_num'
      })
  }
  // 组件中调用时
  this.fn2(10)
  // 等同于
  this.$store.commit('add_num', 10)
  ```

# Vue3

## Vue3有什么更新

- 监测机制的改变
  - Vue3使==用Proxy实现数据劫持==，Vue2用`Object.defineProperty`只能监测属性，不能监测对象
  
  - Proxy数据劫持，可以拦截到属性的**添加**和**删除**
  - Proxy可以拦截到数组**长度**和**索引位置值**的变更
  - Proxy支持`Map`、`Set`、`WeakMap`和`WeakSet`
  
- 作用域插槽

  - Vue2中作用域插槽改变，父组件会**重新渲染**
    - Vue2使用`slot-scope="slotProps"`形式，当`slotProps`里的数据发生改变，父组件就要重新渲染`<child-component><template slot-scope="slotProps">`这部分模板
  
  ```Vue
  <!-- 父组件 -->
  <template>
    <div>
      <child-component>
        <template slot="xxx" slot-scope="slotProps">
          <p>{{ slotProps.text }}</p>
        </template>
      </child-component>
    </div>
  </template>
  <!-- 子组件 -->
  <template>
    <div>
      <slot name="xxx" :text="message"></slot>
    </div>
  </template>
  <script>
  export default {
    data() {
      return {
        message: 'Hello from child component'
      };
    }
  };
  </script>
  ```
  
  - Vue3把作用域插槽改成了==函数形式==，只会使子组件重新渲染，提升了渲染性能
    - Vue3用`v-slot="{ text }"`这种形式，`{text}`部分会被解析成**函数**，这样当`{text}`中数据变化，只用重新调用`() => ({text})`函数，而不会引起父组件重新渲染
  
  ```vue
  <!-- 父组件 -->
  <template>
    <div>
      <ChildComponent v-slot:xxx="{ text }">
        <p>{{ text }}</p>
      </ChildComponent>
    </div>
  </template>
  <!-- 子组件 -->
  <template>
    <div>
      <slot name="xxx" :text="message"></slot>
    </div>
  </template>
  
  <script>
  import { defineComponent, ref } from 'vue';
  export default defineComponent({
    setup() {
      const message = ref('Hello from child component');
      return {
        message
      };
    }
  });
  </script>
  ```
  
- `render`函数书写格式改变

  - Vue2

    ```js
    render: function(createElement) {
        return createElement('div', [
            createElement('span', 'hello'),
            createElement('button', {
                on: {
                    click: this.fn
                }
            }, 'click me')
        ])
    }
    ```

  - Vue3

    ```js
    import {h} from 'vue'
    render: function() {
        return h('div', [
            h('span', 'hello'),
            h('button', {
                onClick: this.fn
            }, 'click me')
        ])
    }
    ```

- 对象式的组件声明方式

  - Vue2是配置项风格，用一系列属性完成组件定义
    - 极易上手，但不够灵活优雅
    - 过于依赖`this`，比如`methods`中`this`指向组件实例，而非`methods`所在对象，使得 TypeScript 在Vue2 中不好用，根本原因是TS特性是==静态类型检查==，而`this`过于灵活，所以对TS不是很友好

  
  ```js
  export default {
    data() {
      return {
        message: 'Hello, Vue 2!'
      };
    },
    methods: {
      greet() {
        // 这句在TS可能会报错 因为无法正确推断 this 的类型
        console.log(this.message);
      }
    },
    created() {
      this.greet();
    }
  };
  ```

  - Vue3==写法形似类==
    - Vue3引入==组合式API==，通过在`setup`函数，像普通变量一样声明使用，从而更自然地使用TS
  
  
  ```js
  import { defineComponent, ref } from 'vue';
  export default defineComponent({
    setup() {
      const message = ref('Hello, Vue 3!');
      const greet = () => {
        console.log(message.value);
      };
      greet();
      return {
        message,
        greet
      };
    }
  });
  ```

## 组合式API和React的Hook很像，区别是什么

- React Hook

  - 从Hook的实现来看，Hook是根据`useState`的调用顺序来确定下一次重渲染时的`state`是源自哪个`useState`
  - 因此==不能在循环、条件、嵌套函数中调用Hook==
  - 必须确保是==在React函数的顶层调用Hook==
  - `useEffect`、`useMemo`等函数必须手动确定依赖关系

- 组合式API

  - 由于是基于Vue响应式，==组合式API不用顾虑调用顺序，可以在循环、条件、嵌套函数中使用==
  - 一次组件实例化==只调用一次setup函数==，而React Hook==每次重渲染都要调用Hook==

  - Vue响应式自动完成了依赖收集，而React Hook需要手动传入依赖，因此对于Hook依赖的顺序很重要

# 虚拟DOM

## 对虚拟DOM的理解

- 本质上来说，虚拟DOM是一个JS对象，通过对象的方式表示DOM结构，是对DOM的抽象。多次虚拟DOM的修改结果一次性更新到页面上，有效减少了重绘重排次数

- Vue在每次数据==变化前，都会**缓存**一份虚拟DOM==。数据==变化后，当前的虚拟DOM会与缓存的虚拟DOM进行比较==。Vue内部封装了==diff算法==，通过这个算法进行比较，找到需要改变的部分进行重新渲染

## 虚拟DOM的解析过程

1. 初始渲染
   - Vue将==模板编译成虚拟DOM树==，即JS对象，其中包含`TagName`、`props`和`Children`这些属性，称为==旧虚拟DOM==
   - 根据旧虚拟DOM，Vue生成真实DOM，插入到文档中
   - Vue初次编译时会声明一个渲染函数`render()`，每次调用都会生成新的虚拟DOM树
2. 页面数据变化
   - 当响应式数据发生变化时，会触发`watcher`回调函数重新渲染模板，触发`watcher`的同时会重新执行`render`函数生成新虚拟DOM
     - 注意！在Vue中`watcher`回调函数不直接更新真实DOM，而是更新虚拟DOM中的模板，因此重新执行`render`函数是有必要的，根据新旧DOM树的对比，收集所有改动，一起进行更新
3. 虚拟DOM比较
   - Vue用**Diff算法**对比新虚拟DOM和旧虚拟DOM
   - 这种比较过程是高效的，因为虚拟DOM是内存中的对象，比操作真实DOM快得多
4. 更新DOM
   - 找出差异后，Vue只对差异处进行更新(例如添加、删除或修改元素)，而不是重新解析和渲染整个DOM树

## 虚拟DOM真的比真实DOM性能好吗

- 首次渲染渲染大量DOM时，由于多了一层虚拟DOM的计算，会比innerHTML插入慢
- 但在后续DOM操作时更快

## Diff算法原理

- 进行新老虚拟DOM对比时
  1. 对比节点本身(`===`)，判断是否为同一节点，如果不是同一节点，则删除该节点重新创建节点进行替换
  2. 如果是同一节点，则进行对比找不同的地方
     1. `patchVnode`：先判断一方有子节点，一方没有子节点的情况。如：新的`children`没有子节点，则最终渲染结果会将旧的子节点移除
     2. `updateChildren`：都有子节点的情况，判断如何最小程度更新节点(diff核心)
  3. ==递归==比较子节点

- ==diff算法只比较同层的子节点==，不进行跨级节点的比较，也就是说，只有当新旧`children`都为多个子节点时，才用Diff算法进行同层级比较

## Vue中key的作用

- 分两种情况会用到`key`：
  - `v-if`中使用`key`
    - 由于Vue会尽可能高效地渲染元素，通常会复用已有元素而不是从头开始渲染。因此当使用`v-if`来实现元素切换时，如果切换前后含有相同类型的元素，那么这个元素就会被复用
    - 如相同的`input`元素，不用`key`，切换前后`input`内容都不会被清除掉，这是不符合需求的。因此通过使用`key`来唯一标识一个元素，该元素不会被复用
    - 此时`key`的作用是==标识一个独立的元素==
  - `v-for`中使用`key`
    - `v-for`更新渲染过的列表时，默认使用“就地复用”的策略。如数据项的顺序发生改变，Vue**不会**移动DOM元素来匹配数据项的顺序，而是简单==复用同一位置的==元素
    - 通过为列表中每一项提供一个`key`值，使Vue跟踪元素的身份，从而复用对应`key`值的元素
    - 此时`key`的作用是==帮助高效更新渲染虚拟DOM==，通过`key`，diff算法可以更准确、快速
      - 更准确：用`a.key === b.key`简单的就可以进行节点对比
      - 更快速： 利用`key`的**唯一性**生成`map`对象来获取对应节点，比遍历的方式更快

## 为什么不建议用索引作为key

- 用索引作为`key`和没写没区别，Vue默认就会用索引作为`key`
- 且用索引作为`key`，不管数组的顺序怎么颠倒，索引都是 0, 1, 2...这样排列，导致Vue会复用错误的旧子节点，做很多额外的工作