## 結構

```
src
	actionCreator
		- index.js
	actionTypes
		- index.js
	reducer
		- counter.js
		- rootReducer.js
		- user.js
	App.js
	index.js
```

## actionTypes/index.js

```js
export const INCREMENT = "INCREMENT";
export const DECREMENT = "DECREMENT";
export const FETCH_USERS_BEGIN   = 'FETCH_USERS_BEGIN';
export const FETCH_USERS_SUCCESS = 'FETCH_USERS_SUCCESS';
export const FETCH_USERS_FAILURE = 'FETCH_USERS_FAILURE';
```

## actionCreator/index.js

```js
import {INCREMENT,DECREMENT,FETCH_USERS_BEGIN,FETCH_USERS_SUCCESS,FETCH_USERS_FAILURE} from '../actionType'

export const increment=()=>{
    return {
        type:INCREMENT
    }
}

export const decrement=()=>{
    return {
        type:DECREMENT
    }
}

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
    return dispatch=>{
        dispatch(fetchUsersBegin())
        return fetch("http://www.elifemall.com.tw/event/api/lottery_log.php")
                .then(res=>res.json())
                .then(data=>{
                    dispatch(fetchUsersSuccess(data))
                })
                .catch(error=>dispatch(fetchUsersFailure(error)))
    }
}
```

## reducer

### reducer/counter.js

```js
import {INCREMENT,DECREMENT} from '../actionType'

const defaultState={
    count:0,
};

export default (state=defaultState,action)=>{
    switch(action.type){
        case INCREMENT:
            return {
                count:state.count+1
            }
        case DECREMENT:
            return {
                count:state.count-1
            }
        case "RESET":
            return {
                count:0
            }
        default:
            return state;
    }
}
```

### reducer/user.js

```js
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

## 合併reducer

使用 `combineReducers`

```js
import {combineReducers} from 'redux'
import counter from './counter'
import users from './users'

export default combineReducers({
    counter,
    users
})
```

## import reducer

```js
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';
import * as serviceWorker from './serviceWorker';
import rootReducer from './reducer/rootReducer'
import thunk from 'redux-thunk'
import {createStore,applyMiddleware} from 'redux'
import {Provider} from 'react-redux'


const store=createStore(
    rootReducer,
    applyMiddleware(thunk)
)

ReactDOM.render(
    <Provider store={store}>
        <App />
    </Provider>
, 
document.getElementById('root'));

// If you want your app to work offline and load faster, you can change
// unregister() to register() below. Note this comes with some pitfalls.
// Learn more about service workers: https://bit.ly/CRA-PWA
serviceWorker.unregister();

```

## 使用合併後的reducer

```js
import React, { Component } from 'react';
import {connect} from 'react-redux'
import {increment,decrement,fetchUsers} from './actionCreator'
class App extends Component {

  increment=()=>{
    this.props.increment()
  }
  decrement=()=>{
    this.props.decrement()
  }

  componentDidMount(){
    this.props.fetchUsers()
  }
  render() {
    if(this.props.error){
      return <div>Error ! {this.props.error.message}</div>
    }

    if(this.props.loading){
      return <div>Loading...</div>
    }
    return (
      <div className="App">
        <div className="counter">
          <h2>Counter</h2>
          <div>
            <button onClick={this.decrement}>-</button>
            <span className="count">{
              this.props.count
            }</span>
            <button onClick={this.increment}>+</button>
          </div>
        </div>
        <ul>
        {
          this.props.users.map(user=>
              <li key={user.id}>{user.name}</li>
            )
        }
        </ul>
      </div>
    );
  }
}

const mapState=(state)=>{
  return {
    count:state.counter.count,
    users:state.users.users,
    loading:state.users.loading,
    error:state.users.error
  }
}

const mapDispatch={
  increment,
  decrement,
  fetchUsers
}

export default connect(mapState,mapDispatch)(App);
```



