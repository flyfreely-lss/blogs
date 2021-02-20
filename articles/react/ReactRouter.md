# ReactRouter 记录

## 前端路由的实现原理 ##

原理：通过检测浏览器 url 的变化，截获 url 地址，然后进行 url 路由匹配。

目前检测 url 变化的方式有两种：

1. window.location.hash（锚点），监听 hashchange 事件

   - url格式：http://www.example.com/#/info

   - 刷新页面，浏览器不会向服务器发送请求

2. html5 的 history对象（ `pushState`，`replaceState` 和 `popstate` 事件），window.history

   - url格式：http://www.example.com/info

   - 刷新页面，浏览器会向服务器发送请求

## 安装

Web: react-router-dom

Native: react-router-native

## React Router常见组件

**Router 组件**

每一个router都会创建一个history对象，用来保持当前位置的追踪

Web: 

- HashRouter（只处理静态url）

- BrowserRouter(非静态站点，要处理不同的url)

HashRouter vs. BrowserRouter 区别

- URL的表现形式不一样

BrowseRouter：http://www.example.com/info

HashRouter：http://www.example.com/#/info

- 底层实现不一样

BrowserRouter使用HTML5的history API，保证UI界面和URL同步。 页面之间传通过state的方式传值给下一个页面的时候，当到达新的页面，刷新或者后退之后再前进，BrowseRouter传递的值依然可以从 window.history.state 中得到。

HashRouter使用URL的哈希部分来保持UI和URL的同步。刷新或者后退之后再前进，无法从window.location中得到 state 的值，所以当刷新路由后state值会丢失导致页面显示异常。

- BrowseRouter 需要server端配置，BrowserRouter不需要

参考地址：[React-Router browserHistory浏览器刷新出现页面404解决方案](https://www.thinktxt.com/react/2017/02/26/react-router-browserHistory-refresh-404-solution.html)

Native: MemoryHistory

**Route 组件**

**Switch 组件**

返回第一个匹配的组件，最多匹配一个。

使用场景 ：

1. 对Route进行分组
2. 404页面渲染兜底

**Link 与 NavLink 组件**

**Redirect 组件**

使用场景：根据登录状态跳转至不同的页面。

**History 对象**

每一个router都会创建一个history对象，此处的 history 与 window.history 不同，用来保持当前位置的追踪

可进行编程式导航： push

**withRouter 组件**

## 动态路由

路由规则不是预先确定的，而是在渲染过程中确定的

```javascript
info/:id
// 获取数据的方式
props.match.params.id
```

