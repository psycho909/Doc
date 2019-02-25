## props，state與render函數的關係

1. 當組件的state或者props發生改變的時候，render函數就會重新執行
2. 當父組件的render函數被運行時，他的子組件的render都將被重新運行一次

## 父組件通過屬性的形式向子組件傳遞參數

```react
// Father
this.state={
    data:[1,2,3]
}
<div>
    {
        this.state.data.map((item,index)=>(
        	<Child item={item}  key={index} />
        ))
    }
</div>

// Child
<div>
    <span>{this.props.item}</span>
</div>

```



## 子組件通過props接受父組件傳遞過來的參數 

### 第一種

```react
// Father
this.state={
    data:[1,2,3]
}
handleOnDelete(index){
    console.log(index)
}
<div>
    {
        this.state.data.map((item,index)=>(
        	<Child item={item} index={index} delete={this.handleOnDelete.bind(this)} key={index} />
        ))
    }
</div>

// Child
handleOnDelete(){
    this.props.delete(index)
}
<div>
    <span onClick={this.handleOnDelete.bind(this)}>{this.props.item}</span>
</div>
```

### 第二種

```js
// Father
this.state={
    data:[1,2,3]
}
handleOnDelete(index){
    console.log(index)
}
<div>
    {
        this.state.data.map((item,index)=>(
        	<Child item={item} index={index} delete={this.handleOnDelete.bind(this)} key={index} />
        ))
    }
</div>

// Child
<div>
    <span onClick={(index)=>{this.props.delete(index)}}>{this.props.delete(index)}</span>
</div>
```



## REF

> ref是幫助我們直接獲取DOM元素的時候使用，但盡量不要去使用

> 不建議使用ref，react建議數據驅動的方式編寫代碼，盡量不要直接去操作DOM

### 方法一

```react
class App extends React.Component {
    state = {
    value: ''
  }
  handleSubmit = e => {
    e.preventDefault();
    this.setState({ value: this.textInput.value})
  };

  render() {
    return (
      <div>
        <h1>React Ref - Callback Ref</h1>
        <h3>Value: {this.state.value}</h3>
        <form onSubmit={this.handleSubmit}>
        	// callback ref
          <input type="text" ref={(input) => {this.textInput = input}} />
          <button>Submit</button>
        </form>
      </div>
    );
  }
}

ReactDOM.render(<App />, document.getElementById("root"));
```
### 方法二

```js
constructor(props){
	super(props)
	this.input=React.createRef()
}
render(){
	return (
    	<input ref={this.input}>
    )
}
```

### 獲取組件`REF`

```sql
class MyButton extends Component{
	constructor(props){
		super(props)
		this.buttonRef=React.createRef()
	}
	render(){
		return (
        	<button ref={this.buttonRef}>
            {props.children}
			</button>
        )
	}
}
class App extends Component{
	constructor(props){
		super(props)
		this.myRef=React.createRef()
	}
	componentDidComponent(){
		console.log(this.myRef.buttonRef)
	}
	render(){
		return (
        	<MyButton ref={this.myRef}>
            	Press here
            </MyButton>
        )
	}
}
```

```sql
import { createRef, forwardRef } from "react";

const MyButton = forwardRef((props, ref) => (
  <button ref={ref}>
    {props.children}
  </button>
));

class App extends Component {
  constructor(props){
    super(props);
    this.realButton = createRef();
  }
  componentDidComponent{
    //直接拿到 inner element ref
    console.log(this.realButton);
  }
  render(){
    return (
    <MyButton ref={this.realButton}>
      Press here
    </MyButton>
    );
  }
}

```



# 無狀態組件

當一個普通組件只有`render`時，完全可通過一個無狀態組件來替換掉普通的組件

1. 性能好
2. 無邏輯操作時

```react
const TodoListUI=(props)=>{
    return (
    	<div>{props.name}</div>
    )
}
```

## PureComponent

> 當持續傳入的`props`沒有變化時，不會重新render

```js
import React,{Component,PureComponent} from 'react'

class Temp extends PureComponent{
    render(){
        console.log("render Temp")
        return (
        	<div>{this.props.val}</div>
        )
    }
}
```



# React.Fragment

>   可做不可見的包裹元素

```react
<React.Fragment>
    <p>1</p>
	<p>2</p>
</React.Fragment>
```

```react
<p>1</p>
<p>2</p>
```

# PropTypes 驗證

```js
improt PropTypes from 'prop-types'

TodoItem.propTypes={
    test:PropTypes.string.isRequired,
    content:PropTypes.string,
    item:PropTypes.oneOfType([PropTypes.number,PropTypes.string]),
}
TodoItem.defaultProps={
    test:"Hello World"
}
```

# `CONTEXT API`上下文

## 第一步創建context

