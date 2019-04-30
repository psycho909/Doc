## 快速創建一個專案
> `vue init webpack <name>` -> `npm install`
----------
## 使用於父子組件通信`$on(eventName)`和`$emit(eventName)`
----------
> `$on(eventName)`監聽事件
> `$emit(eventName)`可用於觸發事件 
>
> 父組件使用於子組件地方直接用`v-on`來監聽子組件處發的事件。然後再對應子組件方法執行處發事件，兩者缺一不可。
```html
<!-- 父组件 -->
<div id="app">
    <!-- 子组件 -->
    <!-- 父组件直接用 v-on 来监听子组件触发的事件。 -->
    <!-- 需跟子组件中的$emit组合使用 -->
    <myson v-on:son_method="father_method"></myson>
</div>
```
```javascript
// 子组件
Vue.component('myson', {
    template: '<button v-on:click="son_method">子按钮</button>',
    methods: {
        // 點擊時觸發事件
        son_method: function () {
            this.counter += 1;
            console.log("son");
            //必須跟模板中的v-on配合使用
            this.$emit('son_method');
        }
    },
});

// 父组件
new Vue({
    el: "#app",
    methods: {
        father_method: function () {
            console.log("father");
        }
    }
});
```

## 獲取當下的DOM屬性
```javascript
//可獲取dom所有屬性
method:{
    getDom:function(){
        console.log(event.target)
    }
}
//可轉成jquery
method:{
    getDom:function(){
        console.log($(event.target))
    }
}
```
## Computed使用
> `computed`使用於計算屬性
```javascript
computed:{
    tipCompu:function(value){
        return (this.something*2)*3;
    }
}
```
```html
<div class="text">{{tipCompu}}</div>
```
## Filters使用
> `filter`只是格式化並顯示在畫面上，並不會去改掉預設資料，而`computed`則是會產生一個新的屬性出來。
>
> 如果要寫複雜的`filter`函數，以`computed`寫為主
```html
{{data | filter}}
```
```javascript
filters:{
    filterName(value){
        return //thing to transform
    }
}
```
```javascript
new Vue({
    el: '#app',
        data() {
            return {
                customer1total: 100
        }
    },
    filters:{
        tip(value){
            return (value*2);
        }
    }
})
```
```Html
<!-- 會顯示200  -->
<div id="app">
    <p><strong>Total: {{ customer1total | tip }}</strong></p>
</div>
```
> 可多個`filterName()`處理
>
> `data` 會先執行 `filterA` 在執行 `filterB`
```html
{{ data | filterA | filterB }}
```
> 可傳參數`filterName(arg1,arg2)`處理
```html
{{ data | filterName(arg1,arg2)}}
```
```javascript
filters:{
    tip(value,arg1,arg2){
        return //some thing transform
    }
}
```
## Mixins使用
> 可以接受一個混合對象的數組。這些mixins可以像正常的實例對象一樣包含選項。
```javascript
var toggle = {
    data() {
        return {
        isShowing: false
        }
    },
    methods: {
        toggleShow() {
        this.isShowing = !this.isShowing;
        }
    }
}
```
```javascript  
var Modal = {
    template: '#modal',
    mixins: [toggle]
};

//相當於
var Modal = {
template: '#modal',
    data() {
        return {
        isShowing: false
        }
    },
    methods: {
        toggleShow() {
            this.isShowing = !this.isShowing;
        }
    }
};
```
## `Props`由外部傳遞內部使用
> 由`外部元件`傳入`內部元件`
```javascript
//外層元件傳入 接收方法使用v-bind
<mymain v-bind:my_listdata="listData"></mymain>
new Vue({
    el:'#app',
    data:{
        listData:[]
    }
})
// 內層(子組件)接收使用props
Vue.component('mymain',function(){
    template:'#mymain',
    porps:['my_listdata']
})
```
## `emit`由內部傳遞外部
> 一開始需要一個事件接收器<br>
> 外部元件接收
```html
<!-- updatausername 是內部向外拋出的事件
getNewName 是外部接收的方法
傳遞方法使用v-on
會取得 Chen Ching -->
<!-- 外部接收 -->
<h3>{{username}}</h3>
<mymain v-on:updata_username="get_new_name"></mymain> 
```
```javascript
data:{
    username:''
},
methods: {
    // getNewName 是接收用的事件
    getNewName (new_name) {
        let vm = this
        vm.username = new_name
    }
}
```
> 內部向外部傳的方法
```javascript
data:{
    username:'Chen Ching'
},
methods: {
updata_username () {
    let vm = this
    vm.$emit('updatausername', vm.username)
        // updatausername 是寫在 HTML 上的事件名稱
        // vm.username 是預計向外傳的變數
    }
},
```

