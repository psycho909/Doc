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



## ref

> ref是幫助我們直接獲取DOM元素的時候使用，但盡量不要去使用

> 不建議使用ref，react建議數據驅動的方式編寫代碼，盡量不要直接去操作DOM

### basic

```react
class App extends React.Component{
    constructor(){
        this.textInput=React.createRef();
        this.state={
            value:''
        }
    }
    handleSubmit=e=>{
        e.preventDefault()
        this.setState({
            value:this.textInput.current.value
        })
    }
    render(){
        return (
            <div>
                <h3>{this.state.value}</h3>
            	<form onSubmit={this.handleSubmit}>
                    <input type="text" ref={this.textInput}>
                    <button>Submit</button>
                </form>
            </div>
        )
    }
}
```

### ES6 Ref

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

```sql
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



## 無狀態組件

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



## React.Fragment

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

## PropTypes

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

## 生命週期

> 生命週期函數只在某一個函數組件會自動執行的函數

### Mounting

#### componentWillMount

> 在組件即將被掛載到頁面時候自動執行

#### componentDidMount

> 組件被掛載到頁面之後，自動被執行一次，之後就不會再重新被重複執行

> ajax請求使用此生命週期

### Updation

#### shouldComponentUpdate(nextProps,nextState)

> 組件被更新之前，它會自動被執行

> rteturn true 會執行更新，rteturn  false不會執行更新

##### nextProps

> 接下來props會變化成怎樣

##### nextState

> 接下來state會變化成怎樣

#### componentWillUpdate

> 組件被更新之前，它會自動執行，但是他在`shouldComponentUpdate`之後被執行，如果`shouldComponentUpdate`返回`true`才執行，如果返回`false`不會執行更新

#### componentDidUpdate

> 組件更新完成之後，他會被執行

### componentWillReceiveProps

> 當一個組件從父組件接收了參數。
>
> 只要父組件的render函數被重新執行了，子組件這個生命週期函數就會被執行。



> 如果這組件第一次存在於父組件中，不會執行。
>
> 如果這個組件之前已經存在於父組件中，才會執行。

### Unmounting

#### componentWillUnmount

> 當這個組件即將被從頁面中剔除的時候，會被執行



## Redux

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
// App.js
import {BrowserRouter,Route} from 'react-router-dom'
import Home from './pages/home/'
<div className="App">
  <Provider store={store}>
     <div>
        <Header/>
        <BrowserRouter>
            <div>
                <Route path="/" exact render={()=> <div>Home</div>}></Route>
                <Route path="/" exact  component={Home}></Route>
                <Route path="/detail" exact render={()=> <div>detail</div>}></Route>
            </div>
     	</BrowserRouter>
     </div>
   </Provider>
</div>
                                                    
// Header
import {Link} from 'react-router-dom'

<Link to="/detail">detail</Link>
<NavLink exact={true} to="/">首頁</NavLink>
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

