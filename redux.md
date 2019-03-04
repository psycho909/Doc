# store

```react
import reducer from './reducer'
import {createStore,applyMiddleware} from 'redux'
import thunk from 'redux-thunk'

let store=createStore(
    reducer,
    applyMiddleware(thunk)
)

export default store;
```



# reducers

```react
import { SET_GAMES,ADD_GAME,GAME_FETCHED,GAME_UPDATED,GAME_DELETE } from "../constants";

const games=(state=[],action={})=>{
    switch(action.type){
        case SET_GAMES:
            return action.games;
        case ADD_GAME:
            return [
                ...state,
                action.game
            ]
        case GAME_FETCHED:
            const index=state.findIndex((item)=>{
                return item._id === action.game._id
            })
            if(index >-1){
                return state.map(item=>{
                    if(item._id === action.game._id){
                        return action.game
                    }
                })
            }else{
                return [
                    ...state,
                    action.game
                ]
            }
        case GAME_UPDATED:
            return state.map(item=>{
                if(item._id === action.game._id){
                    return action.game
                }
            })
        case GAME_DELETE:
            return state.filter(item=>item._id !== action.gameId)
        default:return state;
    }
}

export default games
```

# constants

```react
export const SET_GAMES="SET_GAMES";
export const ADD_GAME="ADD_GAME"
export const GAME_FETCHED="GAME_FETCHED"
export const GAME_UPDATED="GAME_UPDATED"
export const GAME_DELETE="GAME_DELETE"
```

# actions

```react
import {SET_GAMES,ADD_GAME,GAME_FETCHED,GAME_UPDATED,GAME_DELETE} from '../constants/index'

export const setGames=(games)=>{
    return {
        type:SET_GAMES,
        games
    }
}

export const gameFetched=(game)=>{
    return {
        type:GAME_FETCHED,
        game
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

export const fetchGame=(id)=>{
    return (dispatch)=>{
        fetch(`/api/games/${id}`)
        .then((res)=>{
            return res.json()
        })
        .then((data)=>{
            dispatch(gameFetched(data.game))
        })
    }
}

const handleResponse=(response)=>{
    if(response.ok){
        return response.json()
    }else{
        let error=new Error(response.statusText)
        error.response=response
        throw error
    }
}

export const addGame=(game)=>{
    return {
        type:ADD_GAME,
        game
    }
}

export const saveGames=(data)=>{
    return (dispatch) => {
        return fetch('/api/games',{
            method:"post",
            body:JSON.stringify(data),
            headers:{
                "Content-Type":"application/json"
            }
        }).then(handleResponse)
        .then(data=>{
            return dispatch(addGame(data.game))
        })
    }
}

export const gameUpdated=(game)=>{
    return {
        type:GAME_UPDATED,
        game
    }
}

export const updateGames=(data)=>{
    return (dispatch) => {
        return fetch(`/api/games/${data._id}`,{
            method:"put",
            body:JSON.stringify(data),
            headers:{
                "Content-Type":"application/json"
            }
        }).then(handleResponse)
        .then(data=>{
            return dispatch(gameUpdated(data.game))
        })
    }
}

export const gameDelete=(gameId)=>{
    return {
        type:GAME_DELETE,
        gameId
    }
}

export const deleteGame=(id)=>{
    return (dispatch) => {
        return fetch(`/api/games/${id}`,{
            method:"delete",
            headers:{
                "Content-Type":"application/json"
            }
        }).then(handleResponse)
        .then(data=>{
            return dispatch(gameDelete(id))
        })
    }
}
```

# index

```react
import React from 'react'
import {connect} from 'react-redux'
import GamesList from './GamesList'
import {fetchGames,deleteGame} from '../actions'

class GamesPage extends React.Component{
    componentDidMount(){
        this.props.fetchGames();
    }
    render(){
        return (
            <div>
                <GamesList games={this.props.games} deleteGame={this.props.deleteGame} />
            </div>
        )
    }
}

const mapState=(state)=>{
    return {
        games:state.games
    }
}

export default connect(mapState,{fetchGames,deleteGame})(GamesPage)
```