## Vue.$set
> 假設資料結構並非一開始所定義的，可以使用 `$set` 來加入新增的屬性。
```javascript
    data:{
        info:{
            name:"chen",
            age:"100"
        }
    }
    var vm=this;
    vm.$set(vm.info,"sex","male");
    //可在info裡設置sex
```
## Vue 動態組件 :is

```html
<div id="app">
    <div class="form-group">
        <input type="radio" name="drive" id="bus" value="bus" v-model="currentView">
        <label for="bus">Bus</label>
    </div>
    <div class="form-group">
        <input type="radio" name="drive" id="bike" value="bike" v-model="currentView">
        <label for="bike">Bike</label>
    </div>
    <div class="form-group">
        <input type="radio" name="drive" id="car" value="car" v-model="currentView">
        <label for="car">Car</label>
    </div>
    <mycomponent :is="currentView"></mycomponent>
</div>
```
```javascript
new Vue({ 
    el: "#app", 
    data: { 
        currentView: 'bus' 
    },
    components:{
        bus:{template:'<div>I use Bus</div>'},
        bike:{template:'<div>I use Bike</div>'},
        car:{template:'<div>I use Car</div>'},
    }
});
```
****
## Vue <Template> </Template>
> 可做不可見的包裹元素

## Lifecycle生命週期
### `beforeCreate()`
1. 在初始化vue instance並開啟整個Lifecycle後，資料綁定與事件配置之前。目前階段還無法調用$data。
1. 此刻你無法調用 data 及 methods。
1. 應用場景：loading進頁面的事件
### `created()`
1. vue instance創建完成建立完成後。資料`$data`已可取得資料，屬性與事件也已綁定好，目前階段尚未掛載el，DOM也尚未生成。
1. 此刻已經可以調用 data, computed, methods, watch等數據或函式
### `beforeMount()`
1. 在掛載el開始之前。目前階段是相關render函式首次被調用，尚未被DOM給綁定。
### `mounted()`
1. el被剛創建好的vm.$el替換取代，並且掛載到vm上。目前階段已被DOM綁定。
1. 應用場景：對後端發出請求或讀取新資料
### `beforeUpdate()`
1. 在資料更新時調用，Virtual DOM重新render與patch之前，可以在這個階段變更資料狀態。目前階段還不會繪製view。
### `updated()`
1. DOM的更新已經完成，View被顯示在畫面上
1. 由於updated被調用時，DOM 已經更新。所以在此時更新數據很可能會導致updated無限循環的被調用。
### `beforeDestroy()`
1. 在vue instance被銷毀前調用。目前階段還可以完全使用這個vue instance。
### `destroyed()`
1. vue instance銷毀後可以調用，調用後這個vue instance底下的資料與樣板會解除綁定，事件會取消監聽，所有子元件也會被銷毀。

### 注意事項

1.  **ajax請求最好放在created裡面**，因為此時已經可以訪問this了，請求到數據就可以直接放在data裡面。
2.  這裡也碰到過幾次，面試官問：ajax請求應該放在哪個生命週期。
3.  **關於dom的操作要放在mounted裡面**，在mounted前面訪問dom會是undefined。
4.  每次進入/離開組件都要做一些事情，用什麼鉤子：

-   不緩存：

    進入的時候可以用`created`和`mounted`鉤子，離開的時候用`beforeDestory`和`destroyed`鉤子,`beforeDestory`可以訪問`this`，`destroyed`不可以訪問`this`。

-   緩存了組件：

    緩存了組件之後，再次進入組件不會觸發`beforeCreate`、`created` 、`beforeMount`、 `mounted`，**如果你想每次進入組件都做一些事情的話，你可以放在activated進入緩存組件的鉤子中**。

    同理：離開緩存組件的時候，`beforeDestroy`和`destroyed`並不會觸發，可以使用`deactivated`離開緩存組件的鉤子來代替

## 觸發鉤子的完整順序

將路由導航、`keep-alive`、和組件生命週期鉤子結合起來的，觸發順序，假設是從a組件離開，第一次進入b組件：