```js
const myContext=React.createContext()
```

## 第二步 創建 Provider Component

```js
class MyProvider extends Component{
  state={
    name:"Chen"
  }
  render(){
    return (
      // context有Provider屬性，用來提共數據
      <myContext.Provider value={{state:this.state}}>
        {this.props.children}
      </myContext.Provider>
    )
  }
}
```

## 第三步

```js
const Family=(props)=>{
  return (
    <div>
      <h1>Family</h1>
      <Person />
    </div>
  )
}

class Person extends Component {
  render(){
    return (
      <div>
        <h1>Person</h1>
        // 使用context提共的屬性Consumer
        <myContext.Consumer>
        {({state})=><p>My name is {state.name}</p>}
        </myContext.Consumer>
      </div>
    )
  }
}
class App extends Component {
  render() {
    return (
      <div className="App">
        <MyProvider>
          <Family />
        </MyProvider>
      </div>
    );
  }
}
```



# 高階組件

1. 第一個參數是組件
2. 返回值也是組件

## 範例1

```js
const PropsLogger=(WrapperComponent)=>{
    return class extends Component {
		render(){
			return <WrapperComponent {...this.props} />
		}
	}
}

const Hello=PropsLogger((props)=>{
    return (
		<div>Hello {props.name}</div>
	)
})

class App extends Component {
	render(){
		return (
			<div>
            	<Hello name="World" />
            </div>
		)
	}
}
```

## 範例2

```js
// /hoc/widthFetch
import React,{Component} from 'react'

const widthFetch=(url)=>(View)=>{
    return class extends Component{
        constructor(){
            super()
            this.state={
                loading:true,
                data:null
            }
        }
        componentDidMount(){
            fetch(url)
            .then((res)=>res.json())
            .then((data)=>{
                this.setState({
                    loading:false,
                    data:data
                })
            })
        }
        render(){
            if(this.state.loading){
                return (
                    <div>loding</div>
                )
            }else{
                return (
                    <View data={this.state.data} />
                )
            }
        }
    }
}
export default withFetch
```

```js
// ./components/Joke

import React from 'react'
import withFetch from '../hoc/withFetch'

const Joke=withFetch('https://api.icndb.com/jokes/random/3')(props=>{
    return (
        <div>
        {
            props.data.value.map((joke=>(
                <p key={joke.id}>{joke.joke}</p>
            )))
        }
     )
})

export default Joke;
```

```js
// App.js

<Joke />
```

## 範例3 `this.props.render()`

```js
class WithMouse extends Component {
    state={x:0,y:0}
    handleMouseMove=(event)=>{
      this.setState({
        x:event.clientX,
        y:event.clientY
      })
    }
    render(){
        return (
        	<div>{this.props.render(this.state)}</div>
        )
    }
}

const Mouse=()=>{
    return(
    	<WithMouse render={(props)=> <div>The mouse position is {props.x,props.y}</div> } />
    )
}
```



# 生命週期

> 生命週期函數只在某一個函數組件會自動執行的函數

## Mounting

### componentWillMount

> 在組件即將被掛載到頁面時候自動執行

### componentDidMount

> 組件被掛載到頁面之後，自動被執行一次，之後就不會再重新被重複執行

> ajax請求使用此生命週期

## Updation

### shouldComponentUpdate(nextProps,nextState)

> 在組件準備更新之前調用，可以控制組件是否進行更新，返回 true 時組件更新，返回 false 組件不更新。

> return true 會執行更新，return false不會執行更新

```react
  shouldComponentUpdate(nextProps,nextState){
      if(nextState.Number == this.state.Number){
        return false
      }
  }
```



#### nextProps

> 接下來props會變化成怎樣

#### nextState

> 接下來state會變化成怎樣

### componentWillUpdate

> 組件被更新之前，它會自動執行，但是他在`shouldComponentUpdate`之後被執行，如果`shouldComponentUpdate`返回`true`才執行，如果返回`false`不會執行更新

### componentDidUpdate

> 在更新發生之後立即被調用。這個生命週期在組件第一次渲染時不會觸發。

### componentWillReceiveProps

> 當一個組件從父組件接收了參數。
>
> 只要父組件的render函數被重新執行了，子組件這個生命週期函數就會被執行。



> 如果這組件第一次存在於父組件中，不會執行。
>
> 如果這個組件之前已經存在於父組件中，才會執行。

```js
componentWillReceiveProps(nextProps){
    this.setState({
        _id:nextProps.booklist._id,
        id:nextProps.booklist.id,
        title:nextProps.booklist.title,
        author:nextProps.booklist.author
    })
}
```

## Unmounting

### componentWillUnmount

>   在組件即將被卸載或銷毀時進行調用。
>
>   此生命週期是**取消網絡請求**、移除**監聽事件**、**清理 DOM 元素**、**清理定時器**等操作的好時機

