# 生命週期

> 生命週期函數只在某一個函數組件會自動執行的函數

## Mounting

### componentWillMount

> 在組件即將被掛載到頁面時候自動執行

### componentDidMount

> 組件被掛載到頁面之後，自動被執行一次，之後就不會再重新被重複執行

> ajax請求使用此生命週期

## Updation

### shouldComponentUpdate(nextProps,nextState)

> 在組件準備更新之前調用，可以控制組件是否進行更新，返回 true 時組件更新，返回 false 組件不更新。

> return true 會執行更新，return false不會執行更新

```react
  shouldComponentUpdate(nextProps,nextState){
      if(nextState.Number == this.state.Number){
        return false
      }
  }
```



#### nextProps

> 接下來props會變化成怎樣

#### nextState

> 接下來state會變化成怎樣

### componentWillUpdate

> 組件被更新之前，它會自動執行，但是他在`shouldComponentUpdate`之後被執行，如果`shouldComponentUpdate`返回`true`才執行，如果返回`false`不會執行更新

### componentDidUpdate(prevProps, prevState, snapshot)

> 在更新發生之後立即被調用。這個生命週期在組件第一次渲染時不會觸發。

### componentWillReceiveProps(nextProps)

> 當一個組件從父組件接收了參數。
>
> 只要父組件的render函數被重新執行了，子組件這個生命週期函數就會被執行。



> 如果這組件第一次存在於父組件中，不會執行。
>
> 如果這個組件之前已經存在於父組件中，才會執行。

```js
componentWillReceiveProps(nextProps){
    this.setState({
        _id:nextProps.booklist._id,
        id:nextProps.booklist.id,
        title:nextProps.booklist.title,
        author:nextProps.booklist.author
    })
}
```

## Unmounting

### componentWillUnmount

>   在組件即將被卸載或銷毀時進行調用。
>
>   此生命週期是**取消網絡請求**、移除**監聽事件**、**清理 DOM 元素**、**清理定時器**等操作的好時機

## 注意

>**注意：** componentWillMount()、componentWillUpdate()、componentWillReceiveProps() 即將被廢棄，請不要再在組件中進行使用。因此本文不做講解，避免混淆。



# 新的生命週期

## static getDerivedStateFromProps(nextProps, prevState)

1.  用來取代`componentWillReceiveProps`
2.  不可以直接訪問`this.state`&`this.props`
3.  兩個參數`nextProps`&`prevState`
4.  不常使用

> 在每次調用 render 方法之前調用。包括初始化和後續更新時。

```js
state = {
    // 从 props 获取默认 state
    result: this.getResult(this.props.value)
}
componentWillReceiveProps(nextProps) {
    // 常用范式
    if (nextProps.value !== this.props.value) {
        this.setState({
            // 更新 state
            result: this.getResult(nextProps.value)
        })
    }
}
```

```js
state = {
    result: 0,
    // 必须存储 props.value 到 state 的副本，以便 getDerivedStateFromProps 取到 
    value: 0
}
// 新的方法，接收 nextProps 和 prevState
static getDerivedStateFromProps(nextProps, prevState) {
    // prevState.value 相当于当前组件的 this.props.value，是存在 state 的副本
    if (prevState.value !== nextProps.value) {
        // 返回新的state（只需返回更新的部分，与 `setState` 相同）
        return {
            // 相当于上面的 getResult，但只有一处
            result: nextProps.value * nextProps.value,
            // 又一次保存副本
            value: nextProps.value
        }
    }
    // 返回 null 表示不更新，此函数最后一定需要返回值
    return null
}

```

## getSnapshotBeforeUpdate

1.  用來取代`componentWillUpdate`
2.  接收兩個參數`prevProps`&`prevState`
3.  返回值會傳給`componentDidUpdate`的第三個參數`snapshot`

> 在最近一次的渲染輸出被提交之前調用。也就是說，在 render 之後，即將對組件進行掛載時調用。

```react
// list` 條目增加，渲染的 `li` 也增加。但是滾動條位置不變
export default class List extends React.Component {
    listRef = null
    // 新的生命週期
    getSnapshotBeforeUpdate(prevProps, prevState) {
        // 如果 `props.list` 增加，將原來的 scrollHeight 存入 listRef
        if (prevProps.list.length < this.props.list.length) {
            return this.listRef.scrollHeight
        }
        return null
    } 
    // snapshot 就是 `getSnapshotBeforeUpdate` 的返回值
    componentDidUpdate(prevProps, prevState, snapshot) {
        if (snapshot !== null) {
            // scrollTop 增加新的 scrollHeight 和原來 scrollHeight 的差值，以保持滾動條位置不變
            this.listRef.scrollTop += this.listRef.scrollHeight - snapshot
        }
    } 
    
