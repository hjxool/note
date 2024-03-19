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
              	<input ref="aaa" onBlur={fn}>
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