## 注意

>**注意：** componentWillMount()、componentWillUpdate()、componentWillReceiveProps() 即將被廢棄，請不要再在組件中進行使用。因此本文不做講解，避免混淆。



# 新的生命週期

## static getDerivedStateFromProps(nextProps, prevState)

1.  用來取代`componentWillReceiveProps`
2.  不可以直接訪問`this.state`&`this.props`
3.  兩個參數`nextProps`&`prevState`

> 在每次調用 render 方法之前調用。包括初始化和後續更新時。

```js
state = {
    // 从 props 获取默认 state
    result: this.getResult(this.props.value)
}
componentWillReceiveProps(nextProps) {
    // 常用范式
    if (nextProps.value !== this.props.value) {
        this.setState({
            // 更新 state
            result: this.getResult(nextProps.value)
        })
    }
}
```

```js
state = {
    result: 0,
    // 必须存储 props.value 到 state 的副本，以便 getDerivedStateFromProps 取到 
    value: 0
}
// 新的方法，接收 nextProps 和 prevState
static getDerivedStateFromProps(nextProps, prevState) {
    // prevState.value 相当于当前组件的 this.props.value，是存在 state 的副本
    if (prevState.value !== nextProps.value) {
        // 返回新的state（只需返回更新的部分，与 `setState` 相同）
        return {
            // 相当于上面的 getResult，但只有一处
            result: nextProps.value * nextProps.value,
            // 又一次保存副本
            value: nextProps.value
        }
    }
    // 返回 null 表示不更新，此函数最后一定需要返回值
    return null
}

```

## getSnapshotBeforeUpdate

1.  用來取代`componentWillUpdate`
2.  接收兩個參數`prevProps`&`prevState`
3.  返回值會傳給`componentDidUpdate`的第三個參數`snapshot`

> 在最近一次的渲染輸出被提交之前調用。也就是說，在 render 之後，即將對組件進行掛載時調用。

```react
// list` 條目增加，渲染的 `li` 也增加。但是滾動條位置不變
export default class List extends React.Component {
    listRef = null
    // 新的生命週期
    getSnapshotBeforeUpdate(prevProps, prevState) {
        // 如果 `props.list` 增加，將原來的 scrollHeight 存入 listRef
        if (prevProps.list.length < this.props.list.length) {
            return this.listRef.scrollHeight
        }
        return null
    } 
    // snapshot 就是 `getSnapshotBeforeUpdate` 的返回值
    componentDidUpdate(prevProps, prevState, snapshot) {
        if (snapshot !== null) {
            // scrollTop 增加新的 scrollHeight 和原來 scrollHeight 的差值，以保持滾動條位置不變
            this.listRef.scrollTop += this.listRef.scrollHeight - snapshot
        }
    } 
    
    setListRef = ref => (this.listRef = ref)

