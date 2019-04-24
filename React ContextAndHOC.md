## 資料夾結構

```js
src
	- Card.js
	- App.js
	- styles.js
	- ThemeContext.js
	- WithTheme.js
```

## 初始化

### styles.js

```react
const styles = {
    dark: {
        color: 'white',
        backgroundColor: 'black'
    },
    default: {
        color: 'black',
        backgroundColor: 'white'
    }
}

export default styles
```

### Card.js

```react
import React from 'react'

import styles from './styles'

const Card = ({themeData}) => (
    <div style={styles[themeData.theme]}>
        <h1>Card</h1>
        <p>{themeData.themes.toString()}</p>
    </div>
)

export default WithTheme(Card)

```

### App.js

```react
import React from "react";
import Card from "./Card";

const Container = props => <Card />;

class App extends React.Component {
  state = {
    theme: "dark",
    themes:["light","dark"]
  };

  switchTheme = () => {
    const newTheme = this.state.theme === "dark" ? "default" : "dark";
    this.setState({
      theme: newTheme
    });
  };

  render() {
    return (
      <div>
        <button onClick={this.switchTheme}>Switch theme</button>
        <Container themeData={this.state} />
      </div>
    );
  }
}
export default App;
```

## 第一步 創建 createContext

### ThemeContext.js

```react
import React from 'react'
const ThemeContext=React.createContext();
export default ThemeContext;
```

### 改變App.js傳遞Card.js方式

```react
...
import ThemeContext from "./ThemeContext";

const Container = () => (
  <ThemeContext.Consumer>
    {({themeData}) => <Card themeData={themeData} />}
  </ThemeContext.Consumer>
)
...

return (
    <div>
        <button onClick={this.switchTheme}>Switch theme</button>
        <ThemeContext.Provider value={this.state}>
            <Container />
        </ThemeContext.Provider>
    </div>
);
...
```

## 第二步 使用HOC 改變傳遞方式

### 創建 HOC 

#### WithTheme.js

```js
import React from "react";

import ThemeContext from "./ThemeContext";

const withTheme = Component => {
  return class extends React.Component {
    render() {
      return (
        <ThemeContext.Consumer>
          {({theme}) => <Component theme={theme} />}
        </ThemeContext.Consumer>
      );
    }
  }
};

export default withTheme;
```

### 使用  HOC 修改傳遞方式

#### App.js

```react
...
const Container = () => <Card />;
...
```

### Card.js

```react
import React from 'react';

import WithTheme from "./WithTheme";
import styles from './styles';

const Card = (props) => (
    <div style={styles[props.theme]}>
        <h1>Card</h1>
    </div>
)

export default WithTheme(Card);
```

## 第三部  Access `this.context` using `contextType`

### this.context

React 16.6 introduced `contextType` which allows you to access `this.context`to:

1.  Access context inside the lifecycle methods
2.  Use context without using the *render prop* pattern

### 修改 WithTheme.js

```react
import React from 'react'
import ThemeContext from "./ThemeContext";

const WithTheme=Component=>{
    return class extends React.Component{
        static contextType=ThemeContext;
        componentDidMount() {
            console.log(`current theme: ${this.context}`);
        }
        render(){
            return <Component themeData={this.context} />;
        }
    }
}

export default WithTheme
```

