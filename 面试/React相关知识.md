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
