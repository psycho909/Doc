## state,setState 注意事項

### 當setState不同步時，正確獲取state的方法

>   react官網說明setState不一定同步，state為component封裝內部狀態屬性，當呼叫setState()就會重新繪製，所以由react自己透過diff進行控制state更新狀態。

```js
// setState()可以透過第二參數callback來取到更新後的值
this.setState({
    count:3
},callback)
```

### 當state依據props改變時

>   如果state會依據props改變，props與state的更新不一定會同步，所以使用setState()傳入的參數就不是要更新的state object，而會是一個傳入 **previous state** 和 **props** 的function，並且回傳要更新的state object。

```js
this.setState(
    (prevState,props)=>({
        count:prevState.count+1
    }),callback
)
```



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
### 方法二 (常用)

```react
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

### forwardRef使用方法

#### 如何将DOM 通过`Refs` 暴露给父组件

```react
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
##### 解決方法:使用`forwardRef`
在极少数情况下，我们可能希望在父组件中引用子节点的 DOM 节点（官方不建议这样操作，因为它会打破组件的封装），用户触发焦点或者测量子DOM 节点的大小或者位置。虽然我们可以通过向子组件添加 ref的方式来解决，但这并不是一个理想的解决方案，因为我们只能获取组件实例而不是 DOM节点。并且它还在函数组件上无效。

> Ref forwarding 是一种自动将ref 通过组件传递给其子节点的技术。下面我们通过具体的案例来演示一下效果。

```react
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

#### 高阶组件中的refs

```react
import React from 'react'

const HOCLogProps=(Comp)=>{
    class HOCLogProps extends React.Component{
        componentDidUpdate(prevProps){
            console.log('old props:', prevProps);
            console.log('new props:', this.props);
        }

        render(){
            const { forwardedRef, ...rest } = this.props;
            return <Comp ref={ forwardedRef } {...rest} />;
        }
    }
    return React.forwardRef((props, ref) => {
        return <HOCLogProps { ...props } forwardedRef={ ref } />;
    });
}

export default HOCLogProps
```

```react	
import React from 'react'
import HOCLogProps from './HOCLogProps'

const BtnComp=React.forwardRef((props,ref)=>{
    return (
        <div>
            <button ref={ref} className="btn">
                {props.children}
            </button>
        </div>
    )
})

export default HOCLogProps(BtnComp)
```

```react
import React, { Component } from 'react';
import BtnComp from './BtnComp'
class App extends Component {
  constructor(props){
    super(props)
    this.btnRef=React.createRef()
    this.state={
      value:"init"
    }
  }
  componentDidMount() {
    console.log('ref', this.btnRef);
    console.log('ref', this.btnRef.current.className);
    this.btnRef.current.classList.add('cancel'); // 给BtnComp中的button添加一个class
    this.btnRef.current.focus(); // focus到button元素上
    setTimeout(() => {
      this.setState({
        value: '更新'
      });
    }, 3000);
  }
  render() {
    return (
      <div className="App">
        <BtnComp ref={this.btnRef}>{this.state.value}</BtnComp>
      </div>
    );
  }
}

export default App;

```



# 無狀態組件

當一個普通組件只有`render`時，完全可通過一個無狀態組件來替換掉普通的組件

1. 性能好
2. 無邏輯操作時
3. 不能時候`生命週期`

```react
const TodoListUI=(props)=>{
    return (
    	<div>{props.name}</div>
    )
}
```

# PureComponent

1.  當持續傳入的`props`沒有變化時，不會重新render
2.  自動處理` shouldComponentUpdate()`
3.  現在有一個顯示時間的組件,每一秒都會重新渲染一次，對於`Child`組件我們肯定不希望也跟著渲染，所有需要用到`PureComponent`

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

# React.memo

1.  `React.memo()`是一個高階函數，它與 [`React.PureComponent`](https://reactjs.org/docs/react-api.html#reactpurecomponent)類似，但是一個函數組件而非一個類。
2.  `React.memo()`可接受2個參數，第一個參數為純函數的組件，第二個參數用於對比props控制是否刷新，與[`shouldComponentUpdate()`](https://reactjs.org/docs/react-component.html#shouldcomponentupdate)功能類似。
3.  現在有一個顯示時間的組件,每一秒都會重新渲染一次，對於`Child`組件我們肯定不希望也跟著渲染，所有需要用到`React.memo`

```react
function Child({seconds}){
    console.log('I am rendering');
    return (
        <div>I am update every {seconds} seconds</div>
    )
};

function areEqual(prevProps, nextProps) {
    if(prevProps.seconds===nextProps.seconds){
        return true
    }else {
        return false
    }

}
export default React.memo(Child,areEqual)
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

```react
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

```react
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

## 範例2 給HOC組件添加靜態displayName屬性