    render() {
        return (
            <ul ref={this.setListRef} style={{ height: 200, overflowY: 'scroll' }}>
                {this.props.list.map((n, i) => (
                    <li key={i}>{n}</li>
                ))}
            </ul>
        )
    }
}
```

# 生命週期執行順序

## 掛載時

* constructor()
* static getDerivedStateFromProps()
* render()
* componentDidMount()

## 更新時

* static getDerivedStateFromProps()
* shouldComponentUpdate()
* render()
* getSnapshotBeforeUpdate()
* componentDidUpdate()

## 卸載時

* componentWillUnmount()

# 生命週期中是否可調用 setState()

## 初始化 state

* constructor()

## 可以調用 setState()

* componentDidMount()

## 根據判斷條件可以調用 setState()

* componentDidUpdate()

## 禁止調用 setState()

- shouldComponentUpdate()
- render()
- getSnapshotBeforeUpdate()
- componentWillUnmount()

# 從五種組件狀態改變的時機來探究生命週期的執行順序

## 一、父子組件初始化

* Parent 組件： constructor()
* Parent 組件： getDerivedStateFromProps()
* Parent 組件： render()
* Child 組件： constructor()
* Child 組件： getDerivedStateFromProps()
* Child 組件： render()
* Child 組件： componentDidMount()
* Parent 組件： componentDidMount()

## 二、修改子組件自身狀態 state 時

> 點擊子組件中的 *改變自身狀態* 按鈕，則界面上 *自身狀態 counter：* 的值會 + 1，控制台中的 log 打印順序為：

* Child 組件： getDerivedStateFromProps()
* Child 組件： shouldComponentUpdate()
* Child 組件： render()
* Child 組件： getSnapshotBeforeUpdate()
* Child 組件： componentDidUpdate()

## 三、修改父組件中傳入子組件的 props 時

> 點擊父組件中的 *改變傳給子組件的屬性 num* 按鈕，則界面上 *父組件傳過來的屬性 num：* 的值會 + 1，控制台中的 log 打印順序為：

* Parent 組件： getDerivedStateFromProps()
* Parent 組件： shouldComponentUpdate()
* Parent 組件： render()
* Child 組件： getDerivedStateFromProps()
* Child 組件： shouldComponentUpdate()
* Child 組件： render()
* Child 組件： getSnapshotBeforeUpdate()
* Parent 組件： getSnapshotBeforeUpdate()
* Child 組件： componentDidUpdate()
* Parent 組件： componentDidUpdate()

## 四、卸載子組件

> 點擊父組件中的 *卸載 / 掛載子組件* 按鈕，則界面上子組件會消失，控制台中的 log 打印順序為

- Parent 組件： getDerivedStateFromProps()
- Parent 組件： shouldComponentUpdate()
- Parent 組件： render()
- Parent 組件： getSnapshotBeforeUpdate()
- Child 組件： componentWillUnmount()
- Parent 組件： componentDidUpdate()

## 五、重新掛載子組件

> 再次點擊父組件中的 *卸載 / 掛載子組件* 按鈕，則界面上子組件會重新渲染出來，控制台中的 log 打印順序為：

* Parent 組件： getDerivedStateFromProps()
* Parent 組件： shouldComponentUpdate()
* Parent 組件： render()
* Child 組件： constructor()
* Child 組件： getDerivedStateFromProps()
* Child 組件： render()
* Parent 組件： getSnapshotBeforeUpdate()
* Child 組件： componentDidMount()
* Parent 組件： componentDidUpdate()

# 父子組件生命週期執行順序總結

* 當子組件自身狀態改變時，不會對父組件產生副作用的情況下，父組件不會進行更新，即不會觸發父組件的生命週期
* 當父組件中狀態發生變化（包括子組件的掛載以及）時，會觸發自身對應的生命週期以及子組件的更新
  * render 以及 render 之前的生命週期，則 父組件 先執行
  * render 以及 render 之後的聲明週期，則子組件先執行，並且是與父組件交替執行
* 當子組件進行卸載時，只會執行自身的 componentWillUnmount 生命週期，不會再觸發別的生命週期

# Redux

Action Creaters-=> Store => Reducers

Store <= Reducers

Store => React Componets

React Components => Acton Creaters

### 創建store

1.  建立`store資料夾`，創建`index.js`，並創建`reducers.js`

```js
// App.js
import store from './store/index'
```

```js
// /store/index.js
import { createStore,applyMiddleware} from 'redux'
import thunk from 'redux-thunk'
import reducers from './reducers.js'

const store=createStore(
    reducers,
    applyMiddleware(thunk)
)

export default store
```

```js
// /store/reducers
const defaultState={
    inputValue:"",
    list:[]
}

export default (state=defaultState,action)=>{
    return state
}
```

### 使用store

1.  使用`store.getState()`方法獲取state
2.  提供`store.dispatch(action)`更新state
3.  `store.subscribe(listener)`來註冊、取消監聽

```js
// App.js

constructor(props){
    super(props)
    // store.getState()，可以獲取store的資料
    this.state=store.getState();
    
    // 訂閱store，每次store變更時會調動handleStorageChange
    store.subscribe(this.handleStorageChange.bind(this))
}
handleInputChange(e){
    const action={
        type:"change_input_value",
        value:e.target.value
    }
    // dispatch action
    store.dispatch(action)
}
handleStorageChange(){
    this.setState(store.getState())
}
```

```js
// ./store/reducers.js

// reducer 可以接收state，但是絕不能修改state
export default (state=defaultState,action)=>{
    if(action.type === "change_input_value"){
        // 使用深拷貝
        const newState=JSON.parse(JOSN.stringify(state))
        newState.inputValue=action.value
        return newState
    }
    return state
}
```



#### reducer 可以接收state，但是絕不能修改state

### 創建actionCreator,actionTypes

```js
// 創建 actionTypes.js
export const SEARCH_FOCUS="search_focus"
```

```js
// 創建 actionCreator
import {SEARCH_FOCUS} from './store/actionTypes'
// 必須返回對象
export const searchFocus=()=>({
    type:SEARCH_FOCUS
})
```
```js
// './store/reducer'
import {SEARCH_FOCUS} from './store/actionTypes'
export default (state=defaultState,action)=>{
    if(actio.type === SEARCH_FOCUS){
        
    }
    return state
}
```



```js
// App.js
import {searchFocus} from './store/actionCreator'
handleSearchFocus(){
    store.dispatch(searchFocus())
}
```

### 多個``reducer`管理

1.  創建根`reducer`用於將其他不同業務的`reducer`合併

