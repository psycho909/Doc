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

```react
const UseState=()=>{
  const [{count1,count2},setCount]=useState({count1:0,count2:1});
  return (
    <div>
      <button onClick={ ()=>setCount(state=>({...state,count1:state.count1-1}) ) }>-</button>
      <span>{count1} {count2}</span>
      <button onClick={()=>setCount(state=>({...state,count1:state.count1+1}))}>+</button>
    </div>
  )
}
```



## useContext

可以取代`Context.Consumer`使用

### example1

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

### example2

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

### example3

```react
// context.js
import {createContext} from 'react'
const context=createContext()
export const {Provider,Consumer} = context;
export default context;
```

```react
// UseContextOpen
import React, {useState} from 'react'
import UseContextOpenButton from './UseContextOpenButton'
import { Provider } from './context';

const UseContextOpen=()=>{
    const [open,setOpen]=useState(false);
    const toggle=()=>{setOpen(!open)}
    const contextValue={open,toggle}
    
    return (
        <Provider value={contextValue}>
            <div>
                <UseContextOpenButton/>
                { open && <div>Some Content</div> }
            </div>
        </Provider>
    )
}

export default UseContextOpen

```

```react
// UseContextOpenButton
import React, {useContext} from 'react'
import context from './context';

const UseContextOpenButton = ()=>{
    const contextValue=useContext(context)
    const {open,toggle}=contextValue
    return (
        <button onClick={toggle}>{open?"Close Btn":"Open Btn"}</button>
    )
}

export default UseContextOpenButton

```



## useEffect

`componentDidMount`和`componentWillUnmount`和`componentDidUpdate`綜合體

1.  **useEffect 什麼時候執行？** 它會在組件 mount 和 unmount 以及每次重新渲染的時候都會執行，也就是會在 componentDidMount、componentDidUpdate、componentWillUnmount 這三個時期執行。
2.  第一個參數為`componentDidMount`、`componentWillUnmount`
3.  第二個參數為`componentDidUpdate`
4.  第二個參數陣列在每次`useEffect `會去比較這個參數陣列裡面的值，跟上次傳入的做比較，如果一樣的話就不會去執行`useEffects`裡面的函式，如果傳入空陣列，代表每次傳入的時候都是空陣列，只有在第一次`render`的時候會執行。
5.  `componentWillUnmount `的執行`useEffect `傳入一個函式，可以在`return `另外一個函式，另外一個函式就會在清理函式，在`componentWillUnmount`執行
6.  `useEffect`或做 四件事情:
    1.  會判斷這次輸入的陣列是否跟上一次一樣，如果一樣才會繼續
    2.  會執行上一次存下來的清理函式
    3.  執行裡面的內容
    4.  把`return`函式存下來讓下一次可以用

```js	
useEffect(
// componentDidMount
  
    return ()=>{
    // componentWillUnmount
	};
,[])
// [] => componentDidUpdate
```
### example1

```react
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
### example2

```react
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
### example3

```react
const UseEffect=()=>{
  const [state,setState]=useState({
    email:"",
    gender:""
  });

  useEffect(()=>{
    fetch("https://randomuser.me/api/")
    .then(rs=>rs.json())
    .then(data=>{
      const [user]=data.results;
      setState({
        email:user.email,
        gender:user.gender
      })
    })
  },[])
    return (
        <div>
            <h2>User</h2>
            <span>{user.email}</span>
            <br/>
            <span>{user.gender}</span>
        </div>
    )
}
```

### ##example4##

```react
import React,{Fragment,useState,useEffect} from 'react'
import axios from 'axios'

const User = () => {
    const [data,setData]=useState({hits:[]})
    const [query,setQuery]=useState('redux')
    const [url,setUrl]=useState(
        "http://hn.algolia.com/api/v1/search?query=redux"
    )
    const [isLoading,setIsLoading]=useState(false);
    const [isError,setIsError]=useState(false);

    useEffect(()=>{
        const fetchData=async()=>{
            setIsLoading(true)
            setIsError(false)
            try{
                const result=await axios(url)
                setData(result.data)
            }catch(error){
                setIsError(true)
            }
            setIsLoading(false)
        }
        
        fetchData()
    },[url])
    const doFetch=()=>{
        setUrl(`http://hn.algolia.com/api/v1/search?query=${query}`)
    }
    return (
        <Fragment>
            <form
                onSubmit={(e)=>{
                    e.preventDefault();
                    doFetch();
                }}
            >
                <input 
                type="text"
                value={query}
                onChange={e=>setQuery(e.target.value)}
                />
                <button type="submit" >Search</button>
            </form>

            {isError && <div>Something went wrong ...</div>}
            {
                isLoading?(
                    <div>Loading...</div>
                ):(
                    <ul>
                        {
                            data.hits.map(item=>(
                                <li key={item.objectID}>
                                    <a href={item.url}>{item.title}</a>
                                </li>
                            ))
                        }
                    </ul>
                )
            }
        </Fragment>
    )
}

export default User

```



## useRef

### example1

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
### example2

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

### example3

```react
import React, {useRef,useEffect} from 'react'

