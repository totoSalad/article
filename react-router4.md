# react-router4 迁移踩坑

### 资料

[react-router4 官网](https://reacttraining.com/react-router/web/example/auth-workflow)

[迁移教程(官网)](https://github.com/ReactTraining/react-router/blob/master/packages/react-router/docs/guides/migrating.md#on-properties)



### 问题1：在`<Router>`外部跳转链接

#### 方案List：

##### 1. ~~使用`<Link>`跳转~~（扑街）

不能在`<Router>`外部使用`<Link>`

##### 2. ~~使用withRouter获取history跳转~~（扑街）

不能在<Router>外部使用withRouter()

##### 3. 直接使用history的push (ok!)

### why?

#### 什么是history？

#### history是怎么更新路由的？



### 问题2：`<Lnk>`的to属性要改成绝对路径



### 问题3：不存在onEnter/onLeave等handle

#### 解决方案：

`<Router> `包裹的渲染组件(非`<Router>` )的生命周期，代替handle

| on* propties |                渲染组件的生命周期                 |
| :----------: | :--------------------------------------: |
|   onEnter    |   componentDidMount、componentWillMount   |
|   onUpdate   | componentDidUpdate、componentWillUpdate、componentWillReceiveProps |
|   onLeave    |           componentWillUnmount           |



### 问题4: redux监听路由变化 

####  解决方案：

使用react-router-redux v5-beta

1. <connectedRouter /> 包裹router—— 创建store

```jsx
import { ConnectedRouter } from 'react-router-redux'

<ConnectedRouter history={history}>
	<div>
 		<Route exact path="/" component={Home}/>
		<Route path="/about" component={About}/>
		<Route path="/topics" component={Topics}/>
	</div>
</ConnectedRouter>
```

2. —— 创建reducer，以及中间件

```javascript
import { routerReducer, routerMiddleware } from 'react-router-redux'
// Build the middleware for intercepting and dispatching navigation actions
const middleware = routerMiddleware(history)

// Add the reducer to your store on the `router` key
// Also apply our middleware for navigating
const store = createStore(
  combineReducers({
    ...reducers,
    router: routerReducer
  }),
  applyMiddleware(middleware)
)
```



V4的构建之道

https://zhuanlan.zhihu.com/p/25696969

https://juejin.im/post/58ca005361ff4b006c812ad9