```js
import {combineReducers} from 'redux'

import {user} from './user'

export default combineReducers({
    user
})
```



### react-redex使用

```
npm i -S react-redex
```

```js
// App.js
import {Provider} from 'react-redux'
import store from './store/index.js'

// Provider下的組件都可以使用store
<Provider store={store}>
    <Header>
</provider>
```

```js
// Header
import {connect} from 'react-redux'
import {actionhandleInputBlur} from './store/actionCreator'

const Header=(props)=>{
    return (
    	<div>
        	{props.name}
        	<input onClick={props.handleInputBlur}>
        </div>
    )
}
const mapStateToProps=(state)=>{
    return {
        name:header.state.name
	}
}
const mapDispatchToProps=(dispatch)=>{
    return {
        handleInputBlur(){
            const action={
                type:"search_blur"
            }
            dispatch(actionhandleInputBlur)
        }
    }
}
export default connect(mapStateToProps,mapDispatchToProps)(Header)
```

### `mapStateToProps`

```js
const mapStateToProps=(state)=>{
    return {
        name:header.state.name
	}
}
```



### `mapDispatch`用法

```js
import {handleInputBlur} from './store/actionCreator'

const Header=(props)=>{
    return (
    	<div>
        	{props.name}
        	<input onClick={props.actionhandleInputBlur}>
        </div>
    )
}

export default connect(mapStateToProps,{handleInputBlur})(Header)
```

### reducer拆分使用

```js
// 建立分支 header/store/reudcer.js
// 主要 store/reducer.js
import {combinReducer} from 'redux'
import headerReducer from 'header/store/reudcer.js'
export default combinReducer({
    header:headerReducer
})

```

## 異步場景下更新``store`

### redux-thunk

```js
npm i redux-thunk
```

```js
import thunk from 'redux-thunk';
const store = createStore(
    reducer,
    composeWithDevTools(applyMiddleware(thunk))
)
```

```js
export const actionGetUser=()=>{
    return (dispatch)=>{
        return fetch("https://randomuser.me/api/")
            .then((res)=>{
                return res.json()
            })
            .then((data)=>{
                console.log(data.results)
            })
    }
}
```



## react-router-dom

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

### `Link`

1. 一般用法

```js
<Link to="/home">Home</Link>
```

2. 增強用法

state:可用於傳數據，又不出現在網址欄

```js
// http://localhost:3000/pro?a=b#the-hash
console.log(props.lcation.state)

<Link to={{
 pathname:"/pro",
 search:"?a=b",
 hash:"#the-hash"
 state:{fromDashboard:true}
}}>pro</Link>
```



### `NavLnk`

activeClassName（string）：設置選中樣式，默認值為 active；

activeStyle（object）：當元素被選中時, 為此元素添加樣式；

exact（bool）：為 true 時, 只有當地址完全匹配 class 和 style 才會應用；

strict（bool）：為 true 時，在確定位置是否與當前 URL 匹配時，將考慮位置 pathname 後的斜線；

isActive（func）：判斷鏈接是否激活的額外邏輯的功能；

```js
<NavLnk
	exact
    to="/home"
>
Home    
</NavLnk>
```



### `Switch`

```js
// 常用於包覆 <Route />
<Switch>
    ...
	...
	<Route /> 
</Switch>
```

### `404`

```js
// 建立一個 404 component
// 在 Route 最後一個 放置
import NoMath from './Error'
<Switch>
    ...
	...
	<Route component={NoMath} /> 
</Switch>
```

### `render func`

可以對於`Route`進行公能力增強

```js
<Route path="/home" render={()=> <div>Home</div>} />
                            
<Route path="/home" render={(props)=> <Home {...props} />} />
```

### `URL parameters`

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

### `query string`

1.可使用`URLSearchParams`去做分析

```js
// http://localhost:3000/users/weq?name=123

//
<Route path="/users/:name"  component={User}/>

// User.js
const User=(props)=>{
  const params=new URLSearchParams(props.location.search)
  console.log(params.get('name'))
  return (
    <div>User {props.match.params.name}</div>
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

### `redirect`路由導向

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

### `history push`跳轉頁面

可綁定事件跳轉頁面

```js
handleClick(){
    this.props.history.push('/')
}
```

### `withRouter`

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





## react-loadable(異步組件)

```js
npm i -S react-loadable
```

```js
// detail/loadable.js
import Loadable from 'react-loadable';

const LoadableComponent = Loadable({
    // 加載哪個組件
    loader: () => import('./index'),
    // 臨時顯示的組件
    loading(){
        return <div>正在加載</div>
    },
});

export default ()=><LoadableComponent/>;

// App
import Detail from './pages/detail/loadable'
```

### param失效

```js
import {withRouter} from 'react-router-dom'
export default connect(mapState,mapDispatch)(withRouter(Detail));
```

## React使用SCSS

### 開以webpack config功能

```js
npm run eject

