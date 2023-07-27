#react #js 
## react-lazyload

>   懶加載你的組件、圖片和其它與性能相關的任何東西

```js
import React,{useEffect,useState} from 'react';
import LazyLoad from 'react-lazyload'
```

```react
const Spinner=()=>{
  return (
    <div className="post loading">
      <h5>Loading...</h5>
    </div>
  )
}

const Post=({id,title,body})=>{
  return (
    <div className="post">
      <LazyLoad
        once={true}
        placeholder={<img src={`https://picsum.photos/id/${id}/5/5`} alt=""/>}
      >
        <div className="post-img">
          <img src={`https://picsum.photos/id/${id}/200/200`} alt=""/>
        </div>
      </LazyLoad>
      <div className="post-body">
        <h4>{title}</h4>
        <p>{body}</p>
      </div>
    </div>
  )
}

function App() {
  var [list,setList]=useState([])

  useEffect(()=>{
    fetch("https://jsonplaceholder.typicode.com/posts")
    .then(res=>res.json())
    .then(data=>{
      setList(data);
    })
  },[])

  return (
    <div className="App">
      <h2>LazyLoad Demo</h2>
      <div className="post-container">
        {
          list.map(post=>{
            return (
              <LazyLoad 
              key={post.id} 
              height={100}
              offset={[-100,100]}
              placeholder={ <Spinner/> }
              >
                <Post key={post.id} {...post}  />
              </LazyLoad>
            )
          })
        }
      </div>
    </div>
  );
}
```