1.  `beforeRouteLeave`:路由組件的組件離開路由前鉤子，可取消路由離開。
2.  `beforeEach`: 路由全局前置守衛，可用於登錄驗證、全局路由loading等。
3.  `beforeEnter`: 路由獨享守衛
4.  `beforeRouteEnter`: 路由組件的組件進入路由前鉤子。
5.  `beforeResolve`:路由全局解析守衛
6.  `afterEach`:路由全局後置鉤子
7.  `beforeCreate`:組件生命週期，不能訪問`this`。
8.  `created`:組件生命週期，可以訪問`this`，不能訪問dom。
9.  `beforeMount`:組件生命週期
10.  `deactivated`: 離開緩存組件a，或者觸發a的`beforeDestroy`和`destroyed`組件銷毀鉤子。
11.  `mounted`:訪問/操作dom。
12.  `activated`:進入緩存組件，進入a的嵌套子組件(如果有的話)。
13.  執行`beforeRouteEnter`回調函數`next`。

## 比較 Filters 和 Computed
1. Filters 主要用於簡單的文字格式處理，需要在應用程式中重複使用。
1. Computed 適合較複雜的資料處理與轉換。
1. Methods 主要用來觸法狀態的改變，可能意味著會觸發 Computed 或 Filters 重新計算。
1. Filters 和 Computed 應是純粹的輸入輸出，通常不應該在此修改狀態。
## Watch vs Computed
> 雖然在大多數情況下，`Computed` 更合適，但有時仍需要使用 `Watch`。
> 當你需要響應更改的數據執行非同步或複雜的計算時，Watch 就非常有用。
## Watch

```js
watch:{
    item:{
        handel(newValue,oldValue){
            
        },
        immediate:true,
        deep:true
    }
}
```

### handle

返回變更前跟後的資料

### immediate

`immediate:true`代表如果在 wacth 裡聲明了 firstName 之後，就會立即先去執行裡面的handler方法，如果為 `false`就跟我們以前的效果一樣，不會在綁定的時候就執行

### deep

watch 裡面還有一個屬性 `deep`，默認值是 `false`，代表是否深度監聽，可以監聽``data`裡面的`obj`變化

## 事件綁定
### v-bind屬性綁定/樣式綁定
```html
<!-- src是屬性 -->
<img v-bind:src="imgNew">
<!-- isActive為true,即給 div addClass isActive-->
<div class="view" v-bind:class=["isActive"]>Class</div>
<!-- isActive為true,即給 div addClass active-->
<div class="view" v-bind:class={'active':'isActive'}>Class</div>
```
```javascript
new Vue({
    el:"#app",
    data:{
        isActive:true
    }
})
```
## v-model雙向綁定
> 单个复选框，绑定到布尔值
> 多个复选框，绑定到同一个数组
​```html
<!-- checkbox -->
<input type="checkbox" id="jack" value="jack" v-model="checkedName">
<label for="jack">jack</label>
<input type="checkbox" id="John" value="John" v-model="checkedName">
<label for="John">John</label>
<div>{{checkedName}}</div>
<!-- radio -->
<input type="radio" id="one" name="number" v-bind:value="one" v-model="picked">
<label for="one">One</label>
<input type="radio" id="two" name="number" value="two" v-model="picked">
<label for="two">Qwo</label>
<div>{{picked}}</div>
<!-- select -->
<select v-model="selected">
<option disabled value="">selected</option>
<option value="A">A</option>
<option value="B">B</option>
</select>
<span>{{selected}}</span>
```
```javascript
new Vue({
    el:"#app",
    data:{
        checkedName:[],
        checkedName:[],
        picked:'',
        selected:'',
        one:'ONE',
        msg:'Hello'
    }
})
```
### 修饰符
1. `v-model.number=""`，可以自动将用户的输入值转为数值类型
1. `v-model.trim=""`，如果要自动过滤用户输入的首尾空白字符
1. `v-model.lazy=""`，更改 input 內的值並不會馬上變更 model 的資料，而是等到滑鼠移到輸入框外，觸發 change 事件才更新
## 組件之間傳遞數據

### 父組件向子組件傳數據(1) / props
> 第一步先在子組件，props自定義屬性
```javascript
export default {
    name: 'Header-view',
    data() {
        return {
            title: 'Title'
        }
    },
    // 父組件 向 子組件傳參
    // 第一步: props
    props: {
        msg:{   // 自定義屬性
            type:String,
            default:'我是誰?'
        }
    }
}
```
> 第二步在父組件使用子組件自定義的屬性傳參
```html
<HeaderView msg="父組件向子組件傳參"></HeaderView>
```
### 父組件向子組件傳數據(2) / props
> 第一步先在子組件，props自定義屬性
```javascript
export default {
    name: 'Header-view',
    data() {
        return {
            title: 'Title'
        }
    },
    // 父組件 向 子組件傳參
    // 第一步: props
    props: {
        msg:{   // 自定義屬性
            type:String,
            default:'我是誰?'
        }
    }
}
```
> 第二步在父組件使用子組件自定義的屬性傳參
```html
<HeaderView v-bind:msg="title"></HeaderView>
```
```javascript
export default {
  name: 'App',
  components: {
    ListView,
    HeaderView
  },
  data() {
    return {
      title:'Vue入門學習'
    }
  }
}
```
### 子組件給父組件傳遞數據 / $emit
> 子組件 --> 父組件，使用自定義事件
>
> 先在子組件自定義事件

```javascript
// 先在子組件自定義事件
methods:{
	addData:function(){
		// 將輸入框數 傳遞給父組件
		// 觸發myMsg事件，並且傳遞參數
		this.$emit("myMsg",this.newStr)
	}
}
```
> 在父組件當中做一個事件的綁定
```html
<!-- 子組件向父組件傳參 -->
<!-- 在父組件當中做一個事件的綁定 -->
<ListView v-on:myMsg="getData"></ListView>
```
```javascript
methods:{
	getData(msg){
		this.msg=msg;
		console.log('App.vue: '+msg)
	}
}
```
### Vue EventBus 進行 組件間通信傳遞

```javascript
// 先創建一個 bus.js
import Vue from 'vue'
export default new Vue();

