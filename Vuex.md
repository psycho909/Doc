# Actions / Mutations / State

## Actions

1. 定義整個App的所有行為，使用者在前端元件觸發的事件會`dispatch`給`Actions`，接著`Actions`會去`commit mutation`，進而讓相對應的`mutation handler`去做更改狀態的動作。

2. 在這個階段因為常要從後端讀取資料，所以也可以進行`非同步地`跟`Backend API`溝通。

## Mutations

1. 透過`commit`接收`Actions`傳遞的資料與行為，並經過計算處理過後改變State。
2. 每個`Mutation`都有一個字串型態的`事件類型(type)`和一個`回調函數(handler)`。
3. `回調函數(handler)`就是我們實際更改狀態的地方，第一個參數即帶入`State`。
4. 注意只有使用`commit mutation`才能改變在`Store`中的狀態，這個動作就像是`註冊事件`一樣，是不可以直接調用`mutation handler的`。

## State

1. 用一個物件型態記錄整個App的所有狀態。
2. 讓`Mutation`去更改狀態。
3. 雖然建議是將App的所有狀態全部放入`Store`，但是Vuex還是保有彈性，可以讓元件保有自己的局部狀態。

# Actions / Mutations差別

## Actions

1. 更改狀態:`Actions`只能透過`commit mutation`去提交事件，不能直接更改狀態。
2. 處理的事件種類:可同時處理非同步事件(call API)，這樣就可以在此階段等待API回應的時間，接收到的資料再`commit`給`Mutations`，`Mutations`收到的資料就會是即時的。

## Mutations

1. 更改狀態:`Mutations`透過`mutation handler`直接實際更改狀態。
2. 處理的事件種類:只能處理同步事件。

|角色|職權|
|:----:|:----:|
|state|存放 state，類似原本的 data|
|action|調用處理 ajax 及調用 mutation (不改變 State)，類似 methods|
|getter|負責取得 state 資料，類似 computed|
|mutation|負責改變 state 資料 (Vuex 文件說明，盡可能使用 mutation 改變狀態)|
# Vuex的使用
```javascript
npm install vuex --save
```
## Vuex Store
> 創建`Vuex.store`(倉庫)開始，在`src`下創建一個`store`的文件，並新建一個`store.js`，在這個文件中將包函我們所需要的store
>
> 每一個Vuex應用的核心就是`store`(倉庫)。`store`基本上就是一個容器，他包含著你的應用中大部分的狀態(`state`)。Vuex和單純的全局對像有以下兩點不同:
>
>1. Vuex的狀態存儲是響應式的。當Vue組件從存儲中讀取狀態的時候，若存儲中的狀態發生變化，那麼相應的組件也會相應地得到高效更新。
>1. 你不能直接改變存儲中的狀態。改變存儲中的狀態的唯一途徑就是顯式地提交（commit）突變。這樣使得我們可以方便地跟踪每一個狀態的變化，從而讓我們能夠實現一些工具幫助我們更好地了解我們的應用。
>
> Vuex的最新特點之一是能夠在一個`store`文件中定義`actions`和`getter`
>
## `store`的基本結構
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
## Actions
> Vuex的actions期望一個存儲作為必需的參數，然後是一個可選的參數。如果使用參數遭到破壞，可以使用以下語法傳遞額外的參數:
```js
actions:{
    addTodo(context,data){
        context.commit('ADD_TODO',data)
    }
}
```

```javascript
actions:{
    addTodo({commit},data){
        commit('ADD_TODO',data)
    }
}
```
## 組合 actions

```js
actions: {
  actionA ({ commit }) {
    return new Promise((resolve, reject) => {
      setTimeout(() => {
        commit('someMutation')
        resolve()
      }, 1000)
    })
  }
}
```
## 延時改變 Promise後dispatch

>   當一個 action 返回 **Promise** 時，`store.dispatch` 可以處理返回（return）的 **Promise** ：