```react
import React,{Component} from 'react'

const getDisplayName=WrapperComponent=>(
	WrapperComponent.displayName || WrapperComponent.name || "WrapperComponent"
)
const InputHOC =(WrapperComponent) => {
    return class extends Component{
        static displayName=`InputHOC(${getDisplayName(WrapperComponent)})`
        state={
            value:""
        }
        handleChange=(e)=>{
            this.setState({
                value:e.target.value
            })
        }
        render(){
            const passProps={
                value:this.state.value,
                onChange:this.handleChange,
            }
            console.log(id)
            return (
                <div className="input-group">
                    <WrapperComponent {...passProps} />
                </div>
            )
        }
    }
}

export default InputHOC;
```

```react
import React,{Component} from 'react'
import InputHOC from './InputHOC'

class Input extends Component{
    render(){
        return (
            <input type="text" {...this.props} />
        )
    }
}

export default InputHOC(Input)
```

## 範例3 增加屬性

```react
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
                    <View {...this.state.data} />
                )
            }
        }
    }
}
export default withFetch
```

```react
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
        </div>
     )
})

export default Joke;
```

```js
// App.js

<Joke />
```

## 範例4 復用Input

```react
import React,{Component} from 'react'

const InputHOC = ({id,title}) =>(WrapperComponent) => {
    return class extends Component{
        state={
            value:""
        }
        handleChange=(e)=>{
            this.setState({
                value:e.target.value
            })
        }
        render(){
            const passProps={
                value:this.state.value,
                onChange:this.handleChange,
                id
            }
            console.log(id)
            return (
                <div className="input-group">
                    <label htmlFor={id}>{title}</label>
                    <WrapperComponent {...passProps} />
                </div>
            )
        }
    }
}

export default InputHOC;
```

```react
import React,{Component} from 'react'
import InputHOC from './InputHOC'

class Input extends Component{
    render(){
        return (
            <input type="text" {...this.props} />
        )
    }
}

export default InputHOC({id:"inputs",title:"InputLabel"})(Input)
```

```react
import React,{Component,Fragment} from 'react'
import Input from './Hight2/Input'

class App extends Component{
    render(){
        return (
            <Fragment>
                <Input />
            </Fragment>
        )
    }
}

export default App;
```



## 範例5 `this.props.render()`

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

## 注意事項

1.  ### 不要在render函數中使用高階組件

2.  ### 靜態方法需手動複製

3.  ### Ref不會被傳遞



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

```react
// App.js
import store from './store'

constructor(props){
    super(props)
    // store.getState()，可以獲取store的資料
    this.state=store.getState();
    
    // 方法1:訂閱store，每次store變更時會調動handleStorageChange
    store.subscribe(this.handleStorageChange)
    
    // 方法2:訂閱store，每次store變更時會調動
    store.subscribe(()=>{
    	this.setState(store.getState())
    });
}
handleInputChange=(e)=>{
    const action={
        type:"change_input_value",
        value:e.target.value
    }
    // dispatch action
    store.dispatch(action)
}
handleStorageChange=()=>{
    this.setState(store.getState())
}
```

```react
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
export const CHANGE_INPUT_VALUE="change_input_value"
```

```js
// 創建 actionCreator
import {CHANGE_INPUT_VALUE} from './store/actionTypes'
// 必須返回對象
export const ChangeInputValueAction=(value)=>({
    type:CHANGE_INPUT_VALUE,
    value
})
```
```js
// './store/reducer'
import {CHANGE_INPUT_VALUE} from './store/actionTypes'
export default (state=defaultState,action)=>{
    if(actio.type === CHANGE_INPUT_VALUE){
        const newState = JSON.parse(JSON.stringify(state))
        console.log(action.value)
        newState.inputValue = action.value
        console.log(newState.inputValue)
        return newState
    }
    return state
}
```

```js
// App.js
import {ChangeInputValueAction} from './store/actionCreator'
handleInputChange=(e)=>{
    const action=ChangeInputValueAction(e.target.value)
    store.dispatch(action)
}
```

### actionCreator

```react
export const setGames=(games)=>{
    return {
        type:SET_GAMES,
        games
    }
}

export const fetchGames=(id)=>{
    return (dispatch)=>{
        fetch('/api/games')
        .then((res)=>{
            return res.json()
        })
        .then((data)=>{
            dispatch(setGames(data.games))
        })
    }
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



## react-redex使用

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

# 好用的插件

## redux-logger

```js
npm i -S redux-logger
import logger form 'redux-logger'
import rootReducer from './reducer'
const store=createStore(
	rootReducer,
    applyMiddleware(logger)
)
```

## moment

```js
npm i -S moment

{moment(new Date(action.date)).fromNow()}
```

## sfcookies

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

# redux-saga

## 初始化

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
## saga使用generator方式
### 用法1

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
### 用法2

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
### 用法3

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



## 創建saga用的js

### basic1

```js
// ./saga/index.js