const UseRefFocus = ()=>{
    const ref=useRef();
    useEffect(()=>{
        ref.current.focus();
    },[])    
    return (
        <input ref={ref} type="text"/>
    )
}

export default UseRefFocus
```

### example4

```react
import React, {useRef,useState} from 'react'

const UseRefTimer = ()=>{
    const [count,setCount]=useState(0)
    const ref=useRef({});
    const startCounter=()=>{
        ref.current=setInterval(()=>setCount(c=>c+1),100)
    }
    const stopCounter=()=>{
        clearInterval(ref.current)
    } 
    return (
        <div>
            <h1>{count}</h1>
            <button onClick={startCounter}>Start</button>
            <button onClick={stopCounter}>Stop</button>
        </div>
    )
}

export default UseRefTimer
```

### exapmle5 `ref`組件傳遞`forwardRef()`

```react
import React, {
    Component,
    useState,
    useRef,
    useEffect,
    createRef,
    forwardRef
} from 'react'

const InputComponent = () => {
    const [greeting, setGreeting] = useState('Hello React!')

    const handleChange = event => setGreeting(event.target.value)

    const ref = createRef()

    useEffect(() => ref.current.focus(), [])

    return (
        <div>
            <h1>{greeting}</h1>

            <Input value={greeting} handleChange={handleChange} ref={ref} />
        </div>
    )
}

const Input = forwardRef(({value, handleChange}, ref) => (
    <input type="text" value={value} onChange={handleChange} ref={ref} />
))
```

## useReducer

```react
const counterReducer=(state,action)=>{
  switch(action.type){
    case "INCREMENT":
      return {counter:state.counter+1};
    case "DECREMENT":
      return {counter:state.counter-1};
    default:
      return {counter:0};
  }
}
const defaultState={
  counter:0
}
const UseReducer=(props)=>{
    const [state,dispatch]=React.useReducer(counterReducer,defaultState)
    const increment=()=>{
      dispatch({type:"INCREMENT"})
    }
    const decrement=()=>{
      dispatch({type:"DECREMENT"})
    }
    return (
      <div>
        <h2>Hello {props.name}</h2>
        <div>
          <button onClick={decrement}>-</button>
          <span>{state.counter}</span>
          <button onClick={increment}>+</button>
        </div>
      </div>
    )
}
```



## Example 結合 `useState`、`useEffect`、`useReducer`

結合`useState`、`useEffect`、`useReducer`共同完成一個呼叫`BLOG API`的範例

```react
import React,{Fragment,useState,useEffect,useReducer} from 'react'
import axios from 'axios'

const dataFetchReducer=(state,action)=>{
    switch(action.type){
        case "FETCH_INIT":
            return {
                ...state,
                isLoading:true,
                isError:false
            };
        case "FETCH_SUCCESS":
            return {
                ...state,
                isLoading:false,
                isError:false,
                data:action.value
            };
        case "FETCH_FAILURE":
            return {
                ...state,
                isLoading:false,
                isError:true
            }
        default:
            throw new Error();
    }
}

const useDataApi=(initialUrl,initialData)=>{
    const [url,setUrl]=useState(initialUrl)

    const [state,dispatch]=useReducer(dataFetchReducer,{
        isLoading:false,
        isError:false,
        data:initialData
    })

    useEffect(()=>{
        let didCancel=false;

        const fetchData=async()=>{
            dispatch({type:"FETCH_INIT"})

            try{
                const result=await axios(url)

                if(!didCancel){
                    dispatch({type:"FETCH_SUCCESS",value:result.data})
                }
            }catch(error){
                if(!didCancel){
                    dispatch({type:"FETCH_FAILURE"})
                }
            }
        }
        
        fetchData()

        return ()=>{
            didCancel=true
        }
    },[url])
    const doFetch=(url)=>{
        setUrl(url)
    }

    return {...state,doFetch}
}

const User = () => {
    const [query,setQuery]=useState('redux')
    const {data,isLoading,isError,doFetch} = useDataApi(
        'http://hn.algolia.com/api/v1/search?query=redux',
        { hits: [] },
    );
    return (
        <Fragment>
            <form
                onSubmit={(e)=>{
                    e.preventDefault();
                    doFetch(`http://hn.algolia.com/api/v1/search?query=${query}`,);
                }}
            >
                <input 
                type="text"
                value={query}
                onChange={e=>setQuery(e.target.value)}
                />
                <button type="submit" >Search</button>
            </form>

            {isError && <div>Something went wrong ...</div>}
            {
                isLoading?(
                    <div>Loading...</div>
                ):(
                    <ul>
                        {
                            data.hits.map(item=>(
                                <li key={item.objectID}>
                                    <a href={item.url}>{item.title}</a>
                                </li>
                            ))
                        }
                    </ul>
                )
            }
        </Fragment>
    )
}

export default User

```

## useMemo()

```react
import React, { useEffect } from "react";

let count = 0;

function NestedComponent(props) {
  useEffect(() => {
    count += 1;
  });
  return <div>Render Count: {count}</div>;
}

const App=()=>{
    const memoizedChildComponent = useMemo(() => {
        return <NestedComponent />;
      }, []);
}

```

