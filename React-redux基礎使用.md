#react #js #redux
## 目錄結構

```
src
	actions
	-- constants.js
	-- index.js
	reducers
	-- index.js
	store
	-- index.js
	TodoList.js
	index.js
```

## Step0 安裝

```npm
npm i redux react-redux thunk
```

## Step1 創建store

```js
// applyMiddleware 處理中間鍵
import {createStore,applyMiddleware} from 'redux';
// thunk 可異步處理，如:ajax後dispatch
import thunk from 'redux-thunk'
// 狀態處理
import reducers from '../reducers';

const store=createStore(reducers,applyMiddleware(thunk));

export default store;
```

## Step2 創建reducers

```js
import {CHANGE_INPUT_VALUE,DELETE_LIST_ITEM,ADD_LIST_ITEM,GET_FETCH_USER} from '../actions/constants';

const defaultState ={
    inputValue:'',
    list:[]
}
export default (state=defaultState,action)=>{
    if( action.type === CHANGE_INPUT_VALUE) {
        const newState = JSON.parse(JSON.stringify(state))
        console.log(action.value)
        newState.inputValue = action.value
        console.log(newState.inputValue)
        return newState
    }
    if(action.type === ADD_LIST_ITEM){
        const newState = JSON.parse(JSON.stringify(state))
        newState.list.push(newState.inputValue)
        newState.inputValue='';
        return newState
    }
    if(action.type === DELETE_LIST_ITEM){
        const newState = JSON.parse(JSON.stringify(state))
        newState.splice(action.index,1)
        return newState
    }
    if(action.type === GET_FETCH_USER){
        const newState = JSON.parse(JSON.stringify(state))
        newState.list.push(action.data.email)
        console.log(action.data)
        return newState
    }
    return state
}
```

## Step3 創建constants

```js
export const CHANGE_INPUT_VALUE ='change_input_value'
export const DELETE_LIST_ITEM ='delete_list_item'
export const ADD_LIST_ITEM ='add_list_item'
export const GET_FETCH_USER="get_fetch_user"
```

## Step4 創建actions

```js
import {CHANGE_INPUT_VALUE,DELETE_LIST_ITEM,ADD_LIST_ITEM,GET_FETCH_USER} from './constants';

export const ChangeInputValueAction=(value)=>({
    type:CHANGE_INPUT_VALUE,
    value
})

export const AddListItemAction=()=>({
    type:ADD_LIST_ITEM
})

export const DeleteListItemAction=(index)=>({
    type:DELETE_LIST_ITEM,
    index
})

// 處理異步程序時可以使用
export const FetchUser=()=>{
    return (dispatch)=>{
        fetch("https://randomuser.me/api/")
        .then(res=>{
            return res.json()
        })
        .then(data=>{
            dispatch(GetFetchUser(data.results[0]))
        })
    }
}

export const GetFetchUser=(data)=>{
    return {
        type:GET_FETCH_USER,
        data
    }
}
```

## Step5 放入index.js

```js
...
import store from './store';
import {Provider} from 'react-redux'

ReactDOM.render(
    <Provider store={store}>
        <App />
    </Provider>
, 
document.getElementById('root'));
```

## Step6 開始使用

```js
import React,{Component} from 'react';
import {connect} from 'react-redux'
import 'antd/dist/antd.css';
import {Input,Button,List} from 'antd';
import {ChangeInputValueAction,AddListItemAction,DeleteListItemAction,FetchUser} from './actions'
```

```jsx
class TodoList extends Component{
    constructor(props){
        super(props);
        console.log(this.props.list)
    }
    handleInputChange=(e)=>{
        this.props.ChangeInputValueAction(e.target.value)
    }
    handleItemDelete=(index)=>{
        this.props.DeleteListItemAction(index)
    }
    handleBtnClick=()=>{
        this.props.AddListItemAction();
    }
    render(){
        return (
            <div>
                <Button onClick={this.props.FetchUser}>測試</Button>
                <Input
                    style= {{ width:'300px',marginLeft:'50px',marginTop:'50px'}} 
                    value={ this.props.inputValue}
                    onChange ={ this.handleInputChange}
                />
                <Button
                    style= {{ marginLeft:'15px'}}
                    onClick = { this.handleBtnClick }
                >提交</Button>
                <List 
                    style={{
                        width:"300px",
                        marginLeft:"50px",
                        marginTop:"20px",
                        backgroundColor:"#ccc"
                    }} 
                    bordered 
                    dataSource={this.props.list} 
                renderItem={(item,index)=>(<List.Item onClick={()=>this.handleItemDelete(index)}>{item}</List.Item>)} 
                />
            </div>
        )
    }
}
```

```js
const mapState =state=>{
    return {
        inputValue:state.inputValue,
        list:state.list
    }
}
const mapDispatch = {
    ChangeInputValueAction,AddListItemAction,DeleteListItemAction,FetchUser
}
export default connect(mapState,mapDispatch)(TodoList)
```

## 使用Hook

## Step1 `useSelector`和`useDispatch`

使用`useSelector`和`useDispatch`在`react-redux v7.1.0`版本開始有

可以不再使用`connect()`和`mapStateProps`及`mapDispatchProps`

```js
import React, {Component} from 'react'
import {useSelector, useDispatch} from 'react-redux'
import 'antd/dist/antd.css'
import {Input, Button, List} from 'antd'
import {
    ChangeInputValueAction,
    AddListItemAction,
    DeleteListItemAction,
    FetchUser
} from './actions'
```

## Step2 `useSelector`

> `useSelector`取代了`mapStateProps`

```js
const mapState =state=>{
    return {
        inputValue:state.inputValue,
        list:state.list
    }
}
```

========== to ==============

```js
const {inputValue, list} = useSelector(state => state)
```

## Step3 `useDispatch`

> `useDispatch`取代了`mapDispatchProps`

```js
const mapDispatch = {
    ChangeInputValueAction,AddListItemAction,DeleteListItemAction,FetchUser
}
```

========== to ==============

```js
const dispatch = useDispatch()

const handleInputChange = e => {
    dispatch(ChangeInputValueAction(e.target.value))
}
```



```jsx
const TodoList = () => {
    const {inputValue, list} = useSelector(state => state)
    const dispatch = useDispatch()

    const handleInputChange = e => {
        dispatch(ChangeInputValueAction(e.target.value))
    }
    const handleItemDelete = index => {
        dispatch(DeleteListItemAction(index))
    }
    const handleBtnClick = () => {
        dispatch(AddListItemAction())
    }
    return (
        <div>
            <Input
                style={{width: '300px', marginLeft: '50px', marginTop: '50px'}}
                value={inputValue}
                onChange={handleInputChange}
            />
            <Button style={{marginLeft: '15px'}} onClick={handleBtnClick}>
                提交
            </Button>
            <List
                style={{
                    width:"300px",
                    marginLeft:"50px",
                    marginTop:"20px",
                    backgroundColor:"#ccc"
                }}
                bordered
                dataSource={list}
                renderItem={(item,index)=>(<List.Item onClick={()=>handleItemDelete(index)}>{item}</List.Item>)}
            />
        </div>
    )
}

export default TodoList
```

