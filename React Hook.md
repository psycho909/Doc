# React Hook

1.  useState()
2.  useEffect()
3.  useContext()

## useState

1.  **useState 什麼時候執行？** 它會在組件每次render的時候調用
2.  類似`this.state`與`this.setState`的合體

```react
import React,{useState} from 'react'

const Example=()=>{
    const [count,setCount]=useState(0);
    const increase=()=>{
        setCount(count+1);
    }
    const decrease=()=>{
        setCount(count-1);
    }
    return (
    	<div>
        	<button onClick={increase}>-</button>
        	<span>{count}</span>
        	<button onClick={decrease}>+</button>
        </div>
    )
}

```

```react
const SuperButton=(props)=>{
    const {onClick,children}=props
    const onclickHere=onClick;
    return <button onClick={onclickHere}>{children}</button>
}

const UseStateSample=()=>{
    const [count,setCount]=useState(0)

    const increased=()=>{
        setCount(count=>count+1)
    }
    const decreased=()=>{
        setCount(count-1)
    }
    return (
        <p>
            <SuperButton onClick={decreased}>-</SuperButton>
            <b>{count}</b>
            <SuperButton onClick={increased}>+</SuperButton>
        </p>
    )
}
```



## useContext

可以取代`Context.Consumer`使用

```react
import React,{useContext,useState} from 'react'

const CounterContext=React.createContext()

const CounterApp=()=>{
    const [count,setCount]=useState(1)
    const increase=()=>{
        setCount(count+1)
      }
      const decrease=()=>{
        setCount(count-1)
      }
    return (
    	<CounterContext.Provider value={{count,increase,decrease}}>
        	<Counter />
        </CounterContext.Provider>
    )
}

const Counter=()=>{
    const {count,increase,decrease}=useContext(CounterContext);
    return (
    	<div>
          <button onClick={decrease}>-</button>
          <span>{count}</span>
          <button onClick={increase}>+</button>
    	</div>
    )
}
```



```react
const MyContext=React.createContext();

const UseContextSample=()=>{
    const [count,setCount]=useState(0);
    const increment=()=>{
        setCount(count=>count+1)
    }
    const decrement=()=>{
        setCount(count=>count-1)
    }
    return (
        <div>
            <MyContext.Provider value={{increment,decrement,count}}>
                <IncrementButton/>
            </MyContext.Provider>
        </div>
    )
}

const IncrementButton=()=>{
    const {increment,decrement,count}=useContext(MyContext);

    return (
        <p>
            <h3>UseContextSample</h3>
            <button onClick={increment}>+</button>
            <b>{count}</b>
            <button onClick={decrement}>-</button>
        </p>
    )
}
```



## useEffect

`componentDidMount`和`componentWillUnmount`和`componentDidUpdate`綜合體

1.  **useEffect 什麼時候執行？** 它會在組件 mount 和 unmount 以及每次重新渲染的時候都會執行，也就是會在 componentDidMount、componentDidUpdate、componentWillUnmount 這三個時期執行。
2.  第一個參數為`componentDidMount`、`componentWillUnmount`
3.  第二個參數為`componentDidUpdate`
4.  第二個參數，若與前一次相等 effect 就不會執行，空陣列就意味著只會執行一次 ( 永遠相等 )

```js	
useEffect(
// componentDidMount
  
    return ()=>{
    // componentWillUnmount
	};
,[])
```

```js
import { useState, useEffect } from 'react';

const App = () => {
  const [users, setUsers] = useState([]);
  
  useEffect(() => {
    // Connect to the Random User API using axios
    axios("https://randomuser.me/api/?results=10")
      // Once we get a response, fetch name, username, email and image data
      // and map them to defined variables we can use later.
      .then(response =>
        response.data.results.map(user => ({
          name: `{user.name.first} ${user.name.last}`,
          username: `{user.login.username}`,
          email: `{user.email}`,
          image: `{user.picture.thumbnail}`
        }))
      )
      // Finally, update the `setUsers` state with the fetched data
      // so it stores it for use on render
      .then(data => {
        setUsers(data);
      });
  }, []);
  
  // The UI to render
  return (
    <div className="users">
      {users.map(user => (
        <div key={user.username} className="users__user">
          <img src={user.image} className="users__avatar" />
          <div className="users__meta">
            <h1>{user.name}</h1>
            <p>{user.email}</p>
          </div>
        </div>
      ))}
    </div>
  )
}
```

```js
const UseEffectSample=()=>{
    const [count,setCount]=useState(0);

    useEffect(()=>{
        const timerId=setTimeout(()=>{
            setCount(count=>count+1)
        },1000)
        return ()=>{
            return clearTimeout(timerId)
        }
    },[count])

    return (
        <p>
            time: <b>{count}</b>
        </p>
    )
}
```

## useRef

```react
const UseRefSample=()=>{
  const inputRef=useRef();
  const onButtonClick=()=>{
    console.log(inputRef.current.value)
  }
  return (
    <div>
      <input ref={inputRef} type="text"/>
      <button onClick={onButtonClick}>GET INPUT</button>
    </div>
  )
}
```

```react
const UseRefSample=()=>{
  const displayAreaRef=useRef();
  useEffect(()=>{
    let rafid=null;
    const loop=()=>{
      const now=new Date();
      displayAreaRef.current.textContent=`
      ${String(now.getHours()).padStart(2,'0')}:
      ${String(now.getMinutes()).padStart(2,"0")}:
      ${String(now.getSeconds()).padStart(2,'0')}.${String(now.getMilliseconds()).padStart(3,'0')}
      `;
      rafid=requestAnimationFrame(loop)
    };
    loop();
    return ()=>cancelAnimationFrame(rafid);
  })
  
  return (
    <p ref={displayAreaRef} />
  )
}
```