# VueRouter

## 嵌套路由和編程式導航

### 設定嵌套

```js
// route.js
{
      path: '/about/:id',
      name: 'about',
      component:()=>import('./views/About.vue'),
      children:[
        {name:"cookBook",path:"cook_book",component:()=>import('./views/CookBook.vue')}
      ]
}
```

### 1.router-link 導航

```vue
// About.vue
<template>
  <div class="about">
    <h1>{{$route.params}}</h1>
    <span>{{this.allAbout}}</span>
    <div>
      <router-link :to="{name:'cookBook'}">查看食譜</router-link>
    </div>
    <router-view></router-view>
  </div>
</template>
```

### 2.編程式導航

>   通過 '<button @click='jumpTo'>查看食譜' 跳轉

```vue
methods: {
    jumpTo(){
        //通過$router來跳轉
        this.$router.push({name:'cookBook'})
    }
},
```

## 路由重定向

>   重定向也是通過 `routes` 配置來完成

```js
{name:'default',path:'*',redirect: '/index'},
```

>   重定向的目標也可以是一個命名的路由：

```js
{name:'default',path:'*',redirect: {name:'index'}}
```

>   甚至是一個方法，動態返回重定向目標：

```js
{ path: '/dynamic-redirect/:id?',
      redirect: to => {
        const { hash, params, query } = to
        if (query.to === 'foo') {
          return { path: '/foo', query: null }
        }
        if (hash === '#baz') {
          return { name: 'baz', hash: '' }
        }
        if (params.id) {
          return '/with-params/:id'
        } else {
          return '/bar'
        }
      }
    }
```



## this.$router && this.$route 

### $router

1.  meta
2.  跳頁功能

### $route

1.  params
2.  query
3.  fullPath
4.  path

## 動態路由

```html
<router-link :to="{path:'home',params:{userId:'name'}},query:{id:2}">Home</router-link>
```

>   http://192.168.66.112:8080/#/about/welcome/bbb?id=2

```html	
$route.query.id
$route.params:userId
```

```js
{
    path:"/about",
    name:"about",
    component:About,
    children:[
        {
            path:"welcome/:userId",
    		name:"welcome",
    		component:Welcome
        }
    ]
}
```

## 路由Meta資料

>   可以透過自定義Meta資料作為過濾路由時識別使用

```js
{
    path:"/about",
    name:"about",
    component:About,
    meta:{
		requiresAuth:true
    },
    children:[
        {
            path:"welcome/:userId",
    		name:"welcome",
    		component:Welcome
        }
    ]
}
```

```js
// 全局使用beforeEach

router.beforeEach((to,from,next)=>{
    if(to.matched.some(record)=>record.meta.requiresAuth){
        let isLogin=true
        console.log("check auth")
        if(isLogin == false && from.path != '/login'){
            next({
                path:'/login',
                query:{redirect:to.fullPath}
            })
        }else{
            next()
        }
    }else{
        next()
    }
})
```





### $router.push() && $router.replace() && $router.go()

### $router.push()

>   可達到跳頁功能

```js
this.$router.push('home')
this.$router.push({name:'home'})
```

### router.replace(location) = = = window.history.replaceState

### $router.go(n) = = = window.history.go

## Vue-Router導航守衛

有的時候，我們需要通過路由來進行一些操作，比如最常見的登錄權限驗證，當用戶滿足條件時，才讓其進入導航，否則就取消跳轉，並跳到登錄頁面讓其登錄。

為此我們有很多種方法可以植入路由的導航過程：**全局的, 單個路由獨享的, 或者組件級**

## 響應路由參數的變化

beforeRouteUpdate

```vue
export default {
  data(){
    return {
      allAbout:"1"
    }
  },
  mounted(){
    // console.log(this.$route.params.id)
  },
  watch:{
	//to表示你將要去的路由對象, from表示你從哪個路由來
    '$route'(to,from){
      if(to.params.id == 1){
        this.allAbout="第1頁"
      }
      if(to.params.id == 2){
        this.allAbout="第2頁"
      }
    }
  }
}
```