// 把bus.js 設定成全局 使用
// 進入 main.js
import Bus from './bus'
Vue.prototype.bus=bus;

========================================
// 要從 Parent 的值 傳入 Child
// 進入 Parent.vue
mounted(){
    this.bus.$emit('tomsg',"Hello msg")
}

// 進入 Child.vue
// 在頁面初始化前使用
created(){
    this.bus.$on("tomsg",function(data){
        console.log(data) // Hello msg
    })
}
```

### Vue EventBus在router間傳遞的問題

> 因為vue-router在切換時，先加載新的組件，等新的組件渲染好但是還沒掛在前，銷毀舊的組件，然後再掛載組件

在路由切換時，執行的方法依次是： 

* 新組件： beforeCreate 

* 新組件： created 

* 新組件： beforeMount 

* 舊組件： beforeDestroy 

* 舊組件： destroy 

* 新組件： mounted 

  所以，新組件只要在舊組件beforeDestroy之前，$on事件就能成功接收到。 

```js
// 接收
created(){
    this.bus.$on('something',data=>{
        consoe.log(data)
    })
}
// 傳遞
destory(){
	this.bus.$emit('something','daya')
}
```



## Vue.extend 和 Vue.component差別

1. 個人理解是：Vue.extend({})是Vue.component({})的核心，換句話說：
    1. 在實體化 Component 時會用到 Extend。
    1. Vue.component 是一個語法糖，使我們不需透過 Extend 和其他程序來實體化 Vue Instance 的子類別。
## Vue.nextTick()
> 應用場景：需要在視圖更新之後，基於新的視圖進行操作。 
>
> 可以使用Vue.nextTick取得更新後的 DOM。
```html
 <div id="app">
  <input ref="input" v-show="inputShow">
  <button @click="show">show</button>  
 </div>
```

```js
new Vue({
  el: "#app",
  data() {
   return {
     inputShow: false
   }
  },
  methods: {
    show() {
      this.inputShow = true
      this.$nextTick(() => {
        this.$refs.input.focus()
      })
    }
  }
})

```

```js
// 修改數據
vm.msg = 'Hello';
// DOM 還沒有更新
Vue.nextTick(function() {
  // DOM 更新了
});

// 作為一個 Promise 使用
Vue.nextTick().then(function() {
  // DOM 更新了
});

```



## Slot
> 基本上分為三種 slot
1. Single Slot
1. Named Slot
1. Scoped Slot
### Default Slot && Named Slot
```vue
// Child.vue
<template>
    <div>
        <nav class="nav">
			<slot name="nav"></slot>
		</nav>
		<header class="header">
			<slot name="header"></slot>
		</header>
    </div>
</template>
<script>
    export default {
        data(){
            return {
                
            }
        }
    }
</script>
```
```vue
// Parent.vue
<template>
  <div class="hello">
      <Child>
        <template #nav>
            <ul>
                <li>Home</li>
                <li>about</li>
   			</ul>
		</template>
        <template #header >
            <h1>My name is Header slot! by {{user}}</h1>
        </template>
      </Child>
  </div>
