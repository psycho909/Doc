

# 給React-Router添加路由頁面切換時的過渡動畫

```js
npm install react-transition-group --save
```

`react-transition-group`提供了三個React組件，分別是`<Transition>`，`<CSSTransition>`以及`<TranstionGroup>`，關於它們的詳細api還請各位去查閱官方文檔，這裡只是簡單介紹一下它們各自的用途：

*   `<Transition>`：通過`javascript`動態修改`style`的方式為子元素添加動畫，對比`<CSSTransiton>`多了幾個編程式的`props`可以配置

*   `<CSSTransition>`：相比`<Transition>`多了一個`classNames`可以配置，通過引入CSS以及動態更改子元素`className`的方式為子元素添加動畫（是不是像極了Vue裡的`<transition>`）

*   `<TranstionGroup>`：顧名思義，為多個子元素添加動畫，需要結合`<Transition>`或`<CSSTransition>`使用

    



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

