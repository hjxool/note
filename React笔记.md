## 文档

- [中文网](https://react.docschina.org)
- 官方chrome浏览器插件`React Developer Tools`
- vscode插件`ES7 React/Redux/GraphQL/React-Native snippets`
  - 脚手架环境下在`xxx.jsx`文件中，使用`rcc`会自动生成组件结构片段


## 示例

- html

  ```html
  <!--准备容器-->
  <div id="test"></div>
  
  <!-- 引入react核心库 React对象 -->
  <script type="text/javascript" src="../react.development.js"></script>
  <!-- ReactDOM对象 用于支持react操作DOM -->
  <script ... src="../react-dom.development.js"></script>
  <!-- 引入babel，用于将jsx转为js -->
  <script ... src="../babel.min.js"></script>
  <!-- PropTypes对象 用于标签属性限制 -->
  <!-- html内写jsx一定要写type="text/babel" -->
  <script type="text/babel">
  	// 1.创建虚拟DOM 在jsx中不需要写引号 因为不是字符串
  	const dom = <h1>hello</h1>
  	// 2.渲染虚拟DOM到页面 render(虚拟DOM, 容器)
  	ReactDOM.render(dom, document.getElementById('test'))
  </script>
  ```

## jsx语法

- 定义虚拟DOM时，不要写引号

  ```jsx
  const dom = <h1>hello</h1>
  // 如果是多层结构 用()表示一个整体
  const dom = (
      <h1 className="title">
          <span>hello</span>
      </h1>
  )

- 标签中混入JS==表达式==时要用`{}`

  - JS语句(代码)
    - 如`if for`等，控制代码走向而没有返回值
  - JS表达式
    - 一个表达式会产生一个值，可以放在任何需要值的地方

  ```jsx
  let id = 'aAa'
  let content = '啊实打实的'
  const dom = (
  	<h2 id={id.toLowerCase()}>
          <span>{content}</span>
      </h2>
  )
  ```

- 样式的类名指定不能用`class`，要用`className`

  - 是为了避免跟ES6的`class`类冲突

  ```jsx
  const dom = (
  	<h2 className="bg">
          <span>asdasd</span>
      </h2>
  )
  ```

- 内联样式要用`{{}}`

  - 外层`{}`表示里面是JS表达式，里层`{}`表示一个对象

  ```jsx
  const dom = (
  	<h2 style={{fontSize:'20px'}}>
          <span>asdasd</span>
      </h2>
  )
  ```

- 只能有一个==根标签==

  ```jsx
  // 错误形式
  const dom = (
  	<h2>
          <span>asdasd</span>
      </h2>
      <div>xxx</div>
  )
  // 正确形式
  const dom = (
  	<div>
      	<h2>asdasd</h2>
          <input type="text"/>
      </div>
  )
  ```

- 标签必须闭合

  ```jsx
  // 错误形式
  const dom = (
  	<div>
      	<h2>asdasd</h2>
          <input type="text">
      </div>
  )
  // 正确形式
  const dom = (
  	<div>
      	<h2>asdasd</h2>
          <input type="text"/>
          // 或者
          <input type="text"></input>
      </div>
  )
  ```

- 标签首字母小写，则当作html元素

  - ==首字母大写==，则当作react组件

## 组件

- 函数式组件

  - 适用于简单组件
  - 因为没有`this`，所以无法读取`state`等属性

  ```jsx
  // 1.创建函数组件 注意 组件名首字母大写
  function Demo(){
      console.log(this) //此处this时undefined，因为babel编译后开启了严格模式
      return <h2>asdasd</h2>
  }
  // 2.渲染组件到页面 注意 标签必须闭合
  ReactDOM.render(<Demo/>, document.getElementById('test'))
  ```

  - 函数式组件只能使用`props`

  ```jsx
  // 组件标签上传入的props会作为函数组件的参数传入
  function Person(props){
      let {name, age} = props
      return (
      	<ul>
          	<li>姓名：{name}</li>
              <li>年龄：{age}</li>
          </ul>
      )
  }
  // 函数式组件类型限制只能写在外层
  Person.propTypes = {
      name: PropTypes.string.isRequired,
      age: PropTypes.number,
      sex: PropTypes.string,
      speak: PropTypes.func, // 注：函数类型是func
  }
  Person.defaultProps = {
      age: 18,
      sex: '不男不女'
  }
  ReactDOM.render(<Person name="tom" age={18}/>, document.getElementById('test'))

- 类式组件

  - 适用于复杂组件

  ```jsx
  // 1.创建类式组件
  // 必须继承react的Component类
  class Demo extends React.Component {}
  // 或
  import {Component} from 'react'
  class Demo extends Component{
      // 2.必须有render()
      render(){
          // 3.必须有返回值
          return (
              <div>{this.props.name}</div>
          )
      }
  }
  ReactDOM.render(<Demo/>, document.getElementById('test'))
  ```

  - 组件嵌套

  ```jsx
  class A extends Component{
      render() {
          return (
          	<div>
              	<div>A</div>
                  <B/>
              </div>
          )
      }
  }
  class B extends Component{
      render() {
          return (
          	<div>B</div>
          )
      }
  }


## 组件-state属性

- 类似Vue的data

  ```jsx
  class Demo extends Component{
      constructor(props){
       super(props)
       this.state = {
           isHot: true
       }
      }
      render(){
          return <div>{this.state.isHot? '炎热': '凉爽'}</div>
      }
  }
  ```

- 简写`state`

  ```jsx
  class Demo extends Component{
      // 初始固定参数 不需要构造器 直接使用赋值语句是往实例身上添加属性
     	state = {
          isHot: false
      }
      render(){
          return <div>{this.state.isHot? '炎热': '凉爽'}</div>
      }
  }

## 组件-props属性

- 类似Vue的props

  ```jsx
  import {Component} from 'react'
  class Demo extends Component{
      render(){
          return (
          	<ul>
              	<li>姓名：{this.props.name}</li>
                  <li>年龄：{this.props.age}</li>
              </ul>
          )
      }
  }
  // 通过标签传入的键值存在实例身上的props对象
  ReactDOM.render(<Demo name="tom" age={15}/>, document.getElementById('test'))
  ```

- ==批量==传递键值

  ```jsx
  class Demo extends Component{
      render(){
          return (
          	<ul>
              	<li>姓名：{this.props.name}</li>
              </ul>
          )
      }
  }
  let p = {name:'tom', age:18, sex:'male'}
  // 这是react特殊设置 {}是react的 正常情况是不能用...obj展开对象
  ReactDOM.render(<Demo {...p}/>, document.getElementById('test'))
  ```

- `props`类型、必要性限制以及设置默认值

  ```jsx
  // 从react 16版本以后不再使用React.PropTypes
  <script ... src="../prop-types.js"></script>
  class Person extends Component{
      render(){
          return (
          	<ul>
              	<li>姓名：{this.props.name}</li>
              </ul>
          )
      }
  }
  // propTypes是固定写法 写了propTypes后react就会识别给对应类加规则
  // PropTypes.类型 isRequired表示必传
  Person.propTypes = {
      name: PropTypes.string.isRequired,
      age: PropTypes.number,
      sex: PropTypes.string,
      speak: PropTypes.func, // 注：函数类型是func
  }
  // 设置未接收传参时的默认值 defaultProps固定写法
  Person.defaultProps = {
      age: 18,
      sex: '不男不女'
  }
  function fn() {}
  ReactDOM.render(<Person name="tom" age={18} speak={fn}/>)
  ```

- 利用`Class`特性简写`props`

  ```jsx
  class Person extends Component{
      // Person.propTypes其实就是给类身上添加属性 用static关键字可简写
      static propTypes = {
          name: PropTypes.string.isRequired,
          age: PropTypes.number,
          sex: PropTypes.string,
          speak: PropTypes.func, // 注：函数类型是func
  	}
      static defaultProps = {
          age: 18,
          sex: '不男不女'
  	}
      render(){
          return (
          	<ul>
              	<li>姓名：{this.props.name}</li>
              </ul>
          )
      }
  }
  ```

- 构造器和`props`

  ```jsx
  // 如果希望在构造器中使用props 则必须调用super
  class Person extends Component{
      constructor(props){
          super(props)
          // 如果没有调用super 以及传入props 则 在构造器内 通过实例this访问props是undefined
          console.log('构造器', this.props)
      }
  }
  ```

- `props`属性是==只读==的，不能修改

- 标签体内容也在`props`中

  - `children`是一种特殊标签属性，所以可以用批量传递键值的方式简写

  ```jsx
  import {Component} from 'react'
  class A extends Component {
      render() {
          return <B>222</B>
      }
  }
  class B extends Component {
      render() {
          let {children} = this.props // children为222
          return <h2>{children}</h2>
          // 简写
          return <h2 {...this.props}></h2>
      }
  }

## 组件-refs属性

- 类似Vue的`$refs`属性

  ```jsx
  class Demo extends Component{
      fn = () => {
          console.log(this.refs.aaa.value)
      }
      render(){
          return (
          	<div>
                  {/*字符串形式 过时版本 未来可能移除*/}
              	<input ref="aaa" onBlur={fn}>
              </div>
          )
      }
  }
  ```

- 回调形式的`ref`

  ```jsx
  class Demo extends Component{
      fn = (c) => {
          this.input1 = c
      }
      fn2 = () => {
          console.log(this.input1.value)
      }
      render(){
          return (
          	<div>
              	<input ref="{fn}" onBlur={fn2}>
              </div>
          )
      }
  }
  ```

- 通过API`createRef`创建

  - 注！`createRef`创建的容器只能存一个节点，一个类组件下用同一个容器存多个`ref`标记节点，后一个会顶掉前一个

  ```jsx
  class Demo extends Component{
      // React.createRef会返回一个容器 该容器存储被ref标识的节点
      myRef = React.createRef()
      fn = () => {
          // 因为只能存一个节点 所以节点信息都在current下
          console.log(this.myRef.current.value)
      }
      render(){
          return (
          	<div>
              	<input ref="{this.myRef}" onBlur={fn}>
              </div>
          )
      }
  }
  ```

- Tips

  - 勿过度使用`ref`，如**事件发生**的节点和要**操作的节点**是同一个，可以通过事件回调取得该节点的DOM

    ```jsx
    class Demo extends Component{
        fn = (e) => {
            console.log(e.target.value)
        }
        render(){
            return (
            	<div>
                	<input onBlur={fn}>
                </div>
            )
        }
    }
    ```

## 函数组件-hooks

- `hooks`允许函数组件中使用`state`等React特性

  ```jsx
  function Person(props) {
      // useState固定书写形式 第一个是声明的state变量 第二个是该变量更新方法
      const [age, addAge] = useState(18)
      const add_age = () => {
          let t = age + 1
          // 相当于setState addAge不由自己写内部逻辑 是React自动生成的更新函数
          addAge(t)
      }
      
      const [loading, setLoading] = useState(true)
      const [personData, setData] = useState(null) // 没有初始值传null
      const get_data = async () => {
          setLoading(true)
          let res = await fetch('...')
          let json = await res.json()
          setData(json)
          setLoading(false)
      }
      // useEffect副作用钩子 用于发送网络请求、DOM操作等
      // 在组件渲染后执行也可以在组件卸载时清理资源
      useEffect(() => {
          // 执行副作用操作
          get_data()
          return () => {
              // 组件卸载时的清理操作
          }
      }, [依赖]) // 依赖项数组 控制何时触发副作用
      
      return (
      	<div>
          	<span>年龄：{age}</span>
              <button onClick={add_age}></button>
              <div>获取服务器数据：{personData}</div>
          </div>
      )
  }
  ```

- `useEffect`

  - 通过传入不同参数，用一个函数实现不同生命周期，`componentDidUpdate`、`componentDidMount`、`componentWillUnmount`

  ```jsx
  // componentDidMount
  useEffect(() => { })
  
  // componentDidUpdate
  // 第二个参数为要观察的依赖,依赖变化则执行副作用函数
  useEffect(() => { }, [dependency])
  
  // componentWillUnmount
  // 在useEffect Hook 的 回调函数 中 返回一个函数
  useEffect(() => {
      // 之前的Hook中创建了监听事件window.addEventListener("mousemove", () => {})
      // Unmount前就要卸载事件 卸载的方式是返回一个函数
      return () => { window.removeEventListener("mousemove", () => {}) } 
  }, [] )
  ```

  - `useEffect`与`useLayoutEffect`异同
    - 相同点
      - 用法相同
    - 不同点
      - `useEffect`的执行时机是浏览器完成渲染之后
      - `useEffect`是异步执行的，而`useLayoutEffect`是同步执行的
      - `useLayoutEffect`的执行时机是浏览器把内容真正渲染到界面之前，和`componentDidMount`等价

## 组件间传值

### 父往子传数据

- 用`props`属性

  ```jsx
  // 父组件
  import React, {Component} from 'react'
  import Son from '../components/Son'
  class Parent extends Component{
      state = {
          list: []
      }
      render() {
          return (
          	<Son list={list}/>
          )
      }
  }
  // components/Son下子组件
  class Son extends Component{
      render() {
          let {list} = this.props
          return (
          	<div>
              	{list.map(e => {
                      return <div key={e}>{e}</div>
                  })}
              </div>
          )
      }
  }

### 子往父传数据

- 在父组件定义函数传入子组件，子组件触发函数传递数据

  ```jsx
  // 父组件
  import React, {Component} from 'react'
  import Son from '../components/Son'
  import Son2 from '../components/Son2'
  class Parent extends Component{
      state = {
          list: []
      }
      son2ToParent = (data)=>{
          this.setState({
              list: data
          })
      }
      render() {
          return (
          	<Son list={list}/>
              <Son2 fn={this.son2ToParent}/>
          )
      }
  }
  // components/Son2下子组件
  class Son2 extends Component{
      passData = (newList)=>{
          return () => {
              this.props.fn(newList)
          }
      }
      render() {
          return (
          	<button onClick={this.passData(this.newList)}>
                  修改父组件传递给Son的list
              </button>
          )
      }
  }

### 父组件到后代组件的跨组件通信

- 用`<BatteryContext.Provider>`创建一个大容器，将其中的数据共享给==组件树==，对于==组件树==而言是==全局变量==

  - 如果不用，就是`props`一层层传递下去

  ```jsx
  // 父组件
  class Parent extends Component {
      state = {
          color: 'red'
      }
      render() {
          return (
              // 创造共享参数容器 要用Provider
          	<BatteryContext.Provider value={this.state.color}>
              	<Child></Child>
              </BatteryContext.Provider>
          )
      }
  }
  
  // 子组件 因为只演示过度 形式简单 用函数组件形式
  const Child = (props) => {
      return (
      	<GrandChild></GrandChild>
      )
  }
  
  // 后代组件
  class GrandChild extends Component {
      fromParent = (share_color) => {
          return (
              // style接收一个对象 不是Vue的{{}}
          	<h1 style={ {color: share_color} }>红色</h1>
          )
      }
      render() {
          return (
              // 取用共享参数 用Consumer
          	<BatteryContext.Consumer>{this.fromParent}</BatteryContext.Consumer>
          )
      }
  }

### 任意组件通信

- 详见==消息订阅-发布==

## 事件绑定

- 所有原生监听事件，如`onclick`，react全部重写为驼峰式，如`onClick`

  - 注：事件绑定使用`{}`，里面写函数名而不是函数返回值

  ```jsx
  class Demo extends Component{
      constructor(props){
       super(props)
       this.state = {
           isHot: true
       }
       // change函数在实例的原型身上 只有实例调用的时候this才指向实例 所以要换绑复写
       // 因为class内是严格模式 再次触发调用change时this是undefined
       this.change = this.change.bind(this)
      }
      render(){
          return 
          <div onClick={this.change}>
              {this.state.isHot? '炎热': '凉爽'}
          </div>
      }
      change(){}
  }
  ```

- 事件绑定简写

  ```jsx
  class Demo extends Component{
     	state = {
          isHot: false
      }
      render(){
          return 
          <div onClick={this.change}>
              {this.state.isHot? '炎热': '凉爽'}
          </div>
      }
      // 自定义事件方法在触发时 是回到定义处看其作用域
      // 利用箭头函数的特性和类内声明实例固定参数
      change = () => {
          console.log(this) // this指向实例对象
      }
  }
  ```

- 事件绑定传参

  - 函数柯里化的应用：多个函数接收到不同参数，最后组合在一起

  ```jsx
  class Demo extends Component{
      state = {
          name: '',
          password: ''
      }
      save = (type) => {
          // 因为事件绑定的是save返回的函数 因此event事件对象是在返回值函数中传入
          return (event) =>{
              this.setState({
                  [type]: event.target.value
              })
          }
      }
      render(){
          return (
              {/*注意!这里为事件绑定的是save方法的返回值 而返回值是一个函数*/}
          	用户名：<input onChange={save('name')}>
              密码：<input onChange={save('password')}>
          )
      }
  }
  ```

  - 不用柯里化的形式

  ```jsx
  class Demo extends Component{
      state = {
          name: '',
          password: ''
      }
      save = (type, value) => {
          this.setState({
              [type]: value
          })
      }
      render(){
          return (
              {/*标签内传入一个匿名函数做壳 用于传递*/}
  			用户名：<input onChange={(event)=>{this.save('name', event.target.value)}}>
          )
      }
  }

## 数据绑定

- 使用原型链上`Component`的`setState`

  ```jsx
  class Demo extends Component{
      state = {
           isHot: true
   	}
      render(){
          return 
          <div onClick={this.change}>
              {this.state.isHot? '炎热': '凉爽'}
          </div>
      }
      change = () => {
          this.setState({
              isHot:!this.state.isHot
          })
      }
  }
  ```

- `setState`在**React事件**中==异步==，即React会将更新操作放入更新队列，但不是立即执行，使得批量处理多个更新，提高性能

  - **但是**在`setTimeout`或`DOM`事件中是==同步==执行！
  - 标签上驼峰式写法的事件如`onClick`是React事件，不是DOM原生事件

  ```jsx
  class Demo extends Component {
      state = {
          count: 0
      }
      render() {
          return (
          	<div>
              	<span>count:{count}</span>
                  <button onClick={this.btn}>增加</button>
                  <button id="btn2">增加2</button>
              </div>
          )
      }
      btn = () => {
          // setTimeout 同步执行
          setTimeout(() => {
              this.setState({
                  count: this.state.count + 1
              })
              console.log(this.state.count) // 1 说明立即更新了
          })
      }
      
      componentDidMount() {
          // DOM事件 同步执行
          document.querySelector('#btn2').addEventListener('click', () => {
              this.setState({
                  count: this.state.count + 2
              })
              console.log(this.state.count) // 2 说明立即更新了
          })
      }
  }

## 生命周期

- 示例

  ```jsx
  class Life extends React.Component{
      state = {opacity:1}
      disappear = () => {
          // 卸载组件 将挂载在节点上的组件卸载
          ReactDOM.unmountComponentAtNode(document.getElementById('test'))
      }
      // 初始化渲染(挂载)、数据更新时调用 等同Vue beforeMount 和 beforeUpdate
      render() {
          return (
          	<div>
              	<h2 style={opacity: this.state.opacity}>asdas</h2>
                  <button onClick={this.disappear}>消失</button>
              </div>
          )
      }
      // 组件挂载完毕时调用 等同Vue mounted
      // 常用 一般用于初始化 发送请求 订阅消息
      componentDidMount() {
          this.timer = setInterval(()=>{
              let {opacity} = this.state
              opacity -= 0.1
              if (opacity <= 0) opacity = 1
              this.setSate({opacity})
          }, 200)
      }
      // 组件 将要 卸载时调用 等同Vue beforeDestroy
      // 常用 一般用于收尾 关闭定时器 取消消息订阅
      componentWillUnmount() {
          clearInterval(this.timer)
      }
      
  }
  ```

- 生命周期

  - `constructor(props)`：构造器

    - 接收父组件传入参数`props`

  - `getDerivedStateFromProps`：`render`前调用，即初次渲染和更新渲染之前调用

    - 获取`props`派生`state`
    - 极少用，默认返回`null`，使用场景：组件内`state`纯由父组件传入的`props`决定

  - `render`：初次渲染和更新时调用

  - `componentDidMount`：组件挂载完成时调用

  - `shouldComponentUpdate`：阀门，控制组件是否允许更新

  - `getSnapshotBeforeUpdate`：更新前调用

    - 类似Vue的`watch`监听器，可以获取到`state`和`props`值改变之前的`oldValue`
    - ==返回值==将传递给`componentDidUpdate`

    ```jsx
    // 实现一个自动填入并滚动新闻列表
    class Demo extends Component {
        state = {list: []}
        myRef = React.createRef()
        componentDidMount() {
            setInterval(()=>{
                let {list} = this.state
                // 生成
                let t = '新闻' + (list.length + 1)
                // 将新插入的放在最前面
                this.setState({list: [t, ...list]})
            },1000)
        }
        // 接收两个参数 更新前props 更新前state
        getSnapshotBeforeUpdate(preProps, preState) {
            return this.myRef.scrollHeight
        }
        // 更新后接收到getSnapshotBeforeUpdate返回值
        componentDidUpdate(preProps, preState, height) {
            // 向下滚动 用 当前内容高度 减去改变 更新前高度得到向下滚动距离
            this.myRef.scrollTop += (this.myRef.scrollHeight - height)
        }
        render() {
            return (
            	<div id="list" ref="{this.myRef}">
                	{
                        this.state.list.map((e, index) => {
                            return <div key={index}>{e}</div>
                        })
                    }
                </div>
            )
        }
    }

  - `componentDidUpdate(preProps, preState)`：更新完成时调用

    - 接收两个参数，更新前`props`、更新前`state`
    -  可以直接调用`setState()`，但==必须放在条件语句里==

  - `componentWillUnmount`：组件卸载前调用

- 生命周期**执行顺序**

  ```jsx
  // 组件创建时
  constructor()
  static getDerivedStateFromProps()
  render()
  componentDidMount()
  
  // 更新中
  static getDerivedStateFromProps()
  shouldComponentUpdate()
  render()
  getSnapshotBeforeUpdate()
  componentDidUpdate()
  
  // 组件卸载时
  componentWillUnmount()

## 列表渲染

- `{}`中的表达式是**数组**，则会自动遍历渲染

  ```jsx
  class Demo extends Component{
      state = {
          list: [
              {label:'11', value: 'aa'},
              {label:'22', value: 'bb'},
              {label:'33', value: 'cc'},
          ]
      }
      render() {
          return (
          	<div>
              	{
                      this.state.list.map((e, index) => {
                      	return <div key={e.value}>{e.label}</div>
                  	})
                  }
              </div>
          )
      }
  }
  ```

## 条件渲染

- React中没有Vue`v-if`、`v-show`这样的指令，要实现同样的效果

  ```jsx
  // 实现v-if效果
  class A extends Component {
      state = {
          show: false
      }
      render() {
          let {show} = this.state
          if(show) {
              return <h2>xxx1</h2>
          }else {
              return <h2>xxx2</h2>
          }
      }
  }
  // 实现v-show效果
  class A extends Component {
      state = {
          show: false
      }
      render() {
          let {show} = this.state
          return (
          	<h2 style={{ display: show ? '' : 'none' }}>xxx3</h2>
          )
      }
  }

## 子元素分组

- 一个组件只能有**一个根节点**，使用`<Fragment>`可以将多个元素组合，==作为一个元素返回==

  - 缩写形式`<>...</>`
    - 缩写时不能添加任何标签属性
    - 循环渲染时，不能用简写形式，必须用`<Fragment>`，且每一个元素分配一个 `key`，以确保正确渲染
  - ==不会添加额外的DOM节点==，毕竟那样会影响HTML结构及样式布局

  ```jsx
  function Parent() {
      return (
          {/*相当于Fragment作为根节点*/}
      	<>
  			<Child1 />
        		<Child2 />
          </>
      )
  }

## Loading状态、替换模板

- 可以用在任何位置，作为内容加载时的后备方案，实现类似Vue的`v-loading`效果

  - 数据加载完成会自动切换渲染实际内容

  ```jsx
  import React from 'react';
  // 使用时需要引入
  import { Suspense } from 'react'
  
  function ArtistPage({ artist }) {
    return (
      <>
        <h1>{artist.name}</h1>
        {/*fallback传入加载时 代替显示的组件*/}
        <Suspense fallback={<Loading />}>
          <Albums artistId={artist.id} />
        </Suspense>
      </>
    );
  }
  
  function Loading() {
    return <h2>⏳ Loading...</h2>;
  }

## 组件懒加载

- 将较大的组件拆分成单独的文件，在需要渲染时加载

  ```jsx
  // 懒加载需要引入后使用
  import { lazy } from 'react'
  // 传入回调函数
  const ChildComponent = lazy(() => import('./child1'))
  function Parent() {
      return (
      	<>
          	{/*接收lazy返回值的变量 直接 用作组件标签使用*/}
          	<ChildComponent />
          </>
      )
  }
  ```

- 一般搭配`<Suspense>`加载状态使用

  ```jsx
  import { Suspense, lazy } from 'react'
  
  const ChildComponent = lazy(() => import('./child1'))
  
  function Parent() {
    const [show, setShow] = useState(false)
    return (
      <>
        <label>
          <input
            type="checkbox"
            checked={show}
            // 用show对应的set方法将show改为传入的值
            onChange={e => setShow(e.target.checked)}
          />
          Show preview
        </label>
        {show && (
    		 <Suspense fallback={<Loading />}>
          	<ChildComponent />
           </Suspense>
    	  )}
      </>
    );
  }
  function Loading() {
    return <h2>⏳ Loading...</h2>;
  }

## 卸载组件

- `ReactDOM.unmountComponentAtNode`
  - React18中已用`root.unmount()`取代
  - 成功移除组件返回`true`
  - 没有要移除的组件返回`false`

## react脚手架

- ==工程化==项目所必须
  - 工程化：代码编译、检查等操作自动化执行

- 步骤
  1. `npm i -g create-react-app`全局安装
     - 没有`-g`全局安装，会使得在每一处创建工程项目时都
  2. 切换到想创建项目的文件目录，`create-react-app xxx项目名`
  3. 进入项目文件夹，使用`npm start`运行项目

- 示例

  - `public`下`index.html`(主界面)
    - `index.html`不能改名，这是在`webpack`里配置好的，`index.js`绑定容器必会去`public`下寻找`index.html`文件


  ```html
  <head>
      <!-- %PUBLIC_URL%代表publick文件夹路径 -->
      <link rel="icon" href="%PUBLIC_URL%/favicon.ico"/>
      <!-- 开启理想视口 用于做移动端适配 -->
      <meta name="viewport" content="width=device-width, initial-scale=1"/>
      <!-- 用于配置浏览器页签的颜色 只作用于安卓移动端 -->
      <meta name="theme-color" content="#000"/>
      <!-- 用于指定网页添加到手机主屏幕的图标 -->
      <link rel="apple-touch-icon" href="%PUBLIC_URL%/logo.png"/>
      <!-- 网页套壳做安卓、IOS软件时的配置文件 -->
      <link rel="manifest" href="%PUBLIC_URL%/manifest.json"/>
  </head>
  <body>
      <div id='root'></div>
  </body>
  ```

  - `src`下`index.js`(入口文件)

  ```jsx
  // 在入口文件中引入react库文件
  import React from 'react'
  import ReactDOM from 'react-dom'
  // 引入组件 .js文件可以省略后缀
  import App from './App'
  // 绑定到index.html的真实DOM节点
  ReactDOM.render(<App/>, document.getElementById('root'))
  // StrictMode标签是帮助审查其包含标签的代码 给出警告提示
  ReactDOM.render(<React.StrictMode>
                      <App/>
                  </React.StrictMode>,
                  document.getElementById('root'))
  ```

  - `src`下`App.js`(根组件)
    - 每个模块化组件文件中都需要引入库文件，只要用的就要引入

  ```jsx
  // 任何东西包括图片都可以作为模块
  import logo from './logo.svg'
  // import导入的样式文件作用于组件及其子组件
  import './App.css'
  import React, {Component} from 'react'
  // 如果引入的js文件以index命名 则可省略不写
  import Hello from './components/Hello'
  // 根组件内容
  // 作为模块暴露出去 在入口文件中引入
  export default class App extends Component {
      render() {
          return (
          	<div>
              	<Hello/>
              </div>
          )
      }
  }
  ```

- `src/components/Hello`下`index.js`(模块化组件)
  - 组件文件夹最好用大写首字母，表示这是一个组件


```jsx
import React, {Component} from react
import './index.css'
export default class Hello extends Component {
    render() {
        return (
        	<div>Hello</div>
        )
    }
}
```

## 样式模块化

- 样式文件**虽然**是在**各自**组件中引入，**但是**最后汇总到`App.js`，样式会混杂在一块，后引入的组件样式会覆盖前面的组件同名样式

  - 但是使用模块化样式类名只能平铺，如样式

    ```less
    .title{
        > .text{
            .icon {
                ...
            }  
        }
    }
    .icon {}
    ```

    - 调用样式时

    ```jsx
    import hello from './index.module.css'
    export default class Hello extends Component {
        render() {
            return (
                {/* 当有同类名时就会出错 所以用模块化的样式任意层级不能有同名类名 */}
            	<h2 className={hello.title}>
                	<div className={hello.text}>
                    	<img className={hello.icon}/>
                    </div>
                </h2>
            )
        }
    }

- 方法

  1. `xxx.css`改名为`xxx.module.css`

  2. `xxx.module.css`

     ```css
     .title{
         color: red;
         background: yellow;
     }

  3. 在`xxx.js`中引入

     ```jsx
     import React, {Component} from 'react'
     // 引入xxx.css文件不能用别名
     import './index.css'
     // 但是改为xxx.module.css后 可以用别名
     import hello from './index.module.css'
     export default class Hello extends Component {
         render() {
             return (
                 {/* 模块化样式后就需要用表达式来应用样式 */}
             	<h2 className={hello.title}>hello</h2>
             )
         }
     }
     ```

- **更建议**用`.less`将样式以组件分开划分

  ```less
  .hello{
      >.title{
          >.text{
              .icon{...}
          }
      }
  }

## 消息订阅-发布

- 通过工具库`PubSubJS`实现
  1. 安装`npm i pubsub-js --save`
  
     - `--save`表示安装在==生产==环境
  
  2. 引入`import PubSub from 'pubsub-js'`
  
  3. 订阅消息`PubSub.subscribe('eventName', (msg, data) => {})`
  
     - 类似Vue的`$on`
     - 注！与Vue不同的是，订阅消息的回调函数不止接收发布传来的数据，还有发布的消息名
  
     ```jsx
     class A extends Compoenent {
         componentDidMount() {
             this.id = PubSub.subscribe('aaa', (msg, data) => {
                 console.log(msg) // 'aaa'
                 console.log(data) // '123'
             })
         }
         componentWillUnmount() {
             // 组件卸载时注销订阅
             PubSub.unsubscribe(this.id)
         }
     }
     class B extends Component {
         fn = () => {
             PubSub.publish('aaa', '123')
         }
         render() {
             return (
             	<button onClick={fn}>发送数据给A</button>
             )
         }
     }
  
  4. 发布消息`PubSub.publish('eventName', data)`
  
     - 类似Vue的`$emit`
     - 只能传一个参数`data`，**不能**`publish('eventName', data1, data2, ...)`
  
  5. 取消订阅`PubSub.unsubscribe(id)`
  
     ```jsx
     let id = PubSub.subscribe('eventName', () => {})
     // 在订阅消息时会获得订阅的id 就是根据这个id取消订阅
     // 与Vue不同 Vue中是取消所有同名事件的订阅 使用this.$bus.$off('eventName')
     PubSub.unsubscribe(id)

## 路由react-router

- react的一个==插件库==

  - 有三种实现，分别给三个平台用
    - Web：使用`react-router-dom`库，针对web使用
      - 安装：`yarn add react-router-dom`
    - react-native
    - any


### 示例

```jsx
import React, {Component} from 'react'
// 引入路由组件
// 注: Link等都是 组件 所以引入时首字母大写
// 注: 不能在此处引入BrowserRouter 因为路由跳转必须由同一个路由器管理才能实现 如果东包一个BrowserRouter 西包一个BrowserRouter就无法跳转
import {Link, Route, NavLink} from 'react-router-dom'
// 引入路由分类显示组件
// 注: 用于路由的组件应该放在pages文件夹下 不与普通组件混用
import Aaa from './pages/Aaa'
import Bbb from './pages/Bbb'
export default class A extends Component {
    render() {
        return (
        	<div id="index">
            	<div className="left_nav">
                    {/*注: to不识别大小写 编写路由链接*/}
                    <Link className="xx" to="/aaa">跳转aaa</Link>
                    <Link to="/bbb">跳转bbb</Link>
                </div>
                
                <div className="right_content">
                	{/*注册路由
                	path 匹配路径
                	component 对应组件*/}
                    <Route path="/aaa" component="{Aaa}"/>
                    <Route path="/bbb" component="{Bbb}"/>
                </div>
            </div>
        )
    }
}
```

- 在根目录`index.js`中引入`BrowserRouter`
  - 所有==路由组件==需要用`xxxRouter`组件包裹才能使用
    - 可包裹`<BrowserRouter>`或`<HashRouter>`
      - 且路由跳转必须由同一个==路由器==管理才能实现
    - 使用`<HashRouter>`地址栏路径会多出`#`，如`localhost:80/#/aaa`

```jsx
import React from 'react'
import ReactDOM from 'react-dom'
import App from './App'
import {BrowserRouter} from 'react-router-dom'
ReactDOM.render(
	<BrowserRouter>
    	<App/>
    </BrowserRouter>,
    document.getElementById('root')
)
```

### 路由组件和一般组件的区别

- 写法不同
  - 一般组件：`<Aaa/>`
  - 路由组件：`<Route component="{Aaa}"/>`
- 存放位置不同
  - 一般组件：`/components`
  - 路由组件：`/pages`

- 接收到的`props`不同
  - 一般组件：写组件时传什么就收什么
  - 路由组件：接收固定属性
    - `history`
    - `location`
    - `match`

### 导航激活样式设置

- 可以在`<Link>`组件上添加动态样式，或者使用`<NavLink>`自动添加激活样式
- `<NavLink>`可以实现路由链接的高亮

```jsx
import React, {Component} from 'react'
import {Route, NavLink} from 'react-router-dom'
import Aaa from './pages/Aaa'
import Bbb from './pages/Bbb'
export default class A extends Component {
    render() {
        return (
        	<div id="index">
            	<div className="left_nav">
                    {/*自动实现激活样式需要使用NavLink
                    激活的样式由activeClassName属性指定*/}
                    <NavLink activeClassName="style1" className="xx" to="/aaa">
                        跳转aaa
                    </NavLink>
                    <NavLink activeClassName="style1" to="/bbb">
                        跳转bbb
                    </NavLink>
                </div>
            </div>
        )
    }
}
```

- 为了更简便使用，对`<NavLink>`再进行一次封装

```jsx
// /components/Mynavlink/index.js
import React, {Component} from 'react'
import {NavLink} from 'react-router-dom'
export default class Mynavlink extends Component {
    render() {
        let {to, content} = this.props
        return (
        	<NavLink activeClassName="style1" className="xx" to={to}>
                {content}
            </NavLink>
        )
        // 利用props特性进行简写
        return (
        	<NavLink activeClassName="style1" className="xx" {...this.props}/>
        )
    }
}
// 在调用Mynavlink的组件内
import Mynavlink from '../components/Mynavlink'
export default class Demo extends Component {
    render() {
        return (
        	<Mynavlink to={'/aaa'} content={'跳转aaa'}></Mynavlink>
            <Mynavlink to={'/bbb'} content={'跳转bbb'}></Mynavlink>
        )
        // 利用props特性进行简写
        return (
            <Mynavlink to={'/aaa'}>跳转aaa</Mynavlink>
            <Mynavlink to={'/bbb'}>跳转bbb</Mynavlink>
        )
    }
}
```

### `switch`组件

- `<Route path="/xxx" />`组件匹配到对应路径后不会停止，而是继续往下匹配

```jsx
// 引入switch组件
import {Route, Switch} from 'react-router-dom'
export default class A extends Component {
    render() {
        return (
        	<div id="index">
            	<div className="left_nav">
                    <Link className="xx" to="/aaa">跳转aaa</Link>
                    <Link to="/bbb">跳转bbb</Link>
                </div>
                
                <div className="right_content">
                    {/* 用Switch组件包裹即可 */}
                    <Switch>
                        <Route path="/aaa" component="{Aaa}"/>
                        <Route path="/bbb" component="{Bbb}"/>
                    </Switch>
                </div>
            </div>
        )
    }
}
```

### 样式丢失问题

- 现象：像`a/b`这样多级路径，在刷新页面时会以`http://x.x.x.x/a/b`这样的地址重新加载，所以会导致资源请求出错
- 解决方法
  - `.html`中引入样式`./css/xxx.css`改为`/css/xxx.css`
    - 表示从根路径找
  - `.html`中引入样式`./css/xxx.css`改为`%PUBLIC_URL%/css/xxx.css`
    - 表示从根路径`public`文件夹下寻找
  - 使用`<HashRouter>`，地址栏后会有`#`，浏览器会忽略`#`后的路径，使得浏览器刷新不受地址影响

### 模糊匹配

- 用于`/xxx?aa=zz`地址栏传值
- 在`<Route>`标签上使用`<Route exact={true}>`开启精准匹配，`path="/aa"`无法匹配`to="/aa/bb/cc"`

```html
<!-- 可以模糊匹配 -->
<NavLink to="/aa/bb/cc"></NavLink>
<switch>
	<Route path="/aa"></Route>
</switch>
<!-- 匹配失败 -->
<NavLink to="/aa"></NavLink>
<switch>
	<Route path="/aa/bb/cc"></Route>
</switch>
```

### 重定向

- 用于没有路由匹配时重新指向的路由

```jsx
// 需要引入Redirect组件
import {Route, Switch, Redirect} from 'react-router-dom'
// 当地址栏是 http(s)://IP:端口 不带路径时 兜底重新指向一个路由
export default class A extends Component {
    render() {
        return (
        	...
            <Switch>
            	<Route path="/aa" component={Aa}></Route>
                <Route path="/bb" component={Bb}></Route>
                {/*没有匹配到时 重新指向Aa路由*/}
                <Redirect to="/aa"/>
            </Switch>
        )
    }
}
```

### 多级路由

- 父页面

```jsx
// import引入
import Home from './components/Home'
import About from './components/About'
export default class Demo extends Component {
    render() {
        return (
        	<div>
            	<div class="left">
                	<NavLink to="/home"></NavLink>
                    <NavLink to="/about"></NavLink>
                </div>
                <div class="right">
                	<Switch>
                    	<Route path="/home" component={Home}></Route>
                        <Route path="/about" component={About}></Route>
                    </Switch>
                </div>
            </div>
        )
    }
}
```

- 子组件`Home`

```jsx
// import引入
import Aa from './Aa'
import Bb from './Bb'
export default class Home extends Component {
	render() {
        return (
        	<div>
            	<div class="head">
                    {/* 多级路由的子路由 必须拼写父级的路径
                    地址栏/home/aa由模糊匹配进入父级路由/home
                    再匹配到子路由/home/aa*/}
                	<NavLink to="/home/aa"></NavLink>
                    <NavLink to="/home/bb"></NavLink>
                </div>
                <div class="body">
                	<Switch>
                    	<Route path="/home/aa" component={Aa}></Route>
                        <Route path="/home/bb" component={Bb}></Route>
                        <Redirect to="/home/aa"/>
                    </Switch>
                </div>
            </div>
        )
    }
}
```

### 路由传参的方式

#### params参数

- 父级路由

```jsx
export default class Demo extends Component {
    render() {
        const list = [
            {id:'1', name:'xx'},
            {id:'2', name:'ss'},
            {id:'3', name:'vv'},
        ]
        return (
        	<div>
            	<div class="left">
                    {
                        list.map(e => {
                            return (
                                {/*params用/xx的形式地址栏传参*/}
                            	<NavLink to={`/home/${e.id}/${e.name}`}></NavLink>
                            )
                        })
                    }
                </div>
                <div class="right">
                	<Switch>
                        {/*声明接收params参数 不然模糊匹配后面的就忽略了*/}
                        {/*params匹配规则 只与顺序有关 与:命名无关*/}
                    	<Route path="/home/:newid/:newname" component={Home}></Route>
                    </Switch>
                </div>
            </div>
        )
    }
}
```

- 子页面

```jsx
export default class Home extends Component {
    render() {
        // params参数存放在props属性
        let {newid, newname} = this.props.match.params // newid:'1' newname:'xx'
        return (
        	<div>
            	<div>id:{newid}</div>
                <div>name:{newname}</div>
            </div>
        )
    }
}
```

#### search参数

- 父级路由

```jsx
export default class Demo extends Component {
    render() {
        const list = [
            {id:'1', name:'xx'},
            {id:'2', name:'ss'},
            {id:'3', name:'vv'},
        ]
        return (
        	<div>
            	<div class="left">
                    {
                        list.map(e => {
                            return (
                                {/*search用?key=value&key2=value2的形式地址栏传参*/}
                            	<NavLink to={`/home?id=${e.id}&name=${e.name}`}></NavLink>
                            )
                        })
                    }
                </div>
                <div class="right">
                	<Switch>
                        {/*search参数 无需声明接收*/}
                    	<Route path="/home" component={Home}></Route>
                    </Switch>
                </div>
            </div>
        )
    }
}
```

- 子页面
  - `search`参数在`this.props.location.search`中，以`?key=val`字符串形式存储，所以需要自己进行解析

```jsx
// 安装create-react-app时已经集成了query参数解析工具
import qs from 'querystring'
export default class Home extends Component {
    render() {
        // search参数存放在props属性
        let {search} = this.props.location // 开头?要去掉
        // 解析成{id:'1', name:'xx'}
        let {id, name} = qs.parse(search.slice(1))
        return (
        	<div>
            	<div>id:{id}</div>
                <div>name:{name}</div>
            </div>
        )
    }
}
```

#### state参数

- 父级路由
  - 用`state`传参，虽然不像`search`、`params`参数会在地址栏体现，但是保存在`history.location.state`下，刷新浏览器不会丢失参数

```jsx
export default class Demo extends Component {
    render() {
        const list = [
            {id:'1', name:'xx'},
            {id:'2', name:'ss'},
            {id:'3', name:'vv'},
        ]
        return (
        	<div>
            	<div class="left">
                    {
                        list.map(e => {
                            return (
                                {/*state用对象配置项形式传参 用state就可以传对象等复杂数据*/}
                            	<NavLink to={ {pathname:'/home', state:{data:e}} }></NavLink>
                            )
                        })
                    }
                </div>
                <div class="right">
                	<Switch>
                        {/*state参数 无需声明接收*/}
                    	<Route path="/home" component={Home}></Route>
                    </Switch>
                </div>
            </div>
        )
    }
}
```

- 子页面

```jsx
export default class Home extends Component {
    render() {
        // state参数存放在props属性
        let {data} = this.props.location.state
        return (
        	<div>
            	<div>id:{data.id}</div>
                <div>name:{data.name}</div>
            </div>
        )
    }
}
```

### 路由push和replace

- 路由跳转默认用`push`压栈，当点击浏览器后退键时，会一层一层出栈，显示效果就是：点击后退会返回上级路由内容

- 使用`replace`可以替换，当前这级路由，使其返回时可以跳过当前级路由，直接显示父级内容

  - 比如实现效果，页面回退直接返回前一个页面而不是一级一级路由回退
  - 其实`replace`更常用一些，因为`push`会存储每次点击的路由记录，返回时只能一级一级回退，而不是页面回到前一页

  ```jsx
  export default class Demo extends Component {
      render() {
          return (
          	<div>
              	<div class="left">
                      {/* 标签加replace属性开启 */}
                      <NavLink replace to='/home'></NavLink>
                  </div>
                  <div class="right">
                  	<Switch>
                      	<Route path="/home" component={Home}></Route>
                      </Switch>
                  </div>
              </div>
          )
      }
  }

### 编程式路由

- 调用==路由组件==的API进行跳转

  - `replace(path, state)`
  - `push(path, state)`
  - 回退`props.history.goBack()`
  - 前进`props.history.goForward()`

  ```jsx
  export default class Demo extends Component {
      replaceShow = (id, name) => {
          // params参数形式
          this.props.history.replace(`/home/${id}/${name}`)
          // search参数形式
          this.props.history.replace(`/home?id=${id}&name=${name}`)
          // state参数形式
          this.props.history.replace('/home',{id, name})
      }
      pushShow = (obj) => {
          // 除了方法不同 所有形式相同
          this.props.history.push('/home',{data: obj})
      }
      goback = () => {
          this.props.history.goBack()
      }
      forward = () => {
          this.props.history.goForward()
      }
      render() {
          const list = [
              {id:'1', name:'xx'},
              {id:'2', name:'ss'},
              {id:'3', name:'vv'},
          ]
          return (
          	<div>
              	<div class="left">
                      {/*注:必须用路由组件包裹才能使用这些方法*/}
                      <NavLink>
                          <button onClick={()=>{this.replaceShow('1', 'xx')}}>
                              点我replace跳转
                          </button>
                      </NavLink>
                      <NavLink>
                  		<button onClick={()=>{this.replaceShow('1', 'xx')}}>
                          	点我replace跳转
                      	</button>
                  	</NavLink>
                      <NavLink>
                      	<button onClick={()=>{this.pushShow(list[0])}}>
                          	点我push跳转
                      	</button>
                      </NavLink>
                      <NavLink>
                      	<button onClick={ ()=>{ this.goback() }}>
                          	点我后退
                      	</button>
                      </NavLink>
                      <NavLink>
                      	<button onClick={()=>{ this.forward() }}>
                          	点我前进
                      	</button>
                      </NavLink>
                  </div>
                  <div class="right">
                  	<Switch>
                          {/*params参数形式*/}
                      	<Route path="/home/:id/:name" component={Home}></Route>
                          {/*search及state参数形式*/}
                          <Route path="/home" component={Home}></Route>
                      </Switch>
                  </div>
              </div>
          )
      }
  }

- 一般组件使用编程式路由

  - `props.history`等属性方法只在路由组件身上有，需要使用`withRouter`方法加工普通组件使其拥有路由组件API

  ```jsx
  // 父页面
  import React, {Component} from 'react'
  import Header from './components/Header'
  import {Route, Switch} from 'react-router-dom'
  export default class Demo extends Component {
      render() {
          return (
          	<div>
              	<Header/>
                  <Switch>
                  	...
                  </Switch>
              </div>
          )
      }
  }
  // 头部返回控制组件
  // 引入withRouter方法
  import {withRouter} from 'react-router-dom'
  class Header extends Component {
      goback = () => {
          this.props.history.goBack()
      }
      forward = () => {
          this.props.history.goForward()
      }
      render() {
          return (
          	<div>
              	<button onClick={this.goback()}>
                      点我后退
                  </button>
                  <button onClick={this.forward()}>
                      点我前进
                  </button>
              </div>
          )
      }
  }
  // 需要经过withRouter加工后再返回
  export default withRouter(Header)

## redux状态管理

- 当大部分组件需要共享数据时，就是用`redux`进行全局的状态管理

  - `redux`只是进行共享数据处理，不负责更新到页面

- 示例

  - 自定义数据处理方法

  ```js
  // 作为redux的事件处理函数 会接收到2个参数
  export default function count(preValue = 0, action) {
      // preValue之前的值 action传入的自定义参数(对象形式)
      let {type, num} = action
      switch(type) {
          case 'add':
            return preValue + num
          case 'reduce':
            return preValue - num
          default:
            // 没有之前的值时 默认进行初始化操作
            // 没有初始值preValue就是undefined
            return preValue
      }
  }
  ```

  - 创建仓库`store`

  ```jsx
  // 引入createStore 专门用于创建store对象
  import {createStore} from 'redux'
  // 引入自定义函数方法
  import count from './count'
  // 传入自定义方法创建store
  export default createStore(count)
  ```

  - 取用共享数据的组件

  ```jsx
  // 使用store就要引入
  import store from '../redux/store.js'
  // 引入其他库
  ...
  export default class Demo extends Component {
      add = () => {
         // 通知redux执行增加方法 传入的是action参数
         store.dispatch({type: 'add', num: 3})
      }
  	reduce = () => {
          store.dispatch({type: 'reduce', num: 2})
      }
      render() {
          return (
          	<div>
                  {/*getState()获取当前共享数据值*/}
              	<div>计算结果:{store.getState()}</div>
                  <button onClick={this.add}>加3</button>
                  <button onClick={this.reduce}>减2</button>
              </div>
          )
      }
  }
  ```

  - 根目录入口文件`index.js`

  ```jsx
  // 检测到
  // 引入store
  import store from './redux/store'
  // 引入其他库
  ...
  // subscribe方法监测store中共享值发生变化
  store.subscribe(()=>{
      // 值变化后重新渲染变化的节点
      ReactDom.render(<App/>, document.getElementById('root'))
  })