</template>
```
### Slot-Scope

>   使用`slot`插槽使`組件`可以更靈活的使用

1.  父組件在使用子組件`slot`，並在子組件內定義`template`，並定義`slot-scope`
2.  在子組件已定義的`slot`綁定`自定義屬性`
3.  `#`為`v-slot`縮寫

```vue
// child
<template>
    <div>
        <header>
            <slot name="header" :text="nameText"></slot>
            <slot :defaulttext="defaultText">Default name</slot>
        </header>
        <main>
            <slot name="main" :movie="m" v-for="m in movies"></slot>
        </main>
    </div>
</template>
<script>
    export default {
        data(){
            return {
                defaultText:"child default text",
                nameText:"child Name text",
                movies:[
                    {"name":"粽邪","country":"TW"},
                    {"name":"棒邪","country":"TW"},
                    {"name":"復仇者聯盟4","country":"USA"},
                ]
            }
        }
    }
</script>
```

```vue
// Parent.vue
<template>
  <div class="hello">
      <Child>
        <template #header="slotProps">
            <h1>{{slotProps.text}}</h1>
        </template>
        <template #default="slotProps">
            <h2>{{slotProps.defaulttext}}</h2>
        </template>
        <template #main="slotProps">
            <div>{{slotProps.movie['name']}}</div>
        </template>
      </Child>
  </div>
</template>
```

## transition動畫效果
> 淡入、淡出
```css
/*
Vue過度效果
<transition name="detail"></transition>

進入
.detail-enter           過度開始時的狀態
.detail-enter-to        過度結束的狀態
.detail-enter-active    過度的時間、延遲、曲線函數

離開
.detail-leave           過度開始時的狀態
.detail-leave-to        過度結束的狀態
.detail-leave-active    過度的時間、延遲、曲線函數
*/
.detail-enter-active,
.detail-leave-active {
    transition: 2s all;
}
.detail-enter,
.detail-leave-to{
    opacity: 0;
}
.detail-enter-to,
.detail-leave {
    opacity: 1;
}
```
## $refs綁定DOM元素
> ref屬性就是用來綁定某個dom元素或某個組件，然後再this.$ref裡面
```html
<ul class="menu-wrapper" ref="menuScroll">
    <li class="list"></li>
    <li class="list"></li>
    <li class="list"></li>
</ul>
```
```javascript
this.$refs.menuScroll.getElementsByClassName('list')
```
### ref 父組件是可以調用子組件的方法

> 子組件

```javascript
// 子組件
methods:{
    // 父組件是可以調用子組件的方法
    showView(){
        this.showFlag=true;
    }
}
```

> 父組件

```html
<child ref="childView"></child>
```

```javascript
// 父組件
methods:{
    showDetail(){
        this.$refs.childView.showView();
    }
}
```



## 使用擴展套件時或需要獲取DOM元素的注意事項

> 例如better-scroll
```javascript
created(){
    // 調用滾動的初始化方法
    // this.initScroll() //無法調用
    // 開始時,DOM元素還沒有渲染,即高度是問題
    // 在獲取到數據後，並DOM已經被渲染，表示列表高度是沒問題的
}
```
## vm.$nextTick()
> 將回調延遲到下次DOM更新循環之後執行
```javascript
created(){
    //nextTick()
    that.$nextTick(()=>{
        // DOM已經更新
        that.initScroll()
    })
}
```
## Vue.set(target,key,value) 設置新的字段
```javascript
// this.food 裡面沒有 count
Vue.set(this.food,'count',1);
```
## 數組改變監測

### 替換方法

- push()
- pop()
- shift()
- unshift()
- splice()
- sort()
- reverse()

這些方法會改變原來的 array，並自動觸發 view 的更新。

### 替換array

- filter()
- concat()
- slice()

## keep-alive

> 包裹住動態組件時，會緩存不活動的組件實力，而不是銷毀他們
## data為什麼畫面沒有隨資料更新
1. 方法一：利用 splice 達到響應式變化
1. 方法二：利用 vm.$set 達到響應式變化
### 方法一:使用 Vue 可觀察到的陣列方法
> push()、pop()、shift()、unshift()、splice()、sort()、reverse()
### 方法二：使用 vm.$set
```javascript
methods: {
    changeAnimal () {
        // 利用 vm.$set(array, index, value) 方法
        this.$set(this.animals, 0, this.animal)
    }
}
```
## Vue.use()

