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