    setListRef = ref => (this.listRef = ref)

    render() {
        return (
            <ul ref={this.setListRef} style={{ height: 200, overflowY: 'scroll' }}>
                {this.props.list.map((n, i) => (
                    <li key={i}>{n}</li>
                ))}
            </ul>
        )
    }
}
```

# 生命週期執行順序

## 掛載時

* constructor()
* static getDerivedStateFromProps()
* render()
* componentDidMount()

## 更新時

* static getDerivedStateFromProps()
* shouldComponentUpdate()
* render()
* getSnapshotBeforeUpdate()
* componentDidUpdate()

## 卸載時

* componentWillUnmount()

# 生命週期中是否可調用 setState()

## 初始化 state

* constructor()

## 可以調用 setState()

* componentDidMount()

## 根據判斷條件可以調用 setState()

* componentDidUpdate()

## 禁止調用 setState()

- shouldComponentUpdate()
- render()
- getSnapshotBeforeUpdate()
- componentWillUnmount()

# 從五種組件狀態改變的時機來探究生命週期的執行順序

## 一、父子組件初始化

* Parent 組件： constructor()
* Parent 組件： getDerivedStateFromProps()
* Parent 組件： render()
* Child 組件： constructor()
* Child 組件： getDerivedStateFromProps()
* Child 組件： render()
* Child 組件： componentDidMount()
* Parent 組件： componentDidMount()

## 二、修改子組件自身狀態 state 時

> 點擊子組件中的 *改變自身狀態* 按鈕，則界面上 *自身狀態 counter：* 的值會 + 1，控制台中的 log 打印順序為：

* Child 組件： getDerivedStateFromProps()
* Child 組件： shouldComponentUpdate()
* Child 組件： render()
* Child 組件： getSnapshotBeforeUpdate()
* Child 組件： componentDidUpdate()

## 三、修改父組件中傳入子組件的 props 時

> 點擊父組件中的 *改變傳給子組件的屬性 num* 按鈕，則界面上 *父組件傳過來的屬性 num：* 的值會 + 1，控制台中的 log 打印順序為：

* Parent 組件： getDerivedStateFromProps()
* Parent 組件： shouldComponentUpdate()
* Parent 組件： render()
* Child 組件： getDerivedStateFromProps()
* Child 組件： shouldComponentUpdate()
* Child 組件： render()
* Child 組件： getSnapshotBeforeUpdate()
* Parent 組件： getSnapshotBeforeUpdate()
* Child 組件： componentDidUpdate()
* Parent 組件： componentDidUpdate()

## 四、卸載子組件

> 點擊父組件中的 *卸載 / 掛載子組件* 按鈕，則界面上子組件會消失，控制台中的 log 打印順序為

- Parent 組件： getDerivedStateFromProps()
- Parent 組件： shouldComponentUpdate()
- Parent 組件： render()
- Parent 組件： getSnapshotBeforeUpdate()
- Child 組件： componentWillUnmount()
- Parent 組件： componentDidUpdate()

## 五、重新掛載子組件

> 再次點擊父組件中的 *卸載 / 掛載子組件* 按鈕，則界面上子組件會重新渲染出來，控制台中的 log 打印順序為：

* Parent 組件： getDerivedStateFromProps()
* Parent 組件： shouldComponentUpdate()
* Parent 組件： render()
* Child 組件： constructor()
* Child 組件： getDerivedStateFromProps()
* Child 組件： render()
* Parent 組件： getSnapshotBeforeUpdate()
* Child 組件： componentDidMount()
* Parent 組件： componentDidUpdate()

# 父子組件生命週期執行順序總結

* 當子組件自身狀態改變時，不會對父組件產生副作用的情況下，父組件不會進行更新，即不會觸發父組件的生命週期
* 當父組件中狀態發生變化（包括子組件的掛載以及）時，會觸發自身對應的生命週期以及子組件的更新
  * render 以及 render 之前的生命週期，則 父組件 先執行
  * render 以及 render 之後的聲明週期，則子組件先執行，並且是與父組件交替執行
* 當子組件進行卸載時，只會執行自身的 componentWillUnmount 生命週期，不會再觸發別的生命週期