>   自定義一個需要 Vue.use() 的組件，也就是有 install 的組件

### 在 Loading.vue 中定義一個組件

```vue
<template>
    <div class="loading-box">
        Loading...
    </div>
</template>
```

### 在 index.js 中 引入 Loading.vue ，並導出

```js
// 引入組件
import LoadingComponent from './loading.vue'
// 定義 Loading 對象
const Loading={
    // install 是默認的方法。當外界在 use 這個組件的時候，就會調用本身的 install 方法，同時傳一個 Vue 這個類的參數。
    install:function(Vue){
        Vue.component('Loading',LoadingComponent)
    }
}
// 導出
export default Loading
```

### 在 main.js 中引入 loading 文件下的 index

```js
// 其中'./components/loading/index' 的 /index 可以不寫，webpack會自動找到並加載 index 。如果是其他的名字就需要寫上。
import Loading from './components/loading/index'
// 這時需要 use(Loading)，如果不寫 Vue.use()的話，瀏覽器會報錯，大家可以試一下
Vue.use(Loading)
```

### App.vue裡面寫入定義好的組件標籤

```vue
<template>
  <div id="app">
    <h1>vue-loading</h1>
    <Loading></Loading>
  </div>
</template>
```



## Vue Router

### 子路由
```html
<!-- Hi template -->
<template>
    <div>
        <h2>{{ msg }}</h2>
        <router-view></router-view>
    </div>
</template>
<script>
    export default {
        name:'hi',
        data(){
            return {
                msg:'Hi Page'
            }
        }
    }
</script>
```
```html
<template>
    <div>
        <h2>{{ msg }}</h2>
    </div>
</template>
<script>
    export default {
        name:'hi',
        data(){
            return {
                msg:'Hi-1 Page'
            }
        }
    }
</script>

```
```javascript
{
    path:'/Hi',
    name:'Hi',
    component:Hi,
    children:[
        {path:'/',component:Hi},
        {path:'Hi1',component:Hi1},
        {path:'Hi2',component:Hi2},
    ]
}
```
### 參數傳遞
> `name`要與router裡面配置的name一樣，`params`則是傳遞的參數
```html
<!-- $route $=>只要是在Vue組件內引用都可以使用$去引用 -->
<router-link :to="{name:'hi1',params:{username:'Chen'}}">Hi-1</router-link>
```
```html
<!-- hi template -->
<h2>{{ msg }} - {{ $route.params.username }}</h2>
```
### 單頁面多router區域
```html
<router-view></router-view>
<router-view name="left"></router-view>
<router-view name="right"></router-view>
```
```javascript
{
    path: '/',
    name: 'Count',
    // 多個頁面的component 要變成 components
    components: {
        default:Count,
        left:Left,
        right:Right
    }
}
```
### URL傳遞參數
```html
<router-link to="/Params/198/tktasdsadwqe">Params</router-link>
```
```javascript
{
    path:'/params/:newsId/:newsTitle',
    component:Params
}
```
```html
<p>newId {{ $route.params.newsId }}</p>
```
### redirect重定向 / redirect重定向並傳參數
```html
<router-link to="/GoHome">GoHome</router-link>
<router-link to="/goParams/20181023/GoGOGOGOGOG">GoHome</router-link>
```
```javascript
{
    path:'/GoHome',
    redirect:'/'
},{
    path:'/goParams/:newsId/:newsTitle',
    redirect:'/params/:newsId/:newsTitle'
}
```
### 別名方式重新定向 alias
```html
<router-link to="/Hi1">Go Hi1</router-link>
<router-link to="/jspang">Go jspang</router-link>
```
```javascript
{
    path:'/hi1',
    component:Hi1,
    alias:'/jspang'
}
```
> 小坑，不能在首頁使用alias
### router 切換的動畫效果
```html
<transition name="fade" mode="out-in">
    <router-view></router-view>
</transition>
```
```css
.fade-enter{
    opacity: 0px;
}
.fade-leave{
    opacity: 1px;
}
.fade-enter-active{
    transition:all .3s;
}
.fade-leave-active{
    opacity: 0;
    transition:all .3s;
}
```
### mode的作用和404頁面處理
#### mode的作用

1. mode:'history' -> 可以把/#/去掉
1. mode:'hash' -> 可以增加/#/
#### 404頁面處理

