# Vuex
|角色|職權|
|:----:|:----:|
|state|存放 state，類似原本的 data|
|action|調用處理 ajax 及調用 mutation (不改變 State)，類似 methods|
|getter|負責取得 state 資料，類似 computed|
|mutation|負責改變 state 資料 (Vuex 文件說明，盡可能使用 mutation 改變狀態)|
## Vuex的使用
```javascript
npm install vuex --save
```
### 1.Vuex Store
> 創建`Vuex.store`(倉庫)開始，在`src`下創建一個`store`的文件，並新建一個`store.js`，在這個文件中將包函我們所需要的store
>
> 每一個Vuex應用的核心就是`store`(倉庫)。`store`基本上就是一個容器，他包含著你的應用中大部分的狀態(`state`)。Vuex和單純的全局對像有以下兩點不同:
>
>1. Vuex的狀態存儲是響應式的。當Vue組件從存儲中讀取狀態的時候，若存儲中的狀態發生變化，那麼相應的組件也會相應地得到高效更新。
>1. 你不能直接改變存儲中的狀態。改變存儲中的狀態的唯一途徑就是顯式地提交（commit）突變。這樣使得我們可以方便地跟踪每一個狀態的變化，從而讓我們能夠實現一些工具幫助我們更好地了解我們的應用。
>
> Vuex的最新特點之一是能夠在一個`store`文件中定義`actions`和`getter`
>
### `store`的基本結構
```javascript
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

export default new Vuex.Store({
    state:{
    },
    mutations:{
    },
    actions:{
    }
})
```
> 當創建一個`state`對象，當應用程序啟動時，他保持初始狀態。接下來，我們創建一個對象來存儲我們的`mutations`。在Vuex中`mutations`基本上是事件。最後，我們創建一個對象來存儲我的`actions`。`actions`是用來分派`mutations`的函數。
```javascript
export default new Vuex.Store({
    state:{
        todos:[],
        newTodo:''
    },
    mutations: {
        GET_TODO(state, todo){
            state.newTodo =  todo
        },
        ADD_TODO(state){
            state.todos.push({
                body: state.newTodo,
                completed: false
            })
        },
        EDIT_TODO(state, todo){
            var todos = state.todos
            todos.splice(todos.indexOf(todo), 1)
            state.todos = todos
            state.newTodo = todo.body
        },
        REMOVE_TODO(state, todo){
            var todos = state.todos
            todos.splice(todos.indexOf(todo), 1)
        },
        COMPLETE_TODO(state, todo){
            todo.completed = !todo.completed
        },
        CLEAR_TODO(state){
            state.newTodo = ''
        }
    },
    actions:{
        getTodo({commit},todo){
            commit('GET_TODO',todo)
        },
        addTodo({commit}){
            commit('ADD_TODO')
        },
        editTodo({commit},todo){
            commit('EDIT_TODO',todo)
        },
        removeTodo({commit},todo){
            commit('REMOVE_TODO',todo)
        },
        completeTodo({commit},todo){
            commit('COMPLETE_TODO',todo)
        },
        clearTodo({commit}){
            commit('CLEAR_TODO')
        }
    },
    getters:{
        newTodo:state=>state.newTodo,
        todos:state=>state.todos.filter((todo)=>{
            return !todo.completed
        }),
        completedTodos:state=>state.todos.filter((todo)=>{
            return todo.completed
        })
    }
})
```
### Actions
> Vuex的actions期望一個存儲作為必需的參數，然後是一個可選的參數。如果使用參數遭到破壞，可以使用以下語法傳遞額外的參數:
```javascript
actions:{
    addTodo({commit}){
        commit('ADD_TODO')
    }
}
```
### 開始使用
> Vuex2中，分發用於觸發`actions`，而`commit`用於觸發`mutations`。一個`actions`的分對應一個`mutations`的`commit`。由於我們的操作是在Vuex存儲中定義的，所以可以在多個模塊中使用單個調用來分發`actions`：
```html
<button class="btn btn-primary" @click="addTodo">Add Todo</button>
```
> 當addTodo被分發時，它將觸發名叫ADD_TODO的mutation，並將一個新的todo推送(push())到todos列表。其餘的動作，當被觸發時將以類似的方式表現，因此沒有必要對每一個動作進行檢查。
### 使用getters
```javascript
store.getters
```
> Vuex 允許我們在 store 中定義getters（可以認為是 store 的計算屬性）。就像計算屬性一樣，getters的返回值會根據它的依賴被緩存起來，且只有當它的依賴值發生了改變才會被重新計算
>
> `getter`放在`computed`的屬性裡