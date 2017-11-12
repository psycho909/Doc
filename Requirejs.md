# Require.js
1. [加載不支持AMD的庫](#none_amd)
> 在頁面中引入 require.js 在加上 data-main="js/main"
>
> data-main屬性的作用是，指定網頁程序的主模塊。在上例中，就是js目錄下面的main.js，這個文件會第一個被require.js加載。由於require.js默認的文件後綴名是js，所以可以把main.js簡寫成main。
>
> main.js內有`require.config() require()`

### require.config()寫在主模塊頭部
```javascript
require.config({
    baseUrl: "./js",
    shim:{
        "vue":{
            exports:"vue"
        }
    },
    paths: {
        "jquery": "./lib/jquery-3.1.1.min",
        "vue":"./lib/vue"
    },
});
```
### baseUrl加載文件
> RequireJS以一個相對於baseUrl的地址來加載所有的代碼
### paths
> 映射不放於baseUrl下的模塊名
```javascript

require.config({
    baseUrl:'/js',
    paths:{
        //可使用數組方式加載，假使第一個路徑加載失敗會換加載第二個
        'jquery':[
            "//cdnjs.cloudflare.com/ajax/libs/jquery/3.2.1/jquery.min",
            "./lib/jquery-3.1.1.min"
        ]
    }
})
```
### <span id="none_amd">配置不支持amd使用shim</span>
>1. `baseUrl` -> 直接改變基目錄
>1. `paths` -> 指定各個模塊的加載路徑，路徑名稱也可以只接指定他的網址
>1. `shim` -> 使用於加載非AMD規範，`shim`有兩個定義值`exports` & `deps`數組
>>1. `exports`值 ->（輸出的變量名）表明這個模塊外部調用時的名稱
>>2. `deps`數組 -> 表明該模塊的依賴性
```javascript
shim: {
    'underscore':{
        exports: '_'
    },
    //加載不支持AMD庫有依賴狀態
    'backbone': {
        deps: ['underscore', 'jquery'],//依賴的模塊
        exports: 'Backbone'ㄝ//全局變量作為模塊對象
        init:function($){//初始化函數，返回的對象代替exports作為模塊對象
            return $;
        }
    },
    "vue":{
        exports:"Vue"
    },
    //加載不支持AMD的庫無依賴狀態
    "modernizr":{
        exports:"Modernizr"
    },
    //加載不支持AMD的j框架，沒有全局變量但是有依賴
    'bootstrap':['jquery']
}
```
## require()函數
> `require()`函數接受兩個參數。第一個參數是一個數組，表示所依賴的模塊，上例就是`['moduleA', 'moduleB', 'moduleC']`，即主模塊依賴這三個模塊；第二個參數是一個回調函數，當前面指定的模塊都加載成功後，它將被調用。加載的模塊會以參數形式傳入該函數，從而在回調函數內部就可以使用這些模塊。
>
> `require()`異步加載`moduleA，moduleB和moduleC`，瀏覽器不會失去響應；它指定的回調函數，只有前面的模塊都加載成功後，才會運行，解決了依賴性的問題。
>
> 假定主模塊依賴`jquery、handlebars、templates`
>
> 第二個回調函數，如果使用`jquery`要加上，如果是`handlebars`需要使用大寫`Handlebars`
```javascript
require(["jquery","handlebars","templates"], function ($,Handlebars) {
    // some code
});
```
## AMD模塊的寫法
> 創建一個templates.js，並且依賴`jquery`
```javascript
define(["jquery"], function($) {
    var add=function(x,y){
        return x+y
    }
    return{
        add:add
    }
});
```
> 加載AMD版塊方法
```javascript
require(['math'],function(math){
    //some code
    console.log(add(1,2))
})
```
> 如果math 這個模塊還依賴其他模塊時，那麼`define()`函數的第一個參數，必須是一個數組，指明該模塊的依賴性。

## Require text 插件
> `text`插件是允許require.js加載文本
>
> 在想要編譯的文本前加上`text!`
```javascript
define(["text!../template/header.html"], function(header) {
    //some code
});
```
## <span id="map">map</span>
> 可以於開發不容模塊需求去加載不同版本
```javascript
map:{
    'app/api':{
        'jquery':'./lib/jquery'
    },
    'app/api2':{
        'jquery':'./lib/jquery2'
    }
}
```