npm install sass-loader node-sass --save-dev
```

### **Edit Webpack Config**

**config/webpack.config.dev.js**

```js
{
  test: /\.css$/,
  .....
  .... 
}
```

```js
{
  test: /\.scss$/,
  .....
  {
      loader: require.resolve('sass-loader'),
  },
  .... 
}
```

## 好用的插件

### redux-logger

```js
npm i -S redux-logger
import logger form 'redux-logger'
import rootReducer from './reducer'
const store=createStore(
	rootReducer,
    applyMiddleware(logger)
)
```

### moment

```js
npm i -S moment

{moment(new Date(action.date)).fromNow()}
```

### sfcookies

```js
npm i -S sfcookies
import {bake_cookie,read_cookie} from 'sfcookies'

const reminders=(state=read_cookie("reminders") || [],action={})=>{
    let reminders=null;
    switch(action.type){
        case ADD_REMINDER:
            reminders=[...state,reminder(action)]
            bake_cookie("reminders",reminders)
            return reminders;
        case DELETE_REMINDER:
            reminders=state.filter((reminder)=> reminder.id !== action.id)
            bake_cookie("reminders",reminders)
            return reminders;
        case CLEAR_REMINDERS:
            reminders=[]
            bake_cookie("reminders",reminders)
            return reminders;
        default: return state;
    }
}

```

## 捕捉錯誤`componentDidCatch(error,errorInfo)`

1. 會把錯誤捕捉到，把對的顯示出來
2. 並在發布的環境中，可以把把錯誤捕捉到放在`console.log`，把對的顯示在畫面中

```js
// ./component/ErrorBoundary

class ErrorBoundary extends React.Component{
    state={
        hasError:false,
        error:null,
        errorInfo:null
    }
    componentDidCatch(error,errorInfo){
        this.setState({
            hasError:true,
            error:error,
            errorInfo:errorInfo
        })
    }
    render(){
		if(this.state.hasError){
            return <div>{this.props.render(this.state.error,this.state.errorInfo)}</div>
        }else{
            return this.props.children
        }
    }
}
```

```js
// ./App.js
import Broken from './component/Broken'

<ErrorBoundary render={(error,errorInfo)=><p>error.toString()</p>}>
    <Broken />
</ErrorBoundary>
```

## redux-saga

```js
npm i --save redux-saga
```

```js
// index.js

import {createStore,applyMiddleware} from 'redux'

// 引入saga
import createSagaMiddleware from 'redux-saga'
import {composeWithDevTools} from 'redux-devtools-extension'

// 使用saga middleware
const sagaMiddleware=createSagaMiddleware()

// 導入saga目錄下js
import {helloSaga} from './saga'

const store=createStore(
    rootReducer,
    composeWithDevTools(
        applyMiddleware(sagaMiddleware)
    )
)

// 運行saga
sagaMiddleware.run(helloSaga)

```
### saga使用generator方式
#### 用法1

```js
function* gen(){
    yield "js";
    yield "css";
    yield "html";
    return "done";
}

var myGen=gen();
myGen.next(); // js { value:"js",done:false }
myGen.next(); // css { value:"css",done:false }
myGen.next(); // html { value:"html",done:false }
myGen.next(); // html { value:"done",done:true }
```
#### 用法2

```js
function* gen(){
    var x=yield "js";
    var y=yield "css";
    var z=yield "html";
    return x+y+z;
}
var myGen=gen();
console.log(myGen.next());
console.log(myGen.next(1)); // 會取到上一個返回值內容，然後會把 1 當成x
console.log(myGen.next(2));
console.log(myGen.next(3)); // 6
```
#### 用法3

```js
function* gen(){
  var posts=yield $.getJSON("https://jsonplaceholder.typicode.com/posts")
  console.log(posts[0].title)
  var users=yield $.getJSON("https://jsonplaceholder.typicode.com/users")
  console.log(users[0].email)
}

function run(generator){
  var myGen=generator();
  function handle(yielded){
    if(!yielded.done){
      yielded.value.then((data)=>{
        return handle(myGen.next(data))
      })
    }
  }
  return handle(myGen.next())
}
```



### 創建saga用的js

#### basic1

```js
// ./saga/index.js

// saga使用generator方式
function* hellosaga(){
    console.log("hellosaga")
}

```

#### basic2

`takeEvery()`監聽且不中斷的觸發

`takeLatest()`監聽，指運行一次，如果有其他也同時在運行時，只運行最後一次觸發

`put()`可發送action

`call()`當要調用promise的地方，使用call()，會有更好的測試效果

```js
import {delay} from 'redux-saga'
import {takeEvery,put} from 'redux-saga/effects'
import {INCREMENT_ASYNC} from '../constans'
import {increment} from '../actions/counter'

