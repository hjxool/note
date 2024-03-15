## 文档

- [中文网](https://react.docschina.org)
- 官方chrome浏览器插件`React Developer Tools`

## 示例

- html

  ```html
  <!--准备容器-->
  <div id="test"></div>
  
  <!-- 引入react核心库 -->
  <script type="text/javascript" src="../react.development.js"></script>
  <!-- 引入react-dom 用于支持react操作DOM -->
  <script ... src="../react-dom.development.js"></script>
  <!-- 引入babel，用于将jsx转为js -->
  <script ... src="../babel.min.js"></script>
  <!-- html内写jsx一定要写type="text/babel" -->
  <script type="text/babel">
  	// 1.创建虚拟DOM 在jsx中不需要写引号 因为不是字符串
  	const dom = <h1>hello</h1>
  	// 2.渲染虚拟DOM到页面 render(虚拟DOM, 容器)
  	ReactDom.render(dom, document.getElementById('test'))
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
  ReactDom.render(<Demo/>, document.getElementById('test'))
  ```

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
  ReactDom.render(<Demo/>, document.getElementById('test'))
  ```

  - 组件实例属性`state`(类似Vue的data)

  ```jsx
  class Demo extends Component{
      // state必须写在构造器内
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

- 事件绑定、`state`简写形式

  ```jsx
  class Demo extends Component{
      // 初始固定参数 不需要构造器 直接使用赋值语句
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