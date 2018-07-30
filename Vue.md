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

## Vue slot使用
> 使用`slot`插槽使`組件`可以更靈活的使用
```html
<!-- Child -->
<template id="content">
    <main>
        <h1>TiTLE</h1>
        <p>Content</p>
        <!-- 定義slot name -->
        <!-- 使用v-bind綁定 一個自定義屬性item -->
        <slot name="slot-content" :item="val" v-for="val in artlist"></slot>
    </main>
</template>
<!-- Parent -->
<template id="container">
    <div class="container">
        <content>
            <!-- 自定義scope -->
            <!-- 從Child的slot name 獲取 slot -->
            <template scope="props" slot="slot-content">
                <article>
                    <div>{{props.item.title}}</div>
                </article>
            </template>
        </content>
    </div>
</template>
```
```javascript
Vue.component('content',{
    template:'#content',
    data:function(){
        return {
            artlist:[
                {title:'Firefox'},
                {title:'Edge'},
                {title:'IE'}
            ]
        }
    }
})
Vue.component('container',{
    template:'#container
})
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
## Notice
> 在IE11以下 `data(){}` 需寫成 `data:function(){}`

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

## 比較 Filters 和 Computed
1. Filters 主要用於簡單的文字格式處理，需要在應用程式中重複使用。
1. Computed 適合較複雜的資料處理與轉換。
1. Methods 主要用來觸法狀態的改變，可能意味著會觸發 Computed 或 Filters 重新計算。
1. Filters 和 Computed 應是純粹的輸入輸出，通常不應該在此修改狀態。
## Watch vs Computed
> 雖然在大多數情況下，`Computed` 更合適，但有時仍需要使用 `Watch`。
> 當你需要響應更改的數據執行非同步或複雜的計算時，Watch 就非常有用。
## v-on事件綁定
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
```html
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
### v-model修饰符
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
## Vue.extend 和 Vue.component差別
1. 個人理解是：Vue.extend({})是Vue.component({})的核心，換句話說：
    1. 在實體化 Component 時會用到 Extend。
    1. Vue.component 是一個語法糖，使我們不需透過 Extend 和其他程序來實體化 Vue Instance 的子類別。
## Vue.nextTick
> 可以使用Vue.nextTick取得更新後的 DOM。
## Slot
> 基本上分為三種 slot
1. Single Slot
1. Named Slot
1. Scoped Slot
### Single Slot
```html
<my-component>
    <div>This is not slot</div>
</my-component>
<template id="signle-slot">
    <div class="box">
        <slot>Hello from child</slot>
    </div>
</template>
```
```javascript
Vue.component('my-component',{
    template:'#my-component'
})
```
### Named Slot
```html
<name-slot>
    <div slot="header">HHHeader</div>
    <div slot="footer">FFFooter</div>
</name-slot>
<template id="name-slot">
    <div class="box">
        <slot name="header">Header</slot>
        <slot>This is content</slot>
        <slot name="footer">Footer</slot>
    </div>
</template>
```
```javascript
Vue.component('name-slot',{
    template:'#name-slot'
})
```
### Scoped Slot
```html
<div id="app">
    <scope-slot v-bind:items="items">
        <template scope="props" slot="item">
            <span>{{props.text}}</span>
        </template>
    </scope-slot>
</div>
<template id="scope-slot">
    <div class="box">
        <h1>Scope Slot</h1>
        <p>內容</p>
        <slot name="item" v-for="item in items" v-bind:text="item.text"></slot>
    </div>
</template>
```
```javascript
Vue.component('scope-slot',{
    props:['items'],
    template:'#scope-slot'
})
var app=new Vue({
    el:'#app',
    data:{
        items: [
            { id: 1, text: '項目 1' },
            { id: 2, text: '項目 2' },
            { id: 3, text: '項目 3' }
        ]
    }
})
```
### Slot-Scope
```html
<ul>
    <scope-slot v-bind:items="itemsList">
        <li slot-scope="props" slot="item">{{props.id}}.{{props.text}}</li>
    </scope-slot>
</ul>
<template id="scope-slot">
    <div class="box">
        <h1>Scope Slot</h1>
        <p>內容</p>
        <slot name="item" v-for="item in items" v-bind:text="item.text" v-bind:id="item.id"></slot>
    </div>
</template>
```
```javascript
Vue.component('scope-slot',{
    props:['items'],
    template:'#scope-slot'
})
var app=new Vue({
    el:'#app',
    data:{
        itemsList: [
            { id: 1, text: '項目 1' },
            { id: 2, text: '項目 2' },
            { id: 3, text: '項目 3' }
        ]
    }
})
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
### 父組件是可以調用子組件的方法
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
## keep-alive
> 包裹住動態組件時，會緩存不活動的組件實力，而不是銷毀他們
## data為什麼畫面沒有隨資料更新
1. 方法一：利用 splice 達到響應式變化
1. 方法二：利用 vm.$set 達到響應式變化
### 方法一:使用 Vue 可觀察到的陣列方法
> push()、pop()、shift()、unshift()、splice()、sort()、reverse()
## 方法二：使用 vm.$set
```javascript
methods: {
    changeAnimal () {
        // 利用 vm.$set(array, index, value) 方法
        this.$set(this.animals, 0, this.animal)
    }
}
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
> mode的作用
1. mode:'history' -> 可以把/#/去掉
1. mode:'hash' -> 可以增加/#/
> 404頁面處理
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
## Vue EventBus 進行 組件間通信傳遞

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
mounte(){
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



## Vue postcss設定

> 1.  在` .postcssrc.js`設定

```javascript
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

## Vue bulid後的問題

### 增加對IE 11支持

```js
// 安裝
npm i --save babel-polyfill
// main.js
import 'babel-polyfill'
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

