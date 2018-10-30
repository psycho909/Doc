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



## Reduce

Action Creaters-=> Store => Reducers

Store <= Reducers

Store => React Componets

React Components => Acton Creaters

### 創建store

> 建立`store資料夾`，創建`index.js`，並創建`reducers.js`

```js
// App.js
import store from './store/index'
```

```js
// /store/index.js
import { createStore } from 'redux'
import reducers from './reducers.js'

const store=createStore(reducers)

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
            dispatch(action)
        }
    }
}
export default connect(mapStateToProps,mapDispatchToProps)(Header)
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
                                                    
// Home
import {Link} from 'react-router-dom'

<Link to="/detail"></Link>
```

`exact`完完全全跟路徑相等時，才顯示

`BowserRouter`代表著一個路由

`Route`代表著一個路由規則

`Link`替代`a`做頁面跳轉

### 動態路由

```js
// Home
<Link to="/detail/1"></Link>

// App.js
<Route path="/detail/:id" exact render={()=> <div>detail</div>}></Route>
                                        
// page獲取id
this.props.match.params.id
```

```js
// Home
<Link to="/detail?id="></Link>

// App.js
<Route path="/detail/" exact render={()=> <div>detail</div>}></Route>
                                        
// page獲取id
this.props.location.search
// ?id=1
```

### 路由導向

```js
import {Redirect} from 'react-router-dom'

<Redirect to="/" />
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

