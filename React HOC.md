1. MyComponent
2. HOC/WithLocalStorage
3. App

## WithLocalStorage

```react
import React from 'react';

const WithLocalStorage = localStorageKey=>View=>{
    return class extends React.Component{
        state={
            [localStorageKey]:localStorage.getItem(localStorageKey)
        }

        setLocalStorage=value=>{
            localStorage.setItem(localStorageKey,value)
        }

        render(){
            return (
                <div>
                    <h3>{localStorageKey}</h3>
                    <View {...this.state} {...this.props} setLocalStorage={this.setLocalStorage} />
                </div>
            )
        }
    }
}

export default WithLocalStorage;
```

## 1.MyComponent

```react
import React from 'react'
import WithLocalStorage from './HOC/WithLocalStorage'

class MyComponent extends React.Component {
    state={
        value:this.props.myValueInLocalStorage || '',
    }

    componentDidUpdate(){
        this.props.setLocalStorage(this.state.value)
    }

    onChange=(e)=>{
        this.setState({
            value:e.target.value
        })
    }

    render(){
        return (
            <div>
                <h1>
                    Hello React ES6 Class Component with Higher-Order Component!
                </h1>
                <input type="text" onChange={this.onChange} value={this.state.value} />
            </div>
        )
    }
}
export default WithLocalStorage('myValueInLocalStorage')(MyComponent)
```

## 2.MyComponent + useState + useEffect

```react	
import React from 'react'
import WithLocalStorage from './HOC/WithLocalStorage'

const useStateWithLocalStorage = localStorageKey => {
    const [value, setValue] = React.useState(
        localStorage.getItem(localStorageKey) || ''
    )

    React.useEffect(() => {
        console.log(value)
        localStorage.setItem(localStorageKey, value)
    }, [value])

    return [value, setValue]
}
const MyComponent = () => {
    const [value, setValue] = useStateWithLocalStorage(
        'myValueInLocalStorage',
    );

    const onChange = e => {
        setValue(e.target.value)
    }

    return (
        <div>
            <h1>
                Hello React ES6 Class Component with Higher-Order Component!
            </h1>
            <input type="text" onChange={onChange} value={value} />
            <p>{value}</p>
        </div>
    )
}

export default WithLocalStorage('myValueInLocalStorage')(MyComponent)

```



## App

```react
import React, { Component } from 'react';
import MyComponent from './MyComponent'

class App extends Component {
  render() {
    return (
      <div className="App">
        <MyComponent />
      </div>
    );
  }
}

export default App;

```