function* incrementAsync(){
    yield delay(2000)
    yield put({type:INCREMENT_ASYNC})
}

export function* watchIncrementAsync(){
    // 監聽 INCREMENT_ASYNC action方法去觸發 incrementAsync函數
    yield takeEvery(INCREMENT_ASYNC,incrementAsync)
}
```

#### basic3:`call()`ajax請求

```js
import {call} from 'redux-saga/effects'
import axios from 'axios'

function* fetchUser(){
    const user=yield call(axios.get,"https://jsonplaceholder.typicode.com/posts")
    console.log(user)
}

export function* watchFetchUser(){
    yield takeEvery("FETCH_USER_REQUEST",fetchUser)
}
```

#### basic4:`all()`同時執行多個saga

```js
import {delay} from 'redux-saga'
import {takeEvery,takeLatest,put,call,all} from 'redux-saga/effects'
import {INCREMENT_ASYNC,INCREMENT} from '../constans'
import axios from 'axios'

function* incrementAsync(){
    yield delay(2000)
    yield put({type:INCREMENT})
}

function* watchIncrementAsync(){
    // 監聽 INCREMENT_ASYNC action方法去觸發 incrementAsync函數
    yield takeLatest(INCREMENT_ASYNC,incrementAsync)
}

function* fetchUser(){
    const user=yield call(axios.get,"https://jsonplaceholder.typicode.com/posts")
    console.log(user)
}

function* watchFetchUser(){
    yield takeEvery("FETCH_USER_REQUEST",fetchUser)
}

export default function* rootSaga(){
    yield all([
        watchIncrementAsync(),
        watchFetchUser()
    ])
}
```

#### basic5:`fork`

併發執行時使用

```js
import {all,fork} from 'redux-saga/effects'
import * as counterSagas from './counter'
import * as userSagas from './user'

export default function* rootSaga(){
    yield all([
        ...Object.values(userSagas),
        ...Object.values(counterSagas)
    ].map(fork))
}
```

#### basic6:axios && catch

```js
// ./redcuers/user.js
const initalState={
    isFetching:false,
    error:null,
    user:null
}

const user=(state=initalState,action)=>{
    switch(action.type){
        case "FETCH_USER_REQUEST":
            return {
                isFetching:true,
                error:null,
                user:null
            };
        case "FETCH_USER_SUCCESSED":
            return {
                isFetching:false,
                error:null,
                user:action.user
            }
        case "FETCH_USER_FAILURE":
            return {
                isFetching:false,
                error:action.error,
                user:null
            }
        default: return state
    }
}

export default user
```

```js
// ./saga/user.js

import {takeEvery,call,put} from 'redux-saga/effects'
import axios from 'axios'

function* fetchUser(){
    try{
        const user=yield call(axios.get,"https://jsonplaceholder.typicode.com/users")
        yield put({type:"FETCH_USER_SUCCESSED",user})
    }catch(e){
        yield put({type:"FETCH_USER_FAILURE",error:e.message})
    }
}

function* watchFetchUser(){
    yield takeEvery("FETCH_USER_REQUEST",fetchUser)
}

export const userSagas=[
    watchFetchUser()
]
```

# React Hook

1.  useState()
2.  useEffect()
3.  useContext()

## useState

1.  **useState 什麼時候執行？** 它會在組件每次render的時候調用
2.  類似`this.state`與`this.setState`的合體

```react
import React,{useState} from 'react'

const Example=()=>{
    const [count,setCount]=useState(0);
    const increase=()=>{
        setCount(count+1);
    }
    const decrease=()=>{
        setCount(count-1);
    }
    return (
    	<div>
        	<button onClick={increase}>-</button>
        	<span>{count}</span>
        	<button onClick={decrease}>+</button>
        </div>
    )
}

```

```react
const SuperButton=(props)=>{
    const {onClick,children}=props
    const onclickHere=onClick;
    return <button onClick={onclickHere}>{children}</button>
}

const UseStateSample=()=>{
    const [count,setCount]=useState(0)

    const increased=()=>{
        setCount(count=>count+1)
    }
    const decreased=()=>{
        setCount(count-1)
    }
    return (
        <p>
            <SuperButton onClick={decreased}>-</SuperButton>
            <b>{count}</b>
            <SuperButton onClick={increased}>+</SuperButton>
        </p>
    )
}
```



## useContext

可以取代`Context.Consumer`使用

```react
import React,{useContext,useState} from 'react'

const CounterContext=React.createContext()

