#react #js #redux
## 文件結構

```js
actionCreator
	-index.js
actionType
	- index.js
reducer
	- rootReducer.js
	- users.js
App.js
index.js
```

## 初始化

```react
// index.js
import rootReducer from './reducer/rootReducer'
import thunk from 'redux-thunk'
import {createStore,applyMiddleware} from 'redux'
import {Provider} from 'react-redux'
import {composeWithDevTools} from 'redux-devtools-extension'
import logger from 'redux-logger'

const store=createStore(
    rootReducer,
    composeWithDevTools(
        applyMiddleware(thunk,logger)
    )
)
ReactDOM.render(
    <Provider store={store}>
        <BrowserRouter>
            <App />
        </BrowserRouter>
    </Provider>
, 
document.getElementById('root'));
```

## 建立reducer

```react
import {FETCH_USERS_BEGIN,FETCH_USERS_SUCCESS,FETCH_USERS_FAILURE} from '../actionType'

const defaultState={
    users:[],
    loading:false,
    error:null
};

export default (state=defaultState,action)=>{
    switch(action.type){
        case FETCH_USERS_BEGIN:
            return {
                ...state,
                loading:true
            }
        case FETCH_USERS_SUCCESS:
            action.payload.length=10
            return {
                ...state,
                users:action.payload,
                loading:false
            }
        case FETCH_USERS_FAILURE:
            return {
                ...state,
                loading:false,
                users:[],
                error:action.payload.error
            }
        default:
            return state;
    }
}
```

## 建立actionType

```js
export const FETCH_USERS_BEGIN   = 'FETCH_USERS_BEGIN';
export const FETCH_USERS_SUCCESS = 'FETCH_USERS_SUCCESS';
export const FETCH_USERS_FAILURE = 'FETCH_USERS_FAILURE';
```

## 建立actionCreator

```js
import {FETCH_USERS_BEGIN,FETCH_USERS_SUCCESS,FETCH_USERS_FAILURE} from '../actionType'

export const fetchUsersBegin=()=>({
    type:FETCH_USERS_BEGIN
})
export const fetchUsersSuccess=(users)=>({
    type:FETCH_USERS_SUCCESS,
    payload:users
})
export const fetchUsersFailure=(error)=>({
    type:FETCH_USERS_FAILURE,
    payload:error
})

export const fetchUsers=()=>{
    return (dispatch)=>{
        // 初始化請求
        dispatch(fetchUsersBegin())
        return fetch("http://www.elifemall.com.tw/event/api/lottery_log.php")
                .then(res=>res.json())
                .then(data=>{
            		// 獲取DATA時請求
                    dispatch(fetchUsersSuccess(data))
                })
        		// 發生錯誤時請求
                .catch(error=>dispatch(fetchUsersFailure(error)))
    }
}
```

## 建立rootReducer

>   有多個reducer合併在一起

```react
import {combineReducers} from 'redux'
import users from './users'

export default combineReducers({
    users
})
```

## 使用redux

```react
import React, {Component} from 'react'
import {connect} from 'react-redux'
import {
    fetchUsers
} from './actionCreator'

class App extends Component{
    componentDidMount() {
        this.props.fetchUsers()
    }
    render(){
        if (this.props.users.error) {
            return <div>Error ! {this.props.users.error.message}</div>
        }

        if (this.props.users.loading) {
            return <div>Loading...</div>
        }
        return (
        	<div>
                <ul>
                    {this.props.users.users.map(user => (
                        <li key={user.id}>{user.name}</li>
                    ))}
                </ul>
            </div>
        )
    }
}

const mapState = state => {
    return {
        users: state.users
    }
}

const mapDispatch = {
    fetchUsers
}

export default connect(mapState,mapDispatch)(App)
```