```js
actions: {
    waitIncrement({commit}){
        return new Promise((resolve,reject)=>{
            setTimeout(()=>{
                commit(INCREMENT)
                console.log(1000)
                resolve()
            },1000)
        })
    },
    incrementAsync({dispatch,commit,state}){
        return dispatch('waitIncrement').then(()=>{
            commit(INCREMENT)
            console.log("Async")
        })
    }
}

// 组件里 1s之后变值
store.dispatch('waitIncrement').then(() => { ... })
// 或者
store.dispatch('incrementAsync')
```

## 在 actions 中使用 async / await

```js
actions:{
    async actionA({commit}){
        return new Promise((resolve,reject)=>{
            setTimeout(()=>{
                commit(INCREMENT)
                console.log(1000)
                resolve()
            },1000)
        })
    },
    async incrementAsync({dispatch,commit,state}){
        await dispatch("actionA") // 等待actionA 完成
        commit(INCREMENT)
    }
}

```



## 開始使用

> Vuex2中，分發用於觸發`actions`，而`commit`用於觸發`mutations`。一個`actions`的分對應一個`mutations`的`commit`。由於我們的操作是在Vuex存儲中定義的，所以可以在多個模塊中使用單個調用來分發`actions`：
```html
<button class="btn btn-primary" @click="addTodo">Add Todo</button>
```
> 當addTodo被分發時，它將觸發名叫ADD_TODO的mutation，並將一個新的todo推送(push())到todos列表。其餘的動作，當被觸發時將以類似的方式表現，因此沒有必要對每一個動作進行檢查。
## 使用getters
```javascript
store.getters
```
> Vuex 允許我們在 store 中定義getters（可以認為是 store 的計算屬性）。就像計算屬性一樣，getters的返回值會根據它的依賴被緩存起來，且只有當它的依賴值發生了改變才會被重新計算
>
> `getter`放在`computed`的屬性裡

## 使用方式

### state

```vue
<div>{{$store.state.name}}</div>
```

```js
<div>{{getname}}</div>

import {mapeState} from 'vuex'

computed:{
    // 數組寫法
    ...mapState(['name']),
    
    mapState({
        // 箭頭函數
        count:state=>state.count,
        // 傳字符串參數'count' 等同於'state => state.count'
        count:"count",
        // 在使用state中的數據時,如果依賴組件內部的數據,必須使用常規函數
        count(state){
            return state.count+this.msg
        }
    })
    getname(){
		return this.name
	}
}
```

### getters

```html
<div>{{$store.getters.getname}}</div>
```

```js
<div>{{getName}}</div>

import {mapeGetters} from 'vuex'
computed:{
    ...mapGetters(['getname']),
    getName(){
        return this.getname;
    }
},
```

### mutations

```js
methods:{
    changeName(v){
        this.$store.commit('CHANGENAME',v)
    }
}
```

```js
import {mapMutations} from 'vuex'

methods:{
    ...mapMutations(['CHANGENAME']),
    changeName(v){
        this.CHANGENAME(v)
    }
}
```

### actions

```js
methods:{
    changeName(v){
        this.$store.dispatch('changename',v)
    }
}
```

```js
import {mapActions} from 'vuex'

methods:{
    ...mapActions(['changename']),
    changeName(v){
        this.changename(v)
            .then((res)=>{
            
       		})
    }
}
```

# Watch 方法

```vue
const store = new Vuex.Store({
  state: {
    count: 0
  },
  getters: {
    getCountPlusOne: state => state.count + 1
  },
  mutations: {
    increment(state) {
      state.count++;
    }
  }
});

```

`watch` 是將 Vuex 與其他外部代碼整合的最有用的方法，可以在你的 `awesomeService` 或者是在 `catchAllAuthUtils` 等等類似的服務中使用。

