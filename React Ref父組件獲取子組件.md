#react #js #redux 
## 類組件狀況下

### 父組件使用`ref`

```react
class UseRef1 extends Component{
  child=React.createRef();
  focus=()=>{
    console.log(this.child.current.inputRef.current.value)
    this.child.current.inputRef.current.focus();
  }
  render(){
    return (
      <div>
        <Child ref={this.child} />
        <button onClick={this.focus}>獲取焦點</button>
      </div>
    )
  }
}
```

### 子組件

```react
class Child extends Component{
  state={
    name:"HAHA"
  }
  inputRef=React.createRef()

  render(){
    return (
      <input type="text" value={this.state.name} ref={this.inputRef} onChange={(e)=>this.setState({name:e.target.value})} />
    )
  }
}
```

## 函數組件

父組件

```react
const ParentRef=()=>{
  const parentRef=React.useRef();
  const focusHander=()=>{
    console.log(parentRef.current.value)
    parentRef.current.focus();
  }
  return (
    <>
      <ForwardChild ref={parentRef} />
      <button onClick={focusHander}>獲取焦點</button>
    </>
  )
}
```

子組件

```react
const ChildFn=(props,parentRef)=>{
  console.log(props)
  return (
    <>
      <input type="text" ref={parentRef} />
    </>
  )
}

let ForwardChild=React.forwardRef(ChildFn)
```

## 優化