const CounterApp=()=>{
    const [count,setCount]=useState(1)
    const increase=()=>{
        setCount(count+1)
      }
      const decrease=()=>{
        setCount(count-1)
      }
    return (
    	<CounterContext.Provider value={{count,increase,decrease}}>
        	<Counter />
        </CounterContext.Provider>
    )
}

const Counter=()=>{
    const {count,increase,decrease}=useContext(CounterContext);
    return (
    	<div>
          <button onClick={decrease}>-</button>
          <span>{count}</span>
          <button onClick={increase}>+</button>
    	</div>
    )
}
```



```react
const MyContext=React.createContext();

const UseContextSample=()=>{
    const [count,setCount]=useState(0);
    const increment=()=>{
        setCount(count=>count+1)
    }
    const decrement=()=>{
        setCount(count=>count-1)
    }
    return (
        <div>
            <MyContext.Provider value={{increment,decrement,count}}>
                <IncrementButton/>
            </MyContext.Provider>
        </div>
    )
}

const IncrementButton=()=>{
    const {increment,decrement,count}=useContext(MyContext);

    return (
        <p>
            <h3>UseContextSample</h3>
            <button onClick={increment}>+</button>
            <b>{count}</b>
            <button onClick={decrement}>-</button>
        </p>
    )
}
```



## useEffect

`componentDidMount`和`componentWillUnmount`和`componentDidUpdate`綜合體

1.  **useEffect 什麼時候執行？** 它會在組件 mount 和 unmount 以及每次重新渲染的時候都會執行，也就是會在 componentDidMount、componentDidUpdate、componentWillUnmount 這三個時期執行。
2.  第一個參數為`componentDidMount`、`componentWillUnmount`
3.  第二個參數為`componentDidUpdate`
4.  第二個參數，若與前一次相等 effect 就不會執行，空陣列就意味著只會執行一次 ( 永遠相等 )

```js	
useEffect(
// componentDidMount
    
// componentWillUnmount
	return ;
,[])
```

```js
import { useState, useEffect } from 'react';

const App = () => {
  const [users, setUsers] = useState([]);
  
  useEffect(() => {
    // Connect to the Random User API using axios
    axios("https://randomuser.me/api/?results=10")
      // Once we get a response, fetch name, username, email and image data
      // and map them to defined variables we can use later.
      .then(response =>
        response.data.results.map(user => ({
          name: `{user.name.first} ${user.name.last}`,
          username: `{user.login.username}`,
          email: `{user.email}`,
          image: `{user.picture.thumbnail}`
        }))
      )
      // Finally, update the `setUsers` state with the fetched data
      // so it stores it for use on render
      .then(data => {
        setUsers(data);
      });
  }, []);
  
  // The UI to render
  return (
    <div className="users">
      {users.map(user => (
        <div key={user.username} className="users__user">
          <img src={user.image} className="users__avatar" />
          <div className="users__meta">
            <h1>{user.name}</h1>
            <p>{user.email}</p>
          </div>
        </div>
      ))}
    </div>
  )
}
```

```js
const UseEffectSample=()=>{
    const [count,setCount]=useState(0);

    useEffect(()=>{
        const timerId=setTimeout(()=>{
            setCount(count=>count+1)
        },1000)
        return ()=>{
            return clearTimeout(timerId)
        }
    },[count])

    return (
        <p>
            time: <b>{count}</b>
        </p>
    )
}
```

## useRef

```react
const UseRefSample=()=>{
  const inputRef=useRef();
  const onButtonClick=()=>{
    console.log(displayAreaRef.current.value)
  }
  return (
    <div>
      <input ref={inputRef} type="text"/>
      <button onClick={onButtonClick}>GET INPUT</button>
    </div>
  )
}
```

```react
const UseRefSample=()=>{
  const displayAreaRef=useRef();
  useEffect(()=>{
    let rafid=null;
    const loop=()=>{
      const now=new Date();
      displayAreaRef.current.textContent=`
      ${String(now.getHours()).padStart(2,'0')}:
      ${String(now.getMinutes()).padStart(2,"0")}:
      ${String(now.getSeconds()).padStart(2,'0')}.${String(now.getMilliseconds()).padStart(3,'0')}
      `;
      rafid=requestAnimationFrame(loop)
    };
    loop();
    return ()=>cancelAnimationFrame(rafid);
  })
  
  return (
    <p ref={displayAreaRef} />
  )
}
```



## 支援IE11

### 第一步 React Polyfill

-   通過`npm install react-app-polyfill`安裝 polyfill
-   在入口文件（通常是`src/index.js`）的第一行引入該庫

```js
// 在文件的第一行導入，兼容 IE11
import 'react-app-polyfill/ie11';
```

### 第二步 core-js

-   通過`npm install core-js`命令進行安裝
-   在入口文件（通常是`src/index.js`）的第一行引入該庫

```js
// 在文件的第一行導入
import 'core-js';
```