```html
<!-- 404 Page -->
<template>
    <div>{{msg}}</div>
</template>
<script>
    export default {
        data() {
            return {
                msg: '404 page'
            }
        }
    }
</script>

```
```javascript
{
    path:'*',
    component:Error
}
```
### router鉤子函數
> 進入路由之前 & 離開路由之前
1. 在路由JS配置
```javascript
// 在路由JS配置只能使用beforeEnter
{
    path:'/params/:newsId/:newsTitle',
    component:Params,
    beforeEnter:(to,from,next)=>{
        // 此設置可以在路由進入之前設置一個防線
        console.log(to)
        console.log(from)
        next() // 允許跳轉
        // next({path:'/'}) // 跳轉道首頁
    }
}
```
2. 在模板JS配置
```javascript
<script>
    export default {
        data() {
            return {
                msg: 'Params page'
            }
        },
        beforeRouteEnter:(to,from,next)=>{
            console.log('準備進入路由')
            next()
        },
        beforeRouteLeave:(to,from,next)=>{
            console.log('準備離開路由')
            next()
        }
    }
</script>
```
### 編程式導航
> 可用於判斷用戶名是否正確再用methods去處理
```html
<button @click="goBack">後退</button>
<button @click="goGo">前進</button>
<button @click="goHome">返回首頁</button>
```
```javascript
methods: {
    goBack(){
        this.$router.go(-1)
    },
    goGo(){
        this.$router.go(1)
    },
    goHome(){
        this.$router.push('/')
    }
}
```

## export引入

```js
// xxx.js
var x1=a;
var x2=b;
export {x1,x2}

// xxx.vue
import {x1,x2} form ./xxx.js
```



## Vue bulid後的問題

### 增加自定義路徑

```js
// build/webpack.base.conf.js
resolve:{
    extensions: ['.js', '.vue', '.json'],
        alias:{
            'static':path.resolve(__dirname,'../static')
        }
}
```

### 開發前後使用API位置

```js
// config
// dev.env.js dev時
API_PATH:'"http://localhost/brand/dist/data/brand_api.php"'
// prod.env.js build後
API_PATH:'"../event/api/brand_api.php"'
```

> 使用

```js
var api=process.env.API_PATH;
```



### Vue使用SASS

```npm
npm i --save-dev node-sass sass-loader
```

```html
// 在頁面使用
<style lang="scss">
    
</style>
```



### Vue postcss設定

> 1. 在` .postcssrc.js`設定

```js
// .postcssrc.js
// 增加
"autoprefixer": {
    browsers:[
        "> 1%",
        "last 5 versions",
        "Firefox >= 45",
        "ios >= 8",
        "Safari >= 8",
        "ie >= 8"
    ]
}
```

> 2. 在`package.josn`設定

```js
// 增加 autoprefixer 會自動去讀取
"browserslist": [
    "> 1%",
    "last 5 versions",
    "Firefox >= 45",
    "ios >= 8",
    "Safari >= 8",
    "ie >= 8"
]
```

### 增加對IE 11支持

#### 1.

```js
// 安裝
npm i --save babel-polyfill
// main.js
import 'babel-polyfill'
```

#### 2.

```js
// 安裝
npm i --save @babel/polyfill
// main.js
import '@babel/polyfill';
```

```js
const plugins = [];

module.exports = {
  presets: [["@vue/app",{"useBuiltIns": "entry"}]],
  plugins: plugins
};
```



### POSTCSS失效問題

```js
// build/webpkac.prod.config.js
// 把 OptimizeCSSPlugin 相關的註解調

// OptimizeCSSPlugin 功能
//壓縮提取出來的CSS，並且進行css的復用以解決extract-text-webpack-plugin將css處理後會出現的css重複的情況

```



### 路徑問題

```javascript
// build/utils
if (options.extract) {
    return ExtractTextPlugin.extract({
        use: loaders,
        fallback: 'vue-style-loader',
        // 增加這行
        publicPath:"../../"
    })
} else {
    return ['vue-style-loader'].concat(loaders)
}

// config/index
build:{
    // 更改
    assetsPublicPath:"./"
}
```

## 使用axios post到PHP問題

> 默認情況下,js將JavaScript對象序列化為JSON。要以應用程序/ x-www-form-urlencoded格式發送數據,您可以使用以下選項之一。

```js
// 第一種
// 使用 URLSearchParems() API但會遇到版本問題，需使用babel-polyfill
var data=new URLSearchParams();
data.append('name',vm.name)
data.append('email',vm.email)
this.axios.post(api,data)
.then((data)=>{
    console.log(data)
})
.catch((error)=>{
    console.log(error)
})

// 第二種
// 使用 qs.js
// 作用是能把json格式的直接轉成data所需的格式
npm i --save qs.js
// main.js
import qs from 'qs'
Vue.prototype.qs=qs
// ajax
data:{
    this.qs.stringify({
		name:vm.name
    })
}
```