```vue
const unsubscribe = store.watch(
  (state, getters) => {
    return [state.count, getters.getCountPlusOne];
  },
  watched => {
    console.log("Count is:", watched[0]);
    console.log("Count plus one is:", watched[1]);
  },
  {}
);

// To unsubscribe:
unsubscribe();
```

我們所做的就是在調用 vuex 的實例方法 `watch` 時，傳入兩個函數作為實參，第一個函數實參返回我們想要在 state 與/或 getters 上監聽的屬性；第二個函數實參是當屬性值 `state.count` 或 `getters.getCountPlusOne` 有改變時，調用的回調函數。

這是用來結合 Vuex 與 react 或者 angular 甚至是 JQuery 代碼時，非常有用的技巧

# SubscribeAction 方法

有時候，與其監聽 store 中的一個屬性改變，不如使用 `subscribeAction` 方法訂閱一個特定的 action，比如像 `login` 和 `logout` 之類的異步請求，這也是更有用的方案。

調用監聽函數，在每一個 action 分發的時候調用指定的回調函數，並在其中調用自定義代碼。

我們在每一個 action 的分發前以及完成後，來分別開始和停止全局的 spinner。

```vue
const unsubscribe = store.subscribeAction({
  before: (action, state) => {
    startBigLoadingSpinner();
  },
  after: (action, state) => {
    stoptBigLoadingSpinner();
  }
});

// To unsubscribe:
unsubscribe();

```

# Vuex Modules

```
src
	store
		modules
			- user.js
		- index.js
		- types.js
	main.js
```

## main.js

```js
import Vue from 'vue'
import App from './App.vue'
import router from './router'
import store from './store/index.js'

Vue.config.productionTip = false

new Vue({
  router,
  store,
  render: h => h(App)
}).$mount('#app')

```

## store/types.js

```js
const INCREMENT="INCREMENT";
const DECREMENT="DECREMENT";

export default{
    INCREMENT,DECREMENT
}
```

## store/modules/user.js

```js
import types from '../types'

const moduleUser={
    namespaced:true,
    state:{
        count:5
    },
    getters:{
        count(state){
            return state.count
        },
        isEvenOrOdd(state){
            return state.count & 2 === 0?"Even":"Odd"
        }
    },
    mutations:{
        [types.INCREMENT](state){
            return state.count++
        },
        [types.DECREMENT](state){
            return state.count--
        }
    },
    actions:{
        increment({commit,state}){
            commit(types.INCREMENT)
        },
        decrement({commit,state}){
            commit(types.DECREMENT)
        },
        incrementAsync({commit,state}){
            var p=new Promise((resolve,reject)=>{
                setTimeout(()=>{
                    resolve()
                },500)
            })
            p.then(()=>{
                commit(types.INCREMENT)
            }).catch(()=>{
                console.log("Async")
            })
        }
    }
}

export default moduleUser
```

## store/index.js

```js
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex);

import moduleUsers from './modules/user'

export default new Vuex.Store({
    modules:{
        users:moduleUsers
    }
})
```

## 使用modules模塊

```vue
<template>
  <div class="vuex">
    <button @click="decrement">-</button>
    <div>count {{count}} {{isEvenOrOdd}}</div>
    <button @click="increment">+</button>
    <button @click="incrementAsync">async add</button>
  </div>
</template>

<script>
import {mapState,mapGetters,mapActions} from 'vuex'
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  computed:{
    // 方法一
    ...mapState({
		count:state=>state.user.count
    }),
    ...mapGetters({
      count:'user/count',
      isEvenOrOdd:"user/isEvenOrOdd"
    })
    // 方法二
    ...mapState('user',['count']),
    ...mapGetters('user',['count','isEvenOrOdd'])
  },
  methods:{
    // 方法一
    ...mapActions({
      increment:"user/increment",
      decrement:"user/decrement",
      incrementAsync:"user/incrementAsync"
    })
	// 方法二
    ...mapActions('user',['increment','decrement','incrementAsync'])
  }
}
</script>
```

