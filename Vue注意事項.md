# 不要在使用v-for的同一元素上使用v-if

```vue
<ul>
  <li
    v-for="game in games"
    v-if="game.isActive"
    :key="game.slug"
  >
    {{ game.title }}
  <li>
</ul>
```

## **化繁為簡的Watchers**

>   首先，在watchers中，可以直接使用函數的字面量名稱；其次，聲明immediate:true表示創建組件時立馬執行一次。

```js
created(){
	this.fetchPosList()
},
watch:{
    searchInputValue:{
        handler:"fetchPosList",
        immediate:true
    }
}
```

## **釜底抽薪的router key** 

>   假設我們在寫一個網站，需求是從/post-page/a，跳轉到/post-page/b。然後我們驚人的發現，頁面跳轉後數據竟然沒更新？！原因是vue-router"智能地"發現這是同一個組件，然後它就決定要復用這個組件，所以你在created函數裡寫的方法壓根就沒執行。通常的解決方案是監聽$route的變化來初始化數據，但是這樣的寫法很繁瑣，秉持著能偷懶則偷懶的原則，有沒有辦法優化一下？ 

>   招式解析： 答案是給router-view添加一個unique的key，這樣即使是公用組件，只要url變化了，就一定會重新創建這個組件。（雖然損失了一丟丟性能，但避免了無限的bug）。同時，注意我將key直接設置為路由的完整路徑，一舉兩得。

```vue
<router-view :key="$route.fullpath"></router-view>
```

