#  react 入门

## 什么是 React

用于构建用户界面的 javascript 库。

## props介绍

**this绑定的四种方式**

1. 在 constructor 内绑定  推荐

   ```
   this.handleClick = this.handleClick.bind(this)
   ```

2. 直接在 jsx 元素进行绑定  不推荐

   ```
   <button onClick={this.handleClick.bind(this)}></button>
   ```

   缺点：会造成额外的渲染，影响性能

3. 使用箭头函数，ES6类字段  :star: 推荐 

   ```
   const handleClick = () => {}
   ```

4. 使用箭头函数，在 jsx 元素直接操作  不推荐

   ```
   <button onClick={() => this.handleClick()}></button>
   ```

## state介绍

**state更新时机**

- setState 异步更新，批量延迟更新 多个setState调用，合并处理，高效
  - React 控制的事件处理程序（onClick, onChange)、生命周期钩子函数 不会同步更新 state
- setState 同步更新
  - 原生js绑定的事件、setTimeout