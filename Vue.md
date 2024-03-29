#vue #js
## class 與 style

### class

#### example1 使用對象

```vue
// isText is true class is txt
// 數組內使用合併使用Obj格式
// activeClass:"active"
// 三元運算符

<div :class="[{ txt: isText },activeClass,isText?center:'']">{{txt}}</div>

// HTML
<div class="txt active center">123</div>
```

```js
data(){
    return {
        isText:true,
        activeClass:"active",
        center:"center"
    }
}
```

```css
.txt{
  color:red;
}
.active{
  border-bottom:1px solid blue;
}
.center{
  margin: 0 auto;
}
```

#### example2 使用條件判斷

```html
<div class="box" :class="bg ? 'red' : 'green'">This is how you add static classess</div>
```

```js
data(){
    return {
        bg:true
    }
}
```

#### example3 使用數組

```html
<div :class="['box',bg ? 'red' : 'green']">This is how you add static classess</div>
```

#### example4 使用計算屬性

```html
<div class="box" :class="bg">背景</div>
```

```js
computed:{
    bg(){
    	return this.darkMode?"dark-theme":"light-theme"
    }
},
```



### style

```vue
<div :style="[baseStyles,marginBottom]">{{txt}}</div>

// HTML
<div style="width: 100px; margin-bottom: 24px;">123</div>
```

```js
data(){
    return {
        baseStyles:{
            width:"100px"
          },
          marginBottom:{
            marginBottom:"24px"
          }
    }
}
```

## 表單控制

### 1.單選框

```vue
<div class="radio-group">
    <input type="radio" name="sex" v-model="add" value="男">男
    <input type="radio" name="sex" v-model="add" value="女">女
    <h1>{{add}}</h1>
</div>
```

```js
data(){
    return {
        add:""
    }
}
```

### 2.多選框

#### 單選checkbox

```vue
<div class="checkbox-group">
    <input type="checkbox" v-model="agree">
    <div>{{ag()}}</div>
</div>
```

```js
data(){
    return {
        agree:""
    }
},
methods:{
    ag(){
        return this.agree?"同意":"不同意"
    }
}
```
#### 多選checkbox 使用數組

```vue
<div class="checkbox-group2">
    <template v-for="hobby in hobbys">
<input type="checkbox" v-model="info" :value="hobby.value">{{hobby.name}}
    </template>
    <p>{{info}}</p>
</div>
```

```js
data(){
    return {
        info:[],
        hobbys:[
          {name:"讀書",value:"book"},
          {name:"唱歌",value:"song"},
          {name:"跑步",value:"run"},
          {name:"遊戲",value:"game"}
        ]
    }
}
```

### 3.選擇select

```vue
<div id="example-5">
    <select v-model="selectInfo">
        <option disabled value="請選擇">請選擇</option>
        <option v-for="o in options" :value="o">{{o}}</option>
    </select>
    <span>Selected: {{ selectInfo }}</span>
</div>
```

```js
data(){
    return {
        options:["角色1","角色2","角色3"],
        selectInfo:"請選擇"
    }
}
```



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
### getter

```vue
computed:{
    tipCompu:{
		get(){
			return (this.something*2)*3;
		}
	}
}
```

### setter

```vue
<div id="demo">
    <p> {{ fullName }} </p>
    <input type="text" v-model="fullName">
    <input type="text" v-model="firstName">
    <input type="text" v-model="lastName">
</div>
```

```js
data: {
    firstName: 'zhang',
    lastName: 'san'
  },
  computed: {
    fullName: {
      //getter 方法
        get(){
            console.log('computed getter...')
            return this.firstName + ' ' + this.lastName
        }，
   //setter 方法
        set(newValue){
            console.log('computed setter...')
            var names = newValue.split(' ')
            this.firstName = names[0]
            this.lastName = names[names.length - 1]
            return this.firstName + ' ' + this.lastName
        }
      
    }
  },
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
## 客製組件的 `v-model`

在子組件的實體中設定 `model` 物件，這個物件有兩個屬性 `prop` 及 `event` :

- `prop` : `v-model` 目標屬性。
- `event` : `v-model` 監聽的事件。

設定好 `model` 的 `prop` 及 `event` 後，就可以依照原生的 `checkbox` 勾選值去改變 `model`值。

```js
Vue.component('base-checkbox', {
  model: {
    prop: 'checked', // 預設為 value
    event: 'change' // 預設為 input
  },
  props: ['checked', 'label'], // 跟 value 一樣， v-model 的 prop : cheked 要設定在 props 中
  template: `
    <label>
      <input
        type="checkbox"
        :checked="checked"
        @change="$emit('change', $event.target.checked)"
      >
      {{label}}
    </label>
  `
});
```



## `Props`由外部傳遞內部使用

> 由`外部元件`傳入`內部元件`
#### 一般使用

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
#### 推薦使用

```html
<Btn class="btn" txt="按鈕"></Btn>
```

```html
<template id="Btn">
    <button>
        {{txt}}
    </button>
