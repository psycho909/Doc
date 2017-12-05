# Javascript
1. [Ajax單次調用](#getjson)
1. [Ajax多次調用](#multi-getjson)
1. [展開運算子與其餘參數 ...rest](#rest-parameters)
1. [ES6模塊的使用`import`&`export`](#import-export)
1. [javascript 注意事項](#js-notice)
1. [Fetch](#fetch)
1. [ES6 Array.from()使用方式](#array-from)
1. [apply()、call()使用方式](#apply-call)
1. [Array常用操作](#use-array)
1. [DOM API操作](#dom_api)
--------------
## <span id="getjson">Ajax單次調用</span>
```javascript
$.getJSON(url).
    done(function(daTa){
    }).
    fail(function(){
    }).
    always(function(){s
    })
```
## <span id="multi-getjson">Ajax多次調用</span>
```javascript
var firstData = $.get('http://example.com/first-api/');
var secondData = $.get('http://example.com/second-api/');

$.when(firstPromise, secondPromise).done(function(firstData, secondData) {
    // do something
});
```

## <span id="rest-parameters">`展開運算子`與`其餘參數` `...rest`</span>
> 展開運算子(Spread Operator)與其餘參數(Rest parameters)是ES6中的其中一種新特性。也是懶人必學的Javascript新語法之一。這兩種特性的語法是一樣的，都是(`...`)三個點，我們常常在文字聊天時，這個(`...`)常用來代表了"無言"、"無窮的想像"或"後面其他的"的意思。<br>
>1. 符號是三個點(`...`)
>1. 使用於陣列
>1. 一個使用於函式的參數，一個是用於運算中
>
## 其餘參數(Rest parameters)
### 使用於"不確定的傳入參數值"
```javascript
function sum(...numbers){
    let result=0;
    numbers.forEach((number)=>{
        result+=number;
    })
    return result;
}
console.log(sum(1)); // 1
console.log(sum(1, 2, 3, 4, 5)); // 15
```
## 展開運算子(Spread Operator)
> `展開運算子是"把一個陣列展開(expand)成個別數值"的速寫語法`
```javascript
var params=["hello",true,7]
var other=[1,2,...params] //[ 1, 2, "hello", true, 7 ]

var str="foo"
var chars=[..str] //[ "f", "o", "o" ]
```
> 也可用來把陣列展開，傳入函式之中
```javascript
function sum(a,b,c){
    return a+b+c
}
var args=[1,2,3]
console.log(sum(...args)) //6
```
> 展開運算子(Spread Operator)可以讓程式碼更容易閱讀，[這篇文章](https://ponyfoo.com/articles/es6-spread-and-butter-in-depth)的作者作了一張比較表把各種常見的使用情況、ES5、ES6列出來，我加了一個字串分割。表格如下：
>
|情況|ES5|ES6|
|:---:|:---|:---:|
|Concatenation|`[1, 2].concat(more)`|`[1, 2, ...more]`|
|String Split|`chars = str.split("")`|`chars = [ ...str ]`|
|Push onto list|`list.push.apply(list, [3, 4])`|`list.push(...[3, 4])`|
|Destructuring|`a = list[0], rest = list.slice(1)`|`[a, ...rest] = list`|
|new + apply|`new (Date.bind.apply(Date, [null,2015,31,8]))`|`new Date(...[2015,31,8])`|
>
```javascript
// Rest Properties
let { x, y, ...z } = { x: 1, y: 2, a: 3, b: 4 };
x; // 1
y; // 2
z; // { a: 3, b: 4 }

// Spread Properties
let n = { x, y, ...z };
n; // { x: 1, y: 2, a: 3, b: 4 }
```
## `let`&`const`使用
> `let`常量 & `const`變量<br/>
> 在一個函數內部，在一個代碼塊內部為作用域<br>
> `{}大括號內`的代碼塊即為`let`和`const`的作用域
## ES6 更方便的數據訪問
```javascript  
//object
const people={
    name:'lux',
    age:20
}
const {name,age}=people;
console.log(name,age)
//array
const color=['red','blue']
const [first,second]=color
console.log(first,second)
```
## <span id="import-export">ES6模塊的使用`import`&`export`</span>
```javascript
//全部導入
import people from './example'
//有一種特殊情況，即允許你將整個模塊當作單一對象進行導入
//該模塊的所有導出都會作為對象的屬性存在
import * as example from "./example.js"
console.log(example.name)
console.log(example.age)
console.log(example.getName())
//導入部分
import {name, age} from './example'
// 導出默認, 有且只有一個默認
export default App
// 部分導出
export class App extend Component {};
```
> 注意事項
```javascript
1.當用export default people導出時，就用 import people 導入（不帶大括號）

2.一個文件裡，有且只能有一個export default。但可以有多個export。

3.當用export name 時，就用import { name }導入（記得帶上大括號）

4.當一個文件裡，既有一個export default people, 又有多個export name 或者 export age時，導入就用 import people, { name, age } 

5.當一個文件裡出現n多個 export 導出很多模塊，導入時除了一個一個導入，也可以用import * as example

```
## ajax正確請求方式
> 點擊按鈕發送ajax請求
>1. 考慮ajax請求失敗的情況
>1. 考慮ajax請求成功，但是狀態不是200的情況
>1. 考慮ajax請求成功，狀態是200，但是沒有數據的情況
>1. 考慮ajax重複請求的情況
>1. 考慮ajax請求接口時間長的交互效果
```javascript
var submit=function(){
    var me=$(this);
    //判斷是否重複點擊
    if(me.hasClass('disabled')){
        return;
    }
    me.addClass('disabled');
    //發送的數據
    var sendData={
        time:3
    }
    $.ajax({
        url:url,
        type:post,
        data:sendData,
        beforeSend:function(){
            lnv.pageloading();
        }
    })
    //服務器請求成功
    .done(function(res){
        //處理返回的數據，因為有的時候可能是字符串，非JSON格式
        res=typeof(res)==='string'?JSON.parse(res):res;
        //如果返回的數據狀態是200
        if(res.status==200){
            //如果返回的數據data為空
            if(res.data.length===0){
                console.log('沒有數據')
            }else{
                //如果返回的數據data不為空
            }
        }else{
            //如果返回數據狀態不是200
            console.log('error')
        }
    })
    //服務器請求失敗
    .fail(function(){
        console.log('error')
    })
    .always(function(){
        //無論成功還是失敗，刪掉loading效果
        lnv.destroyloading();
        //刪除重複點擊的標示
        me.removeClass('disabled');
    })
}
```
## `Array.find()`查找
```javascript
arr.find(callback[, thisArg]);

var pets = [
  { type: 'Dog', name: 'Max'},
  { type: 'Cat', name: 'Karl'},
  { type: 'Dog', name: 'Tommy'},
]
var pet=pets.find(function(pet){
    return pet.type==='Dog' && pet.name==='Tommy'
})
console.log(pet)
```
## `for in`用於遍歷數組或者對象的屬性
```javascript
for(variable in object){
    //something
}
```
## <span id="js-notice">javascript 注意事項</span>
### 使用`if()`判斷時能使用三元運算`x+y?'A':'B'`，就使用三元運算
## <span id="fetch">`Fetch`使用</span>
> `fetch()`方法是一個位於全域window物件的方法，它會被用來執行送出Request(要求)的工作，如果成功得到回應的話，它會回傳一個帶有Response(回應)物件的已實現Promise物件。fetch()的語法結構完全是Promise的語法，十分清楚容易閱讀，也很類似於jQuery的語法:
```javascript
fetch(url,{method:'get'})
.then(function(response){
    //處理response
}).catch(function(err){
    //Error
})
```
> 但要注意的是fetch在只要在伺服器有回應的情況下，都會回傳已實現的Promise物件狀態(只要不是網路連線問題，或是伺服器失連等等)，在這其中也會包含狀態碼為錯誤碼(404, 500...)的情況，所以在使用時你還需要加一下檢查
```javascript
fetch(url,{method:'get'})
.then(function(response){
    //ok 代表狀態碼在範圍 200-299
    respones.ok
    respones.status
}).catch(function(err){
    //Error
})
```
> response 常用的是`json`與`text`方法<br>
> json方法會回傳一個帶有包含JSON資料的物件值的Promise已實現物件。
```javascript
fetch(url,{method:'get'})
.then(function(response){
    //處理response
    return response.json()
}).then(function(j){
    //返回j是一個Javascript物件
}).catch(function(err){
    //Error
})
```
### Request
> Request接口定義了通過HTTP請求資源的request格式。參數需要URL、method和headers，同時Request也接受一個特定的body，mode，credentials以及cache hints.
>
> 最簡單的Request當然是一個URL，可以通過URL來GET一個資源
```javascript
var req=new Request("/index.html");
console.log(req.mehtod)//"GET"

var uploadReq=new Request("/uploadImage",{
    method:"POST",
    headers:{
        "Content-Type":"image/png"
    },
    body:"image data"
})
fetch(uploadReq)
```
### Headers 接口
> Fetch引入了3個接口，分別是Headers、Request以及Response。他們直接對應了相應的HTTP概念，但是基於安全考慮，有些區別，例如支持CORS規則以及保證cookies不能被第三方獲取。
>
> Headers接口是一個簡單的多映射的名-值表
```javascript
var headers=new Headers({
    "Content-Type":"text/plain",
    "Content-Length": content.length.toString(),
    "X-Custom-Header": "ProcessThisImmediately"
})
```
> Header的值是可以被檢索的
```javascript
console.log(headers.has("Content-Type"))//true
```
## <span id="array-from">ES6 Array.from()使用方式</span>
> 可以將類似Array對象的東西(arguments,NodeList)轉成真的數組形式
>
> 舊方法:`Array.prototype.slice.call()`
```javascript
let divs=document.querySelectorAll('.box');
Array.from(divs)
```
## <span id="apply-call">apply()、call()使用方式</span>
> 在javascript中，call和apply都是為了改變某個函數運行的上下文而存在，就是為了改變函數體內部的this的指向。
```javascript
function fruit(){}
fruit.prototype={
    color:'red',
    say:function(){
        console.log('My color is '+this.color)
    }
}
var apple=new fruit;
apple.say(); //My color is red
```
> 如果有一個對象banana={color:'yellow'},不想重新定義say的方法，可以通過call或apply使用apple的say方法
```javascript
banana={
    color:'yellow'
}
apple.say.call(banana)//My color is yellow
apple.say.apply(banana)//My color is yellow
```
> call和apply是為了動態改變this的兒出現，當一個object沒有某個方法，但是其他的有，可以借助call或apply用其他對象的方法來操作
### apply()、call()的區別
> apply、call做用完一樣，只是接受參數的方式不一樣:
```javascript
var func=(arg1,arg2)=>{}
func.call(this,arg1,arg2);
func.apply(this,[arg1,arg2]);
```
> call需要把參數按順序傳遞進去，而apply則是把參數放在數組裡
>
> Javascript中，某個函數的參數數量是不固定的，因此適用條是，當你的參數是明確知道數量時用`call`
>
>而不確定的時候則用`apply`，然後把參數`push()`進數組傳遞進去。
```javascript
var array1 = [12 , "foo" , {name:"Joe"} , -2458]; 
var array2 = ["Doe" , 555 , 100];
Array.prototype.push.apply(array1,array2);
//array1 值为  [12 , "foo" , {name "Joe"} , -2458 , "Doe" , 555 , 100]
```
> 使用apply()獲取數組中的最大值和最小值
```javascript
var numbers=[5, 458 , 120 , -215 ];
var maxNumbers=Math.max.apply(Math,numbers)
var minNumbers=Math.min.apply(Math,numbers)
```
> ES6使用`...`展開運算符取代`apply()`獲取數組中的最大值和最小值
```javascript
var numbers=[5, 458 , 120 , -215 ];
var maxNumbers=Math.max(...numbers)
var minNumbers=Math.min(...numbers)
```
> 類(偽)數組使用數組方法
```javascript
var domNodes=Array.prototype.slice.call(document.querySelector('*'));
```
> 可使用新方法`Array.from()`
```javascript
var domNodes=Array.from(document.querySelector('*'));
```
## <span id="map-reduce">`map()`和`reduce()`使用方式</spn>
### 使用`map()`對每個陣列元素加工
```javascript
arr.map((value,key)=>{
        //dosmthing
    }
)
var myArr=[1,2,3]
myArr.map((v,i)=>{
    return v+1
})
//2,3,4
```
> `map()`會將每一個元素代入處理函數，而處理函數回傳的值，會被收集組成一個新的陣列，這個新的陣列元素數量會和原本陣列的一樣。
### 使用`map()`進行資料校正處理
```javascript
var myArr=[1,2,3,4,5,6,7,8,9,10]
myArr.map((v,i)=>{
    if(v>5){
        return 5
    }
    return v
})
```
### 使用`reduce()`進行數值加總
```javascript
arr.reduce((prev,element)=>{
    //dosmthing
},init)
var myArr=[1,2,3]
myArr.reduce((prev,element)=>{
    return prev+element
},0)
```
### 使用`map()`&`reduce()`一起始用
```javascript
var myArr = [ 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 ];
myArr.map((v,i)=>{
    if(v>5){
        return 5
    }
    return v
}).reduce((prev,element)=>{
    return prev+element
},0)
```
### 使用`reduce()`進行陣列扁平化
```javascript
var myArr = [
    [ 1, 2 ],
    [ 3, 4, 5 ],
    [ 6, 7, 8 ]
];
myArr.reduce((arr,element)=>{
    return arr.concat(element)
},[])
```
### 使用`reduce()`進行資料歸納和統計
```javascript
var myArr = [
    'C/C++',
    'JavaScript',
    'Ruby',
    'Java',
    'Objective-C',
    'JavaScript',
    'PHP'
];
myArr.reduce((langs,langName)=>{
    if(langs.hasOwnProperty(langName)){
        langs[langName]++;
    }else{
        langs[langName]=1;
    }
    return langs
},{})
```
### 處理Object的形式
```javascript
var data = {
    'Fred': 1,
    'Leon': 2,
    'Wesley': 3,
    'Chuck': 4,
    'Denny': 5
};
//使用Object.keys()取的包含所有key的陣列
Object.keys(data).reduce((prev,name)=>{
    return data[name]+prev
},0)
```
## <span id="use-array">Array常用操作</span>
> `join`:將陣列元素用固定符號串成字串
```javascript
var arr = ["jack", "john", "may", "su", "Ada"];
arr.join('|')
```
> `splice`:會返回並改變陣列內容，移除或新增元素
```javascript
array.splice(index , howMany[, element1[, ...[, elementN]]])
//index : 要從哪個索引位置開始改變
//howMany : 用來指出要移除多少個元素. 如果 howMany 等於 0，則沒有任何元素被移除
//element1, ..., elementN : 要加入陣列的元素，如果省略則表示不加入只刪除
var myFish = ["angel", "clown", "mandarin", "surgeon"];
myFish.splice(2,0,'drum') //["angel", "clown","drum", "mandarin", "surgeon"]
myFish.splice(2,1) //["angel", "clown", "mandarin", "surgeon"]
```
> `some`:陣列比對，只要有一個元素是 true，就返回 true (很好用)
```javascript
var arr = ["jack", "john", "may", "su", "Ada"];
arr.some(function (value, index, array) {
    return value == "may" ? true : false;
});
```
> `every`:陣列比對，所有元素都是 true 才是 true (很好用)
```javascript
var arr = ["jack", "john", "may", "su", "Ada"];
arr.every(function (value, index, array) {
    return value.length>2;
});
```
> `Array.from()`:將字串或輸入參數組成陣列
```javascript
let divs=document.querySelectorAll('.box');
Array.from(divs)
```
> Array.from()可以將`new Set()`結構轉成數組
```javascript
Array.from(new Set(["foo", window]));
//["foo", window]
```
> Array.from()可以將`new Map()`結構轉成數組
```javascript
var m = new Map([[1, 2], [2, 4], [4, 8]]);
Array.from(m);
//[[1, 2], [2, 4], [4, 8]]
```
> 使用Array.from()把字符串逐一轉成數組
```javascript
Array.from("foo");
//["f", "o", "o"]
```
> 使用Array.from()產生數字
```javascript
Array.from({length:5}, (v, k) => k);
//[0, 1, 2, 3, 4]
```
## <span id="dom_api">DOM API操作</span>
> 新增的操作DOM API有`before()`,`after()`,`replaceWidth()`,`append()`,`prepend`
### `before()`
> DOM之前插入，ChildNode.before((節點或字符串)....)
```javascript
var p=document.createElement('p')
document.querySeletor('#div').before(p)
//before不可以直接使用html
//使用方式可以
document.querySeletor('#div').insertAjacent('beforebegin','<div>123</div>')
```
### `after()`
> DOM之後插入
### `replaceWith()`
> DOM可替換節點
### `prepend()`
> ParentNode.prepend((節點或字符串))<br/>
> 可在當前節點最前面插入其他節點內容
### `append()`
> ParentNode.append((節點或字符串))<br/>
> 可在當前節點最後面插入其他節點內容
## 視口寬高、位置與滾動高度
1. `window.innerHeight,window.innerWidth`=>`瀏覽器窗口高度，如果有滾動條則包括`
1. `window.outerHeight,window.outerWidth`=>`瀏覽器窗口整個高度，包括窗口標題、工具攔、狀態等`
1. `offsetWidth`=>`一個元素布局寬度。`
```javascript
//不包含滾動條
document.documentElement.clientWidth
//使用offsetHeight作為Fallback要比clientHeight更好

//元素距離文黨距離
el.offsetTop
el.offsetLeft

//滾動
scrollTop //滾動高度
```