```js
// hero.js
export default [
  {
    name: 'Amethyst',
    alter_ego: 'Amy Winston',
    first_appearance: 'LEGION OF SUPER-HEROES #298 (1983)',
  },
  {
    name: 'Aquaman',
    alter_ego: 'Arthur Curry',
    first_appearance: 'MORE FUN COMICS #73 (1941)',
  },
  {
    name: 'Arsenal',
    alter_ego: 'Roy Harper',
    first_appearance: 'ADVENTURE COMICS #250 (1958)',
  },
  {
    name: 'Atom',
    alter_ego: 'Ray Palmer',
    first_appearance: 'SHOWCASE #34 (1961)',
  },
  {
    name: 'Batgirl',
    alter_ego: 'Barbara Gordon',
    first_appearance: 'BATMAN #139 (1961)',
  },
  {
    name: 'Batman',
    alter_ego: 'Bruce Wayne',
    first_appearance: 'DETECTIVE COMICS #27',
  }
];
```

App.js

```react
import React from 'react'

const App=({char, searchTerm, searchTermChanged})=>{
    return (
        <section>
            <div id="header">
                <h1>SuperHeros</h1>
                <h3>a lsit of Major SuperHeros</h3>
            </div>

            <form>
                <div className="search">
                    <input
                        type="text"
                        name="search"
                        placeholder="search"
                        value={searchTerm}
                        onChange={e => searchTermChanged(e.target.value)}
                    />
                </div>
            </form>

            <table>
                <thead>
                    <tr style={{textAlign: 'center'}}>
                        <th>Name</th>
                        <th>Alert Ego</th>
                        <th>First Appearence</th>
                        <th>View</th>
                    </tr>
                </thead>
                <tbody>
                    {char.map(curChar => (
                        <tr key={curChar.name}>
                            <td>{curChar.name}</td>
                            <td>{curChar.alert_ego}</td>
                            <td>{curChar.first_appearance}</td>
                        </tr>
                    ))}
                </tbody>
            </table>
        </section>
    )
}

export default App
```

# Provider

```react
// Provider.js
import React, { Component } from 'react'
import Char from './hero'

const DEFAULT_STATE = {
    allChar: Char,
    searchTerm: ''
}

export const ThemeContext = React.createContext(DEFAULT_STATE)

export default class Provider extends Component{
    state = DEFAULT_STATE
    
    searchTermChanged = searchTerm => {
        this.setState({searchTerm})
    }
    
    render(){
        return(
            <ThemeContext.Provider
                value={{
                    ...this.state,
                    searchTermChanged: this.searchTermChanged
                }}
            >
                {this.props.children}
            </ThemeContext.Provider>
        )
    }
}
```

# Consumer

```react
// Consumer
import React, { Component } from 'react'
import {ThemeContext} from './Provider'

export default class Consumer extends Component{
    render(){
        const {children} = this.props
        
        return(
            <ThemeContext.Consumer>
                {({allChar, searchTerm, searchTermChanged}) => {
                    const char = searchTerm? allChar.filter(
                        char => char.name.toLowerCase().indexOf(searchTerm.toLowerCase()) > -1
                    ) : allChar
                    return React.Children.map(children, child => React.cloneElement(child, {
                        char,
                        searchTerm,
                        searchTermChanged
                    }))
                }}
            </ThemeContext.Consumer>
        )
    }
}
```

# index.js

```js
import React from 'react'
import ReactDOM from 'react-dom'
import Provider from './Provider'
import Consumer from './Consumer'
improt App from './App'

ReactDOM.render(
    <Provider>
        <Consumer>
            <App />
        </Consumer>
    </Provider>,
    document.getElementById('root')
)
```

