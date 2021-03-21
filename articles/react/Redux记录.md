# Redux 记录

## 什么是 Redux

Redux 是一个独立的状态管理库，提供可预测的状态管理。

应用中的所有 state 都以一个对象树的形式存储在一个单一的 store 中，唯一改变 state 的方式就是派发 action。

## 使用 Redux 的好处

Redux 解决了多级组件传递数据的痛苦

通过 connect 函数，将数据连接到任何组件

## 使用场景

1. 公共组件，业务组件非常多，用户使用方式比较复杂，项目庞大
2. 不同用户角色权限管理
3. 需要与服务器进行大量的交互，聊天、直播等应用
4. view 需要从多个来源获取数据
5. react 解决不了的多交互、多数据源。

## 如何工作的

### 三大核心

**action**

作用：描述发生了什么的指示器。

**reducer**

作用：把 action 和 state 串起来，描述 action 如何改变 state tree。

纯函数，固定的输入一定有固定的输出。

**store**

作用：维持应用的 state，并在当你发起 action 的时候调用 reducer。

Redux 只有一个单一的 store。

### 使用的一般过程

1. 创建 reducer
2. 创建 action
3. 创建 store

## 三大原则

1. 单一数据源：store 是全局变量对象。
2. state是只读的，唯一改变的方法是触发 action
3. reducer 使用纯函数来执行修改， 为了描述 action 如何改变 state tree ，你需要编写 [reducers](https://cn.redux.js.org/docs/Glossary.html#reducer)。

## 实际应用

副作用操作，使用 Redux-thunk，进行不纯的操作。

## 实例应用 心得记录

容器型组件中的两个重要方法

1.  `mapStateToProps` 这个函数来指定如何把当前 Redux store state 映射到展示组件的 props 中。

2. `mapDispatchToProps()` 方法接收 [`dispatch()`](https://www.reduxjs.cn/api/store#dispatch) 方法并返回期望注入到展示组件的 props 中的回调方法。

