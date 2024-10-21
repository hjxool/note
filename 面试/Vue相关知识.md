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