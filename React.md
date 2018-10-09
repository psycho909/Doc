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