# ReactRouter 记录

## 前端路由的实现原理 ##

原理：通过检测浏览器 url 的变化，截获 url 地址，然后进行 url 路由匹配。

目前检测 url 的方式有两种：

1. window.location.hash（锚点），监听 hashchange 事件

   - url格式：http://www.baidu.com/index.html#8

   - 刷新页面，浏览器不会向服务器发送请求

2. html5 的 history 

   - url格式：http://www.baidu.com/index/info

   - 刷新页面，浏览器会向服务器发送请求

## 安装

Web: react-router-dom

Native: react-router-native

## React Router常见组件

**Router 组件**

每一个router都会创建一个history对象，用来保持当前位置的追踪

Web: HashRouter（只处理静态url）、BrowserRouter(非静态站点，要处理不同的url)

Native: MemoryHistory

**Route 组件**

**Switch 组件**

最多匹配一个组件。

使用场景 ：

1. 对Route进行分组
2. 404页面渲染

**Link 与 NavLink 组件**

**Redirect 组件**

使用场景：根据登录状态跳转至不同的页面。

**History 对象**

每一个router都会创建一个history对象，用来保持当前位置的追踪

可进行编程式导航： push

**withRouter 组件**

## 动态路由

路由规则不是预先确定的，而是在渲染过程中确定的

```javascript
info/:id
// 获取数据的方式
props.match.params.id
```