</template>
```

```js
Vue.component("Btn",{
    template:"#Btn",
    props:{
        txt:{
            type:String,
            required:true,
            validator:function(value){
                return ["按鈕"].indexOf(value) !== -1
            }
        }
    }
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

## Vue.$set(目標,key,value)
> 假設資料結構並非一開始所定義的，可以使用 `$set` 來加入新增的屬性。
```javascript
    data:{
        info:{
            name:"chen",
            age:"100"
        }
    }
    this.$set(this.info,"sex","male");
	Vue.set(this.info,"sex","male")
    //可在info裡設置sex
```
## `$attrs`和`$listenter`

平常在實作 Vue 組件之間的資料傳遞大部分都是透過 `props` 及 `$emit`，或是直接經由 Vuex 進行狀態管理，而除了這兩種方法，還有另外一種做法是透過 `$attrs` 及 `$listenter`。

### $attrs

当一个组件没有声明任何 **prop** 时，这里会包含所有父作用域的绑定 (class 和 style 除外)，并且可以通过 **v-bind="$attrs"** 传入内部组件——在创建高级别的组件时非常有用。

```vue
    <div id="app">
        <test2 :data1="data1" :data2="data2"></test2>
    </div>

    <template id="test2">
        <div class="test">
            {{data1}} - {{$attrs}}
        </div>
    </template>
```

```js
        Vue.component("test2",{
            template:"#test2",
            props:['data1'],
            mounted(){
                this.$emit("event1")
            }
        })
        new Vue({
            el:"#app",
            data:{
                data1:"Hello World1",
                data2:"Hello World2"
            }
        })
```



### $listeners

父組件可以透過 `$listeners` 取得所有子組件 `$emit` 打出來的事件

```vue
    <div id="app">
        <test2 :data1="data1" :data2="data2" @event1="ev1" @event2="ev2"></test2>
    </div>

    <template id="test2">
        <div class="test">
            {{data1}} - {{$attrs}}
            <test3 v-bind="$attrs" v-on="$listeners"></test3>
        </div>
    </template>

    <template id="test3">
        <div class="test">
            {{data2}}
        </div>
    </template>
```

```js
        Vue.component("test2",{
            template:"#test2",
            props:['data1'],
            mounted(){
                this.$emit("event1","1231231")
            }
        })
        Vue.component("test3",{
            template:"#test3",
            props:['data2'],
            mounted(){
                this.$emit("event2","21321123123")
            }
        })
        new Vue({
            el:"#app",
            data:{
                data1:"Hello World1",
                data2:"Hello World2"
            },
            methods:{
                ev1(data){
                    console.log("ev1",data)
                },
                ev2(data){
                    console.log("ev2",data)
                }
            }
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
1. 在生命週期當中只執行一次，在DOM渲染完成之後只會執行一次
1. 當數據再傳遞過來時候，這鉤子函數就不會再觸發了
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

- 不緩存：

  進入的時候可以用`created`和`mounted`鉤子，離開的時候用`beforeDestory`和`destroyed`鉤子,`beforeDestory`可以訪問`this`，`destroyed`不可以訪問`this`。

- 緩存了組件：

  緩存了組件之後，再次進入組件不會觸發`beforeCreate`、`created` 、`beforeMount`、 `mounted`，**如果你想每次進入組件都做一些事情的話，你可以放在activated進入緩存組件的鉤子中**。

  同理：離開緩存組件的時候，`beforeDestroy`和`destroyed`並不會觸發，可以使用`deactivated`離開緩存組件的鉤子來代替

### 注意事項2

> 當需要在`mounted`設置DOM的計算寬度時，必須是有DOM節點並且已經被渲染

介面中先組件渲染 -> 數據後續才有 -> 有了數據之後(ajax) -> 再傳遞給組件 -> 組件渲染顯示

1. 檢查先創建了組件還是先有數據
2. 但當在`Ajax`獲取數據渲染後的DOM`mounted`獲取不到DOM，因為組件先創建了，裡面還沒有數據
3. 在生命週期當中只執行一次，在DOM渲染完成之後只會執行一次
4. 當數據再傳遞過來時候，這鉤子函數就不會再觸發了。
5. `beforeUpdate`監控數據的變化，當ajax獲取數據完成之後，當數據發生改變就會去更新DOM，`beforeUpdate`就在更新的時候去執行，就可以去獲取以獲取數據的DOM

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

## 監聽子組件的生命週期函數

有時，需要在父組件監聽子組件掛載後`mounted`，做一些邏輯處理。

例如：
加載遠端組件時，想抓取組件從遠端加載到掛載的耗時。

`@hook`可以监听到子组件的生命周期钩子函数(`created`, `updated`等等). 例如: 

```vue
// Parent.vue
<template>
  <Child v-bind="$props" v-on="$listeners" @hook:mounted="doSomething"> </Child>
</template>
<script>
  import Child from "./Child";
  export default {
    props: {
      title: {
        required: true,
        type: String
      }
    }
    components: {
      Child
    },
    methods: {
      doSomething(){
        console.log("child component has mounted!");
      }
    }
  };
</script>

作者：James Zhang
链接：https://juejin.im/post/5d790819e51d453b5e465bc7
来源：掘金
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```



## 比較 Filters 和 Computed
1. Filters 主要用於簡單的文字格式處理，需要在應用程式中重複使用。
1. Computed 適合較複雜的資料處理與轉換。
1. Methods 主要用來觸法狀態的改變，可能意味著會觸發 Computed 或 Filters 重新計算。
1. Filters 和 Computed 應是純粹的輸入輸出，通常不應該在此修改狀態。
## Watch vs Computed
> 雖然在大多數情況下，`Computed` 更合適，但有時仍需要使用 `Watch`。
> 當你需要響應更改的數據執行非同步或複雜的計算時，Watch 就非常有用。
### watch與computed的set函數的比較

> vuex 接收 的computed ，用set監測不到變化，必須要用watch才可以生效；（原理：實質沒有改變computd的值，只是改變了get的return值 => 組件外的訪問）

> v-model 改變的computed，用watch監測不到變化，必須要用computed對象中的set函數方法才能監測得到（原理：相當於每次改變了computed自身的值 => 組件內的訪問）

## Watch

```js
watch: {
    someData: {
        handler(newVal, oldVal) {
            // 監聽變量變化的處理函數
        },
        deep: true, // 是否深度監聽，例如對象某些屬性的變化
        immediate: true // 是否在第一次賦值時執行
    }
}
```

### handler

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
>
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

### 修饰符
1. `v-model.number=""`，可以自动将用户的输入值转为数值类型
1. `v-model.trim=""`，如果要自动过滤用户输入的首尾空白字符
1. `v-model.lazy=""`，更改 input 內的值並不會馬上變更 model 的資料，而是等到滑鼠移到輸入框外，觸發 change 事件才更新
## 組件之間傳遞數據

### 父組件向子組件傳數據(1) / props
> 第一步先在子組件，props自定義屬性
​```javascript
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
​```html
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

#### 在html使用

```vue
<div id="app">
    <test></test>
    <button @click="myEvent">$ON</button>
</div>
<template id="test">
    <div class="test">
        <button @click="eventBus">EVENT BUS</button>
        <test-child></test-child>
    </div>
</template>
<template id="test-child">
    <div class="test">
        test-child - {{msg}}
    </div>
</template>
```



```js
// 先創建Bus
var Bus = new Vue();
```

```js

Vue.component("test", {
    template: "#test",
    methods: {
        eventBus() {
            Bus.$emit("tobus", "TOBUS")
        }
    }
})
Vue.component("test-child", {
    template: "#test-child",
    mounted() {
        Bus.$on("tobus", (data) => {
            this.msg = data
        })
    },
    data() {
        return {
            msg: ""
        }
    }
})
new Vue({
    el: "#app",
    data: {
        data1: "Hello World1",
        data2: "Hello World2"
    },
    mounted() {
        Bus.$on("tobus", (data) => {
            console.log(data)
        })
    },
    methods: {
        myEvent() {
            console.log(123)
        }
    }
})
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

### Vue EvebtBus 小問題 - 多次觸發

#### 方法1:

在每次調用方法前先解綁事件( bus.$off )，然後在重新綁定( bus.$on )

```js
this.$off("helloBus")
this.$eventBus.$on('helloBus',function(data){
    console.log("toHelloBus: "+data)
})
```

#### 方法2:

```js
npm i -S vue-happy-bus

// main.js
import BusFactory from 'vue-happy-bus'
Vue.prototype.$BusFactory=BusFactory;
```

每個使用`Bus`的組件中都要在data中註冊這個事件`Bus:this.$BusFactory(this)`這樣才能調用其中這個this指向vue原型

## Vue.extend 和 Vue.component差別

1. 個人理解是：Vue.extend({})是Vue.component({})的核心，換句話說：
    1. 在實體化 Component 時會用到 Extend。
    1. Vue.component 是一個語法糖，使我們不需透過 Extend 和其他程序來實體化 Vue Instance 的子類別。
## Vue.nextTick()
> 應用場景：需要在視圖更新之後，基於新的視圖進行操作。 
>
> 可以使用Vue.nextTick取得更新後的 DOM。
>
> this.$nextTick 確保DOM已經渲染完成之後，再去做一些事情
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

## 非同步組件

- **工廠函數**
- **Promise**
- **物件**

### **工廠函數**

`1` 秒之後頁面就會取得組件而進行渲染

```js
Vue.component('async-component-factory-function', (resolve, reject) => {
  setTimeout(() => {
    resolve({
      template: '<div>Async Component</div>'
    });
  }, 1000);
});
```

### `Promise`

```js
Vue.component('async-component-promise', () => new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve({
      template: '<div>Async Component Promise</div>'
    });
    // reject('Error!!!');
  }, 1000);
}));
```

### **物件**

- `component` : 非同步組件。
- `loading` : 在非同步組件載入前渲染於頁面的組件。
- `error` : 載入錯誤時的組件。
- `delay` : 在 `delay` 多久後顯示等待組件。
- `timeout` : 超過 `timeout` 時間後渲染錯誤組件。

```js
const LoadingComponent = {
  template: '<div>Loading...</div>'
};
const ErrorComponent = {
  template: '<div>Error!!!</div>'
};
Vue.component('async-component-object', () => ({
    // 需要加載的組件 (應該是一個 `Promise` 對象)
    // component: import('./MyComponent.vue'),
    component: new Promise((resolve, reject) => {
        setTimeout(() => {
            resolve({
                template: '<div>Async Component Object</div>'
            });
            // reject('Error!!!');
        }, 5000);
    }),
    // 異步組件加載時使用的組件
    loading: LoadingComponent,
    // 加載失敗時使用的組件
    error: ErrorComponent,
    // 展示加載時組件的延時時間。默認值是 200 (毫秒)
    delay: 3000,
    // 如果提供了超時時間且組件加載也超時了，
    // 則使用加載失敗時使用的組件。默認值是：`Infinity`
    timeout: 6000
}));
```

- 在 `5` 秒載入組件。
- `3` 秒後顯示等待組件 `LoadingComponent` 。
- 如果超過 `6` 秒，顯示錯誤組件 `ErrorComponent` 。

## v-slot 2.6

使用`slot`插槽使`組件`可以更靈活的使用

1.  父組件在使用子組件`slot`，並在子組件內定義`template`，並定義`slot-scope`
2.  在子組件已定義的`slot`綁定`自定義屬性`
3.  `#`為`v-slot`縮寫

### Default Slot && Named Slot

1.  `v-slot:default`  匿名插槽
2.  `v-slot:nav` 可縮寫 `#nav` 具名插槽

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
        <main>
    		<slot>默認值SLOT</slot>
    	</main>
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
		<template v-slot:default>
			<p>我是匿名SLOT</p>
		</template>
      </Child>
  </div>
</template>
```
### Slot-Scope 作用域插槽

1.  重點是`slotProps`接取子組件裡`:text="nameText"`類似屬性的數據
2.  `slotProps`可以隨意命名

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
                    {"name":"AA","country":"TW"},
                    {"name":"BB","country":"TW"},
                    {"name":"CC","country":"USA"},
                ]
            }
        }
    }
</script>
```

#### example2

```vue
// Parent
<template>
    <titled>
        <template #header="slotProps">
    <div>{{slotProps.user.firstname}}</div>
        </template>
        <template #btn="{alertText}">
    <button @click="alertText">Click ME!</button>
        </template>
    </titled>
</template>
<script>
    import titled from '@/components/titled.vue';
    export default{
        components:{titled}
    }
</script>
```

```vue
// child
<template>
	<div>
        <header>
            <slot name="header" :user="user">{{user.lastname}}</slot>
    	</header>
        <slot name="btn" :alertText="alertText"></slot>
    </div>
</template>
<script>
    export default {
        name:"titled",
    	data(){
                return {
                    user:{
                        firstname:"Mr",
                        lastname:"Chen"
                    }
                }
            },
            methods:{
                alertText(){
                    console.log(123)
                }
            }
    }
</script>
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
## data資料改變監測

### 數組替換方法

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

## Vue2.0 bulid後的問題

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

#### 1.方法1

```js
// 安裝
npm i --save babel-polyfill
// main.js
import 'babel-polyfill'
```

#### 2.方法2

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

在babel.config.js中配置如下

```js
module.exports = {
  presets: [
    ['@vue/app', {
      polyfills: [
        'es6.array.iterator',
        'es6.promise',
        'es7.promise.finally',
        'es6.symbol',
        'es6.array.find-index',
        'es7.array.includes',
        'es6.string.includes',
        'es6.array.find',
        'es6.object.assign'
      ]
    }]
  ]
}
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

## 使用axios

```js
npm i -S axios
```

```js
// main.js
import axios from 'axios'
Vue.prototype.$axios=axios
```

```vue
// Home.vue
mounted(){
    this.$axios.get(api)
	.then((res)=>{
		console.log(res)
	})
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

## Vue 全局引入第三方插件如:jquery...

### 方法1(Best):

>   創建lib目錄，存放裡面
>
>   對象包含一個 install 方法，該方法的參數是 Vue 構造函數，我們使用 Object.defineProperty 或 Reflect 的方法將 `$echarts` 定義到 Vue.prototype 中去。

```js
import jquery from 'jquery'

export default{
    install(Vue){
        Object.defineProperty(Vue.prototype,"$jquery",{
            value:jquery
        })
    }
}
```

>   在main.js引入

```js
import jquery from './lib/jquery'

Vue.use(jquery)
```

>   使用

```js
mounted(){
	this.$jquery('.vuex').html()
}
```

### 方法2:

> 在main.js引入

```js
import jquery from 'jquery'

Vue.prototype.$jquery=jquery;
```

> 使用

```js
mounted(){
    this.$jquery.ajax({
        url:"https://randomuser.me/api/",
        method:"GET",
        dataType:"JSON",
        success:function(data){
            console.log(data)
        }
    })
}
```

## 監聽某個元素的尺寸變化

```js
// 安裝resize-detector
cnpm i --save resize-detector

// 引入
import { addListener, removeListener } from 'resize-detector'

// 使用
addListener(this.$refs['dashboard'], this.resize) // 監聽
removeListener(this.$refs['dashboard'], this.resize) // 取消監聽

// 一般我們會對回調函數進行去抖
methods: {
    // 這裡用了lodash的debounce
    resize: _.debounce(function (e) {
      console.log('123')
    }, 200)
}

```



## Vue Cli 3.x

## ENV 環境變量`env`使用

在根目錄建立:

1.  `.env`:在所有的環境中被載入
2.  開發用:`.env.development`模式用於 `vue-cli-service serve`
3.  生產用:`.env.production`模式用於 `vue-cli-service build` 和 `vue-cli-service test:e2e`
4.  意模式不同於 `NODE_ENV`，一個模式可以包含多個環境變量。也就是說，每個模式都會將 `NODE_ENV`的值設置為模式的名稱——比如在 development 模式下 `NODE_ENV` 的值會被設置為 `"development"`

```js
// .env.development

NODE_ENV=development
// 名稱一定得命名 VUE_APP_XXX
VUE_APP_API=../api/brand_api.php
VUE_APP_TITLE=My App
```

```js
// 使用
process.env.VUE_APP_API
console.log(process.env.VUE_APP_API)
```

## Vue Component 好的寫法

###  Lazy Loading / Async Components

```vue
<template>
  <section>
    <editor></editor>
  </section>
</template>

<script>
export default {
  name: 'dashboard',
  components: {
    'Editor': () => import('./Editor')
  }
}
</script>
```

### Required Props

```vue
<template>
  <section>
    <editor></editor>
  </section>
</template>

<script>
export default {
  name: 'dashboard',
  props:{
	enabled:{},
    isAdmin:{
    	required:true
    }
  },
  components: {
    'Editor': () => import('./Editor')
  }
}
</script>
```

### Trigger Custom Events with `$emit`

```vue
<template>
  <section>
    <button @click="onClick">Save</button>
  </section>
</template>

<script>
export const SAVE_EVENT = 'save';
export default {
  name: 'triggerEvent',
  methods: {
    onClick() { 
      this.$emit(SAVE_EVENT);
    }
  }
}
</script>
```

```vue
<template>
  <section>
  <p v-show="showSaveMsg">Thanks for Listening for the saved event</p>
  <trigger-event @save="onSave"></trigger-event>
  </section>
</template>

<script>
export default {
  name: 'TriggerEvent',
  data(){
    return {
      showSaveMsg: false
    }
  },
  components:{
    //You can find Trigger Custom Events in VueJs https://gist.github.com/eabelard/36ebdc8367bfeff75387cc863c810f65 
    TriggerEvent: () => import('./TriggerEvent')
  },
  methods: {
    onSave() { 
        this.showSaveMsg = true;
    }
  }
}
</script>
```



## Vue優化方式

### 1.使用v-if代替v-show

### 2.v-for必須加上key，並避免同時使用v-if

1.  為了過濾一個列表中的項目 比如 `v-for="user in users" v-if="user.isActive"`。在這種情形下，請將 `users`替換為一個計算屬性 (比如`activeUsers`)，讓其返回過濾後的列表
2.  為了避免渲染本應該被隱藏的列表 比如 `v-for="user in users" v-if="shouldShowUsers"`。這種情形下，請將 `v-if` 移動至容器元素上 (比如 ul, ol)

### 3.對路由組件進行懶加載

1.  使用異步路由可以根據URL自動加載所需頁面的資源，並且不會造成頁面阻塞，較適用於移動端頁面，建議主頁面直接import，非主頁面使用異步路由

    ```vue
    {
      path: '/order',
      component: () => import('./views/order.vue')
    }
    ```

### 4.異步組件

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

### 5.打包優化 使用CDN

#### 方法一:

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width,initial-scale=1.0">
    <link rel="icon" href="<%= BASE_URL %>favicon.ico">
    <title>VUE后台管理系统</title>
    <link rel="stylesheet" href="https://unpkg.com/element-ui/lib/theme-chalk/index.css">
</head>
<body>
<div id="app"></div>
<!-- built files will be auto injected -->
<script src="https://unpkg.com/vue@2.5.16/dist/vue.runtime.min.js"></script>
<script src="https://unpkg.com/vuex@3.0.1/dist/vuex.min.js"></script>
<script src="https://unpkg.com/vue-router@3.0.1/dist/vue-router.min.js"></script>
<script src="https://unpkg.com/element-ui/lib/index.js"></script>

</body>

</html>


```

```js
// vue.config.js

module.exports = {
    baseUrl: process.env.NODE_ENV === "production" ? "./" : "/",
    outputDir: process.env.outputDir,
    configureWebpack: {
        externals: {
            vue: "Vue",
            vuex: "Vuex",
            "vue-router": "VueRouter",
            "element-ui": "ELEMENT"
        }
    }
};
```

#### 方法二:

```js
// vue.config.js  修改
const path = require('path')

function resolve(dir) {
  return path.join(__dirname, './', dir)
}

// cdn预加载使用
const externals = {
  'vue': 'Vue',
  'vue-router': 'VueRouter',
  'vuex': 'Vuex',
  'axios': 'axios',
  'element-ui': 'ELEMENT',
  'js-cookie': 'Cookies',
 'nprogress': 'NProgress'
}

const cdn = {
  // 开发环境
  dev: {
    css: [
      'https://unpkg.com/element-ui/lib/theme-chalk/index.css',
      'https://cdn.bootcss.com/nprogress/0.2.0/nprogress.min.css'
    ],
    js: []
  },
  // 生产环境
  build: {
    css: [
      'https://unpkg.com/element-ui/lib/theme-chalk/index.css',
      'https://cdn.bootcss.com/nprogress/0.2.0/nprogress.min.css'
    ],
    js: [
      'https://cdn.jsdelivr.net/npm/vue@2.5.17/dist/vue.min.js',
      'https://cdn.jsdelivr.net/npm/vue-router@3.0.1/dist/vue-router.min.js',
      'https://cdn.jsdelivr.net/npm/vuex@3.0.1/dist/vuex.min.js',
      'https://cdn.jsdelivr.net/npm/axios@0.18.0/dist/axios.min.js',
      'https://unpkg.com/element-ui/lib/index.js',
      'https://cdn.bootcss.com/js-cookie/2.2.0/js.cookie.min.js',
      'https://cdn.bootcss.com/nprogress/0.2.0/nprogress.min.js'
    ]
  }
}

module.exports = {
  // 修改webpack config, 使其不打包externals下的资源
  configureWebpack: config => {
    const myConfig = {}
    if (process.env.NODE_ENV === 'production') {
      // 1. 生产环境npm包转CDN
      myConfig.externals = externals
    }
    if (process.env.NODE_ENV === 'development') {
      /**
       * 关闭host check，方便使用ngrok之类的内网转发工具
       */
      myConfig.devServer = {
        disableHostCheck: true
      }
    }
    return myConfig
  }
}

```

```html
<!-- public/index.html -->
<!DOCTYPE html>
<html lang="zh-Hant-TW">

<head>
  <meta charset="utf-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width,initial-scale=1.0">
  <link rel="icon" href="<%= BASE_URL %>favicon.ico">

  <!-- 使用CDN加速的CSS文件，配置在vue.config.js下 -->
  <% for (var i in htmlWebpackPlugin.options.cdn&&htmlWebpackPlugin.options.cdn.css) { %>
  <link href="<%= htmlWebpackPlugin.options.cdn.css[i] %>" rel="preload" as="style">
  <link href="<%= htmlWebpackPlugin.options.cdn.css[i] %>" rel="stylesheet">
  <% } %>

  <!-- 使用CDN加速的JS文件，配置在vue.config.js下 -->
  <% for (var i in htmlWebpackPlugin.options.cdn&&htmlWebpackPlugin.options.cdn.js) { %>
  <link href="<%= htmlWebpackPlugin.options.cdn.js[i] %>" rel="preload" as="script">
  <% } %>

  <title>vue-project-demo</title>
</head>

<body>
  <noscript>
    <strong>We're sorry but vue-project-demo doesn't work properly without JavaScript enabled. Please enable it to
      continue.</strong>
  </noscript>
  <div id="app"></div>
  <!-- 使用CDN加速的JS文件，配置在vue.config.js下 -->
  <% for (var i in htmlWebpackPlugin.options.cdn&&htmlWebpackPlugin.options.cdn.js) { %>
  <script src="<%= htmlWebpackPlugin.options.cdn.js[i] %>"></script>
  <% } %>
  <!-- built files will be auto injected -->
</body>

</html>

```

## Vue deploy Github

### Step1

```js
// vue.config.js
module.exports = {
  publicPath: process.env.NODE_ENV === 'production'?'/my-project/':'/'
}
```
### Step2

```js
npm run build
```

### Step3

```js
#!/usr/bin/env sh 
 
# abort on errors 
set -e 
 
# build 
npm run build 
 
# navigate into the build output directory 
cd dist 
 
git init 
git add -A 
git commit -m 'deploy' 
 
git push -f git@github.com:<USERNAME>/<REPO>.git master:gh-pages 
 
cd - 
```