// saga使用generator方式
function* hellosaga(){
    console.log("hellosaga")
}

```

### basic2

`takeEvery()`監聽且不中斷的觸發

`takeLatest()`監聽，指運行一次，如果有其他也同時在運行時，只運行最後一次觸發

`put()`put函數是用來發送action的 effect，你可以簡單的**把它理解成為redux框架中的dispatch函數**，當put一個action後，reducer中就會計算新的state並返回，**注意：** **put 也是阻塞 effect**

`call()`可以理解為實行一個異步函數,是阻塞型的，只有運行完後面的函數，才會繼續往下；在這裡可以片面的理解為async中的await！但寫法直觀多了！

```js
import {delay} from 'redux-saga'
import {takeEvery,put} from 'redux-saga/effects'
import {INCREMENT_ASYNC} from '../constans'
import {increment} from '../actions/counter'

function* incrementAsync(){
    yield delay(2000) // middleware 拿到一個 yield 後的 Promise，暫停1s後再繼續執行
    yield put({type:INCREMENT_ASYNC}) // 告訴 middleware 發起一個 INCREMENT 的 action。
}

// watcher Saga: 在每個INCREMENT_ASYNC上生成一個新的incrementAsync任務
export function* watchIncrementAsync(){
    // 監聽 INCREMENT_ASYNC action方法去觸發 incrementAsync函數
    yield takeEvery(INCREMENT_ASYNC,incrementAsync)
}
```

```react
 import { all, put, takeEvery } from 'redux-saga/effects'
 const delay = (ms) => new Promise(res => setTimeout(res, ms))

 // worker Saga: 執行異步的 increment 任務
 export function* incrementAsync() {
   yield delay(1000) // middleware 拿到一個 yield 後的 Promise，暫停1s後再繼續執行
   yield put({ type: 'INCREMENT' })  // 告訴 middleware 發起一個 INCREMENT 的 action。
 }

 // watcher Saga: 在每個INCREMENT_ASYNC上生成一個新的incrementAsync任務
 export function* watchIncrementAsync() {
  yield takeEvery('INCREMENT_ASYNC', incrementAsync)
 }

// 啟動saga們
 export default function* rootSaga() {
  yield all([
    watchIncrementAsync(),
    helloSaga()
  ])
 }

```



### basic3:`call()`ajax請求

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

### basic4:`all()`同時執行多個saga

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

### basic5:`fork`

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

### basic6:axios && catch

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

# Create React App 3 絕對路徑設置

```react
import { Header } from '../Header'
import { HeaderContainer } from '../../containers/HeaderContainer'
import headerStore from '../../store/headerStore'

//-------------------------------------------------

import { Header } from 'components/Header'
import { HeaderContainer } from 'containers/HeaderContainer'
import headerStore from 'store/headerStore'
```

```js
// 在跟目錄下 創建 jsconfig.json

{
  "compilerOptions": {
    "baseUrl": "src"
  }
}
```



# `env` 環境變量設置

再項目跟目錄創建``.env`

```env
REACT_APP_SECRET_CODE=abcdef
```

```react
<div>{process.env.REACT_APP_SECRET_CODE}</div>
```

-   `.env`：默认。
-   `.env.local`：本地覆盖。**除 test 之外的所有环境都加载此文件**。
-   `.env.development`, `.env.test`, `.env.production`：设置特定环境。
-   `.env.development.local`, `.env.test.local`, `.env.production.local`：设置特定环境的本地覆盖。



# 錯誤處理

純粹接收錯誤把錯誤回傳給網頁，用於視覺錯誤處理

```react
state={
    hasError:false
}
static getDerivedStateFromError(error){
    return {
      hasError:true
    }
}
render(){
    if(this.state.hasError){
        return <h1>Error</h1>
    }
}
```

想把錯誤傳給後端做紀錄，或者發生錯誤時導向另外網址

```react
componentDidCatch(error,info){
    axios.post('/api/logger',{info})
  }
```

# Protal 傳送門

網頁有三個DIV，想把react分別分配給三個DIV

* `createPortal(somthing,for)`

```react
import React, {Component} from 'react'
import {createPortal} from 'react-dom'

class LessionModal extends Component {
    render() {
        return createPortal(
            <div>LessionModal</div>,
            document.getElementById("modal")
        )
    }
}

export default LessionModal
```



# 支援IE11

## 第一步 React Polyfill

-   通過`npm install react-app-polyfill`安裝 polyfill
-   在入口文件（通常是`src/index.js`）的第一行引入該庫

```js
// 在文件的第一行導入，兼容 IE11
import 'react-app-polyfill/ie11';
```

## 第二步 core-js

-   通過`npm install core-js`命令進行安裝
-   在入口文件（通常是`src/index.js`）的第一行引入該庫

```js
// 在文件的第一行導入
import 'core-js';
```