或者使用 2.2 中引入的 `beforeRouteUpdate` [導航守衛](https://link.juejin.im/?target=https%3A%2F%2Frouter.vuejs.org%2Fzh%2Fguide%2Fadvanced%2Fnavigation-guards.html)：

```vue
export default {
  data(){
    return {
      allAbout:"1"
    }
  },
  mounted(){
    // console.log(this.$route.params.id)
  },
  beforeRouteUpdate(to,from,next){
    if(to.params.id == 1){
      this.allAbout="第1頁"
    }
    if(to.params.id == 2){
      this.allAbout="第2頁"
    }
    
    next()
  }
}
```

## 全局守衛

>   全局守衛，每次使用router時都會調用

1.  router.beforeEach 全局前置守衛 進入路由之前
2.  router.beforeResolve 全局解析守衛(2.5.0+) 在beforeRouteEnter調用之後調用
3.  router.afterEach 全局後置鉤子 進入路由之後

### beforeEach

>   ## 切換必做 beforeEach，在切換頁面的某個時機點，會做一次

```js
const router = new VueRouter({ ... })

router.beforeEach(async (to, from, next) => {
  //...
  switch(to.name) {
    case 'news':
      await router.app.$store.dispatch('API', '/news')
      break
    case 'products':
      await router.app.$store.dispatch('API', '/products')
      break
    //...
  }
  next()
})
```



### **使用方法** 

```js
// main.js
import router from './router'
router.beforeEach((to,from,next)=>{
    next()
})
router.beforeResolve((to, from, next) => {
  	next();
});
router.afterEach((to, from) => {
  	console.log('afterEach 全局後置鉤子');
});
```

### to,from,next 這三個參數：

>   to和from是**將要進入和將要離開的路由對象**,路由對象指的是平時通過this.$route獲取到的路由對象。 

### **next:Function** 這個參數是個函數，且**必須調用，否則不能進入路由**(頁面空白)。 

-   next() 進入該路由。
-   next(false): 取消進入路由，url地址重置為from路由地址(也就是將要離開的路由地址)。
-   next 跳轉新路由，當前的導航被中斷，重新開始一個新的導航。

我們可以這樣跳轉：next('path地址') 或者 next({path:''}) 或者 next({name:''}) 且允許設置諸如 replace: true、name: 'home' 之類的選項 以及你用在router-link或router.push的對象選項。 

## 路由獨享守衛

>   如果你不想全局配置守衛的話，你可以為某些路由單獨配置守衛： 

```js
const router = new VueRouter({
  routes: [
    {
      path: '/foo',
      component: Foo,
      beforeEnter: (to, from, next) => {
        // 參數用法什麼的都一樣,調用順序在全局前置守衛後面，所以不會被全局守衛覆蓋
        // ...
      }
    }
  ]

})
```
## 路由組件內的守衛

1.  beforeRouteEnter 進入路由前
2.  beforeRouteUpdate (2.2) 路由復用同一個組件時
3.  beforeRouteLeave 離開當前路由時

### 使用方法

```js
beforeRouteEnter (to, from, next) {
  // 在路由獨享守衛後調用 不！能！獲取組件實例 `this`，組件實例還沒被創建
},

beforeRouteUpdate (to, from, next) {
  // 在當前路由改變，但是該組件被覆用時調用 可以訪問組件實例 `this`
  // 舉例來說，對於一個帶有動態參數的路徑 /foo/:id，在 /foo/1 和 /foo/2 之間跳轉的時候，
  // 由於會渲染同樣的 Foo 組件，因此組件實例會被覆用。而這個鉤子就會在這個情況下被調用。
},

beforeRouteLeave (to, from, next) {
  // 導航離開該組件的對應路由時調用，可以訪問組件實例 `this`
}
```

### **beforeRouteEnter訪問this** 

因為鉤子在組件實例還沒被創建的時候調用，所以不能獲取組件實例 this，可以通過傳一個回調給next來訪問組件實例 。

但是**回調的執行時機在mounted後面**,所以在我看來這裡對this的訪問意義不太大，可以放在created或者mounted裡面。

```js
beforeRouteEnter (to, from, next) {
console.log('在路由獨享守衛後調用');
  next(vm => {
    // 通過 `vm` 訪問組件實例`this` 執行回調的時機在mounted後面，
  })
}
```

### **beforeRouteLeave用法**

導航離開該組件的對應路由時調用，我們用它來禁止用戶離開，比如還未保存草稿，或者在用戶離開前，將setInterval銷毀，防止離開之後，定時器還在調用。 

```js
beforeRouteLeave (to, from , next) {
  if (文章保存) {
    next(); // 允許離開或者可以跳到別的路由 上面講過了
  } else {
    next(false); // 取消離開
  }
}
```

## 完整的路由導航解析流程(不包括其他生命週期)：

1.  觸發進入其他路由。
2.  調用要離開路由的組件守衛beforeRouteLeave
3.  調用局前置守衛：beforeEach
4.  在重用的組件裡調用 beforeRouteUpdate
5.  調用路由獨享守衛 beforeEnter。
6.  解析異步路由組件。
7.  在將要進入的路由組件中調用beforeRouteEnter
8.  調用全局解析守衛 beforeResolve
9.  導航被確認。
10.  調用全局後置鉤子的 afterEach 鉤子。
11.  觸發DOM更新(mounted)。
12.  執行beforeRouteEnter 守衛中傳給 next 的回調函數