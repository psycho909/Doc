

# react-router-dom

```js
npm i -S react-router-dom
```

```js
import React, { Component } from 'react';
import './App.css';
import About from './About'
import Home from './Home'
import NoMath from './Error'

import {
  BrowserRouter as Router,
  Route,
  Link,
  Switch,
  Redirect
} from 'react-router-dom'

const User=(props)=>{
  return (
    props.match.params.name === 'chen'?
    <Redirect to="/" />:<div>User {props.match.params.name}</div>
    
  )
}

class App extends Component {
  render() {
    return (
      <Router>
        <div className="App">
          <ul>
            <li><Link to="/">Home</Link></li>
            <li><Link to="/about">About</Link></li>
            <li><Link to="/users">User</Link></li>
            <li><Link to={{
              pathname:"/pro",
              search:"?a=b",
              hash:"#the-hash",
              state:{fromDashboard:true}
            }}>pro</Link></li>
          </ul>
          <Switch>
            <Route path="/" exact component={Home}></Route>
            <Route path="/about"  render={(props)=> <About {...props} />}></Route>
            <Route path="/users/:name"  component={User}/>
            <Route component={NoMath} /> 
          </Switch>
        </div>
        
      </Router>
    );
  }
}

export default App;
```

`exact`完完全全跟路徑相等時，才顯示

`BowserRouter`代表著一個路由

`Route`代表著一個路由規則

`Link`替代`a`做頁面跳轉

## `Link`

1. 一般用法

```js
<Link to="/home">Home</Link>
```

2. 增強用法

state:可用於傳數據，又不出現在網址欄

```react
// http://localhost:3000/pro?a=b#the-hash
console.log(props.lcation.state) // {fromDashboard:true}

<Link to={{
 pathname:"/pro",
 search:"?a=b",
 hash:"#the-hash"
 state:{fromDashboard:true}
}}>pro</Link>
```



## `NavLink`

activeClassName（string）：設置選中樣式，默認值為 active；

activeStyle（object）：當元素被選中時, 為此元素添加樣式；

exact（bool）：為 true 時, 只有當地址完全匹配 class 和 style 才會應用；

strict（bool）：為 true 時，在確定位置是否與當前 URL 匹配時，將考慮位置 pathname 後的斜線；

isActive（func）：判斷鏈接是否激活的額外邏輯的功能；

```react
<NavLink
	exact
    to="/home"
>
Home    
</NavLnk>
```



## `Switch`

```react
// 常用於包覆 <Route />
<Switch>
    ...
	...
	<Route /> 
</Switch>
```

## `404`

```react
// 建立一個 404 component
// 在 Route 最後一個 放置
import NoMath from './Error'
<Switch>
    ...
	...
	<Route component={NoMath} /> 
</Switch>
```

## `render func`

可以對於`Route`進行公能力增強

```react
<Route path="/home" render={()=> <div>Home</div>} />
                            
<Route path="/home" render={(props)=> <Home {...props} />} />
```

## `URL parameters`

```js
// xxxx/users/chen
//
<Route path="/users/:name"  component={User}/>

// User.js
const User=(props)=>{
  return (
    <div>User {props.match.params.name}</div>
  )
}
```

## `query string`

1.可使用`URLSearchParams`去做分析

```react
// http://localhost:3000/users/weq?name=123

//
<Route path="/users/:name"  component={User}/>

// User.js
const User=(props)=>{
  const params=new URLSearchParams(props.location.search)
  console.log(params.get('name')) // 123
  return (
    <div>User {props.match.params.name}</div> // User weq
  )
}
```

2. 使用`query-string`庫去做處理

```js
npm i -S query-string
import queryString from 'query-string'

const User=(props)=>{
  const values=queryString.parse(props.location.search)
  console.log(values)
  return (
    <div>User {props.match.params.name}</div>
  )
}
```

## `redirect`路由導向

1. 用法1

```js
import {Redirect} from 'react-router-dom'
// 從/users/:name 跳轉到 /users/profile/:name
<Redirect from="/users/:name" to="/users/profile/:name" />
```

2.用法2，可根據情況去做判斷跳轉

```js
const User=(props)=>{
  return (
    props.match.params.name === 'chen'?
    <Redirect to="/" />:<div>User {props.match.params.name}</div>
    
  )
}
```

## `history push`跳轉頁面

可綁定事件跳轉頁面

```js
handleClick(){
    this.props.history.push('/')
}
```

## `withRouter`

有些組件是在其他組件定義，得不到props資訊，可藉由withRouter高階組件去取得

```js
// get不到props資訊
const Hello=(props)=>{
    return (
        <div>
            <button onClick={()=>console.log(props)}>Hi</button>
            <p>Hello</p>
        </div>
    )
}
```

```js
import {withRouter} from 'react-router-dom'
const Hello=(props)=>{
    
    return (
        <div>
            <button onClick={()=>console.log(props)}>Hi</button>
            <p>Hello</p>
        </div>
    )
}
const WithRouterHello=withRouter(Hello)

<div>
  <WithRouterHello/>
</div>
```



# 給React-Router添加路由頁面切換時的過渡動畫

```js
npm install react-transition-group --save
```

`react-transition-group`提供了三個React組件，分別是`<Transition>`，`<CSSTransition>`以及`<TranstionGroup>`，關於它們的詳細api還請各位去查閱官方文檔，這裡只是簡單介紹一下它們各自的用途：

*   `<Transition>`：通過`javascript`動態修改`style`的方式為子元素添加動畫，對比`<CSSTransiton>`多了幾個編程式的`props`可以配置

*   `<CSSTransition>`：相比`<Transition>`多了一個`classNames`可以配置，通過引入CSS以及動態更改子元素`className`的方式為子元素添加動畫（是不是像極了Vue裡的`<transition>`）

*   `<TranstionGroup>`：顧名思義，為多個子元素添加動畫，需要結合`<Transition>`或`<CSSTransition>`使用

    

# 正確使用方式

## `<Route>` 傳遞props to Component

```react
<Route path="/pokemon" render={() => <Pokemon someData={someData} /> } />
```

## `<Redirect> ` 正確跳轉頁面方式

```react
state={
    showPokemon:false
}
handleSubmit=()=>{
    this.setState({
        showPokemon:true
    })
}
render() {
    if(this.state.showPokemon){
    	return <Redirect to="/pokemon/" />   
    }
    return (
    	<div></div>
    )
}
```

## 具認證保護Routes

### 先建立`PrivateRoute`組件

```react
const PrivateRoute=({component:Component,...rest})=>{
    <Route {...rest} render={(props)=>(
    	utils.auth.isAuthenticated == true
        ? <Component {...props} />
        : < Redirect to="/login" />
    )} />
}
```

```react
<Route path="/digimon" component={Digimon} />
<PrivateRoute path="/yugioh" component={Yugioh} />
```

## 使用 rote config file

```react
export const routes=[
    {
        path:"pokemon",
        component:"Pokemon"
    },
    {
        path:"digimon",
        component:"Digimon"
    }
]
```

```react
import {routes} from './routes'

<Router>
    {
        routes.map((route)=>{
			<Route
            	key={route.path}
                path={route.path}
                component={route.component}
            />
        })
	}
</Router>
```

