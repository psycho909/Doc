# Vue 使用
1. [`on()`&`emit()`](#emit)
1. [獲取當下DOM屬性](#domattr)
1. [`Computed`使用](#computed)
1. [Filters使用](#filters)
1. [Mixins使用](#mixins)
1. [Props使用](#props)
1. [`emit`使用](#emit)
1. [Vue.$set](#set)
1. [Vue Slot使用](#slot)
1. [Vue 動態組件 :is](#is)
## 快速創建一個專案
> `vue init webpack <name>` -> `npm install`
----------
## <span id="emit">使用於父子組件通信`$on(eventName)`和`$emit(eventName)`</span>
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

## <span id="domattr">獲取當下的DOM屬性</span>
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
## <span id="computed">Computed使用</span>
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
## <span id="filters">Filters使用</span>
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
## <span id="mixins">Mixins使用</span>
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
## <span id="props">`Props`由外部傳遞內部使用</span>
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
## <span id="emit">`emit`由內部傳遞外部</span>
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

## <span id="set">Vue.$set</span>
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

## <span id="slot">Vue slot使用</span>
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
## <span id="is">Vue 動態組件 :is</span>
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