## proxyTable

### Vue Axios跨域問題CROS

> ### 修改前

```js
  this.axios.get('http://data.taipei/youbike').then((response) => {
    console.log(response.data)
  })
```

### Vue cli 2.x

> 修改`config/index.js`

```js
proxyTable: {
  '/api': {                        // 自訂 local 端的位置
    target: 'http://jsonplaceholder.typicode.com',  // 遠端 URL Domain
    changeOrigin: true,
    pathRewrite: {
      '^/api': ''
    }
  }
},
```

### Vue cli 3.x

> 在`vue.config.js`配置

```js
devServer:{
	proxy:{
            '/api':{
                target:"http://jsonplaceholder.typicode.com",
                changeOrigin:true,
                pathRewrite:{
                    '^/api':''
                }
            }
        }
}
```

### 修改後

經過這樣設定後，`http://jsonplaceholder.typicode.com/todos/1` 網域內的資源，都會在 local 端以 `/api` 的形式被代理，也就是說，像 `http://data.taipei/youbike` 這樣的遠端資源，我們就可以在 local 端用 `/api/todos/1` 來取得。

所以，再回到 `App.vue`，這裏將原本的 `http://jsonplaceholder.typicode.com/todos/1` 改成 `/api/todos/1`，像這樣:

```js
  this.axios.get('/api/todos/1').then((response) => {
    console.log(response.data)
  })
```

### 範例

> `https://pool.viabtc.com/user/api/e573c6ac371821077c0fded1cb5d3fdc/`

```js
devServer:{
        proxy:{
            '/user':{
                target:"https://pool.viabtc.com/",
                secure:false,
                changeOrigin:true,
                pathRewrite:{
                    '^/user':''
                }
            }
        }
    }
```

```js
var api="/user/user/api/e573c6ac371821077c0fded1cb5d3fdc/";
			this.axios.get(api).then((res) => {
				console.log(res)
			})
```

## Vue Cli 3.x

## 建立`vue.config.js`

在根目錄自行建立`vue.config.js`

```JS
module.expors={
    
}
```



## 路徑修改

```js
module.expors={
    baseUrl:'./', // 檔案路徑修改
    productionSourceMap:false, // 不產生map檔案
}

```

## `env`使用

在根目錄建立:

1.  開發用:`.env.development`
2.  生產用:`.env.production`

```js
// 名稱一定得命名 VUE_APP_XXX
VUE_APP_API=../api/brand_api.php
```

```js
// 使用
process.env.VUE_APP_API
```

### proxy

```vue
// 在vue.config.js建立
deServer:{
    proxy:{
        '/api':{
            target:"",
            changeOrigin:true,
            pathRewrite:{
            	"^/api":""
            }
        }
    }
}
```

### 修复 HMR(热更新)失效

```js
module.exports = {
    chainWebpack: config => {
        // 修复HMR
        config.resolve.symlinks(true);
    }
}
```



## Vue優化方式

1.  ### 使用v-if代替v-show

2.  ### v-for必須加上key，並避免同時使用v-if

    1.  為了過濾一個列表中的項目 比如 `v-for="user in users" v-if="user.isActive"`。在這種情形下，請將 `users`替換為一個計算屬性 (比如`activeUsers`)，讓其返回過濾後的列表
    2.  為了避免渲染本應該被隱藏的列表 比如 `v-for="user in users" v-if="shouldShowUsers"`。這種情形下，請將 `v-if` 移動至容器元素上 (比如 ul, ol)

3.  ### 異步路由

    1.  使用異步路由可以根據URL自動加載所需頁面的資源，並且不會造成頁面阻塞，較適用於移動端頁面，建議主頁面直接import，非主頁面使用異步路由

        ```vue
        {
          path: '/order',
          component: () => import('./views/order.vue')
        }
        ```

4.  ### 異步組件

    ```vue
    <template>
      <div>
        <HellowWorld v-if="showHello" />
      </div>
    </template>
    <script>
    export default {
      components: { HellowWorld: () => import('../components/HelloWorld.vue') },
      data() {
        return {
          showHello: false
        }
      },
      methods: {
        initAsync() {
          addEventListener('scroll', (e) => {
            if (scrollY > 100) {
              this.showHello = true
            }
          })
        }
      }
    }
    </script>
    
    ```


