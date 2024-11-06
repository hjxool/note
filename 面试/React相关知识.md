# 组件基础

## react事件机制

- `<div onClick={this.fn.bind(this)}>点我</div>`
  - React并不是将`click`事件绑定到了`div`的真实DOM上，而是在`document`处监听了所有的事件，当事件触发并且冒泡到`document`时，React通过合成事件将事件内容封装并交由真正的处理函数运行
  - 这种方式不仅减少了内存的消耗，还能在组件挂在销毁时统一订阅和移除事件
  - 注：Vue是将事件绑定到真实DOM上的

- 合成事件
  - 冒泡到`document`的事件不是原生的浏览器事件，而是react自己实现的合成事件
  - 因此==阻止事件冒泡==，应该调用`event.preventDefault()`，而非`event.stopProppagation()`

