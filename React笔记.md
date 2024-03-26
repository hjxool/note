## 文档

- [中文网](https://react.docschina.org)
- 官方chrome浏览器插件`React Developer Tools`

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
      <h1 class="title">
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
          return {
              <div>{this.props.name}</div>
          }
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
  class Person extends Component{
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

- 批量传递键值

  ```jsx
  class Person extends Component{
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
                  {/*字符串形式 过时版本 为了可能移除*/}
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

  - `componentWillUnmount`：组件卸载前调用

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
  import Hello from './components/hello/index'
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

- `src/components/hello`下`index.js`(模块化组件)

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