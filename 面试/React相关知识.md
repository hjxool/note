# 组件基础

## react事件机制

- `<div onClick={this.fn.bind(this)}>点我</div>`
  - React并不是将`click`事件绑定到了`div`的真实DOM上，而是在`document`处监听了所有的事件，当事件触发并且冒泡到`document`时，React通过合成事件将事件内容封装并交由真正的处理函数运行
  - 这种方式不仅减少了内存的消耗，还能在组件挂在销毁时统一订阅和移除事件
  - 注：Vue是将事件绑定到真实DOM上的
- 合成事件
  - 冒泡到`document`的事件不是原生的浏览器事件，而是react自己实现的合成事件
  - 对原生浏览器事件来说，浏览器会==给监听器创建一个事件对象==，如果有很多事件监听，就需要分配很多的事件对象，造成高额的内存分配问题。但对于合成事件，有一个事件池专门来管理事件对象的创建和销毁，当事件需要被使用时，就会从池子中复用对象，事件回调结束后，就会销毁事件对象上的属性

## React事件和HTML事件区别

- 事件命名方式不同
  - 原生事件为全小写，如`onclick`
  - react 事件用小驼峰，如`onClick`
- 事件函数语法
  - 原生事件为==字符串==，如`onclick="fn"`
  - react 事件为==函数或函数引用==，如`onClick={fn}`
- 阻止浏览器的默认行为
  - HTML事件可以使用`return false`阻止浏览器默认行为
  - react 事件必须用`preventDefault()`

## React高阶组件(HOC)、props、hooks区别

- 可以理解为HOC、`props`都是`hooks`诞生前的替代品，为了解决代码复用问题

- HOC

  - 是一种复用组件的技巧，不是React API，接收一个组件作为参数，返回一个增强的新组件

  ```js
  // hoc的定义
  function fn(component, callback) {
    return class extends React.Component {
      constructor(props) {
        super(props);
        this.state = {
          aa: callback(data, props)
        };
      }
      // ...
      render() {
        // 使用新数据渲染被包装的组件
        return <component data={this.state.aa} {...this.props} />;
      }
    };
  
  // 使用
  const c2 = fn(Login,(data, props) => data.xxx(props.id))
  ```

- `props`

  - 类似Vue的`props`，就是父组件传入子组件共享数据的方式

  ```js
  class Component1 extends React.Components {
    state = {
      name: 'Tom'
    }
    render() {
      return (
        <div>
          { this.props.xxx(this.state) }
        </div>
      );
    }
  }
  
  // 调用方式
  <DataProvider xxx={data => (<h1>Hello {data.name}</h1>)}/>
  ```

- `hooks`

  - React 16.8 新增特性。可以在不编写**类组件class**的情况下使用`state`及其他的 React 特性

  ```js
  import React, { useState, useEffect } from 'react';
  
  function Counter() {
    // 使用 useState 初始化 count 状态
    const [count, setCount] = useState(0);
    // 使用 useEffect 从本地存储中获取初始计数值
    useEffect(() => {
      const savedCount = localStorage.getItem('count');
      if (savedCount) {
        setCount(parseInt(savedCount, 10));
      }
    }, []);
    // 使用 useEffect 在 count 变化时保存到本地存储
    useEffect(() => {
      localStorage.setItem('count', count);
    }, [count]);
    // 处理按钮点击事件
    const handleClick = () => {
      setCount(count + 1);
    };
    return (
      <div>
        <p>Count: {count}</p>
        <button onClick={handleClick}>Increment</button>
      </div>
    );
  }
  
  export default Counter;
  ```

## 对React-Fiber的理解

- 目的
  - 为了解决渲染的性能问题
- 特点
  - 将渲染工作分割成更小的单元，称为"fiber"，这些单元可以被暂停、终止或优先处理
  - 不同的更新可以赋予不同的优先级。如，用户输入和动画可以被赋予更高的优先级，以确保它们能快速响应
- 原理
  - 通过创建一个"fiber树"来跟踪组件的更新，节点对应组件树中的每一个React元素，其中包含组件、状态、props、DOM节点等信息
  - React-Fiber通过**协调**这些节点，决定哪些需要更新以及如何更新

## React.Component 和 React.PureComponent 区别

- `React.Component`

  - Class组件的基类，用于复杂组件，不过在React16后逐步被函数组件代替

- `React.PureComponent`

  - Class组件的基类，写法同`React.Component`，用于简单组件，`PureComponent`自动实现了`shouldComponentUpdate`生命周期方法，对比`props`和`state`来决定组件是否需要重新渲染，从而提升渲染性能。
  - 因此适合只挂载`props`和`state`，无复杂逻辑的"纯组件"

  ```jsx
  import React from 'react';
  // 当 App 组件的 state.value 更新时
  // MyComponent 只会在 value 发生变化时重新渲染
  class MyComponent extends React.PureComponent {
    render() {
      console.log('MyComponent rendered');
      return (
        <div>
          {this.props.value}
        </div>
      );
    }
  }
  // 对比React.Component
  class App extends React.Component {
    state = {
      value: 0
    };
    increment = () => {
      this.setState({ value: this.state.value + 1 });
    };
    render() {
      return (
        <div>
          <button onClick={this.increment}>Increment</button>
          <MyComponent value={this.state.value} />
        </div>
      );
    }
  }
  ```

- 在React16后函数组件有了Hooks，`React.PureComponent`存在的意义是什么？

  - `React.PureComponent`是Hooks没有引入之前，用于优化类组件性能的解决方案，在React16之前仍是重要工具

- 与Hooks对比

  - 函数组件中，可以使用`React.memo`来实现类似于`React.PureComponent`的优化
  - 注意：只能用于==函数组件==
  - 本质是通过`React.memo`==包装函数组件==，实现`props`==自动浅比较==。本质还是函数组件，因此可以用函数组件的所有东西

  ```jsx
  import React from 'react';
  
  // 定义一个简单的函数组件
  const MyComponent = ({ name }) => {
    return <div>Hello, {name}!</div>;
  };
  
  // 使用React.memo包装 函数组件 
  const MemoizedComponent = React.memo(MyComponent);
  // 也可以自定义比较函数控制props比较逻辑
  const MemoizedComponent = React.memo(MyComponent, (pre, next) => {// 类似shouldComponentUpdate
      return pre.name === next.name
  })
  const App = () => {
    const [count, setCount] = React.useState(0);
    return (
      <div>
        <button onClick={() => setCount(count + 1)}>Increment</button>
        <MemoizedComponent name="Alice" />
      </div>
    );
  }
  ```

## 元素 组件 实例 之间的关系

- 元素
  - 创建之后不可变，描述了一个DOM节点，在屏幕上呈现成什么样子
- 组件
  - 可通过带有`render()`方法的class类
  - 或有`return ('模板')`的函数创建
- 实例
  - **只有**用类声明的组件，即有使用`this`关键字指向的组件，才有实例
  - 但是不需要直接创建一个组件的实例，因为React帮我们做了这些
