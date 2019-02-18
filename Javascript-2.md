# Javascript-2
1. [](#)

--------------
## 什么时候你不能使用箭头函数？
### 定义对象方法
> 定义字面量方法
```javascript
const calculator = {
    array: [1, 2, 3],
    sum: ()=>{
        console.log(this === window); //true
        return this.array.reduce((result, item) => result + item);
    }
};
console.log(this === window); //true
// Throws "TypeError: Cannot read property 'reduce' of undefined"
calculator.sum();
```
> 定义原型方法
```javascript
function Cat(name) {
    this.name = name;
}

Cat.prototype.sayCatName = ()=> {
    console.log(this === window); // true
    return this.name;
};

const cat = new Cat('Mew');
cat.sayCatName(); // Mew
```
### 定义事件回调函数
```javascript
const button = document.getElementById('myButton');
button.addEventListener('click', () => {
    console.log(this === window); // => true
    this.innerHTML = 'Clicked button';
});
```
### 定义构造函数
```javascript
const Message = (text) => {
    this.text = text;
};
// Throws "TypeError: Message is not a constructor"
const helloMessage = new Message('Hello World!');
```
### 首字母大寫
```javascript
var firstToUpper=(str,lowerRest=false)=>{
    return str.slice(0,1)+(lowerRest?str.slice(1).toLowerCase():str.slice(1))
}
```
### 計算數組中值得出現次數
```javascript
var countCurrents=(arr,value)=>{
    return arr.reduce((a,v)=>v===value?a+1:a+0,0)
}
```

### 當前URL
```javascript
window.location.href
```

### 數組之間的區別
```javascript
var difference=(arr,values)=>arr.filter(v=>!values.includes(v))
```

#### inlcudes()
```javascript
'abcde'.includes('ab') // true
[1,2,3,4].includes(5) // false
```
### 獲取滾動位置
```javascript
var getScrollPos=(el=window)=>{
    return {
        x:(el.pageXOffset !== undefined)?el.pageXOffset:el.scrollLeft,
        y:(el.pageYOffset !== undefined)?el.pageYOffset:el.scrollTop
    }
}
```

### 用range初始化數組
```javascript
var initialArrayRange=(end,start=0)=>{
    return Array.apply(null,Array(end-start)).map((v,i)=>i+start)
}
```

### Array最後一個的值
```javascript
var last=arr=>arr.slice(-1)[0]
```

### 範圍內隨機整數
```javascript
var randomRange=(min,max)=>Math.floor(Math.random()*(max-min+1))+min
```

### 反轉字符串
```javascript
var reverseString=str=>[...str].reverse().join('')
```

### 數組去重複
```javascript
var unique=arr=>[...new Set(arr)]

var years=[2016,2016,2016,2017,2017,2018,2018,2019]
var res=[];
for(var i=0;i<years.length;i++){
    if(res.indexOf(years[i]) === -1){
        res.push(years[i])
    }
}
```

### 驗證數字
```javascript
var vaildNumber=n=>!isNaN(parseFloat(n))&&isFinite(n);
```

### Object拷貝
```javascript
var auntie = { 
    name: '陳小美', 
    ages: 22,
        bwh: {
        strength: 34,
        agility: 25,
        intelligence: 96
    },
    single: true
};

// 1.for...in 淺拷貝
var auntie3={};
for(var v in auntie){
    auntie3[v]=auntie[v]
}
// 轉字串 完全複製
var str=JSON.stringify(auntie)
var auntie3=JSON.parse(str)
// Object.assign() 淺拷貝
var auntie3=Object.assign({},auntie)
```

## 找DOM Child節點
```javascript
childElementCount : 返回子元素（不包括文本节点和注释）的个数
firstElementChild : 指向第一個子元素; firstChild的元素版,但是IE8以下不兼容
// 兼容的方法
var obj=oUl.firstElmentChild || oUl.firstChild;

lastElmentChild : 指向最後 一個子元素; lastChild的元素版
previousElementSibling : 指向前一個同輩元素; previousSibling的元素版
nextElementSibling : 指向最後一個同輩元素; nextSibling的元素版
```
## addEventListener()的使用
> `target.addEventListener(type, listener [,{capture: Boolean, bubbling: Boolean, once: Boolean}])`
* type 表示監聽事件類型的字符串
* listener 當所監聽的事件類型觸發時,會接收到一個事件通知
* options(可選)
    1. capture表示listener會在該類型的事件捕獲階段傳播到該EventTarget時觸發
    1. once表示listener再添加之後最多只調用一次。如果是true,listener會在其被調用之後自動移除
    1. passive 表示listener永远不会调用preventDefault()。如果listener仍然调用了这个函数，客户端将会忽略它并抛出一个控制台警告。
```javascript
link.addEventListener('click',function(e){
    e.preventDefault();
    console.log('link clicked')
},{capture:false,once:true})
//输出一次link clicked!后自动移除listener函数，再次点击无效。
```
## ES6`Object`物件的擴展
> 在 ES6 中允許在物件中直接給變量，這時候物件的屬性名稱為變數的名稱，物件的屬性值為變數的值，讓我們來看一下這個例子：
```javascript
var a='apple';
var obj={a};
console.log(obj) // {a: "apple"}
```
> 這時候變數的名稱(a)會變成物件的屬性名稱，變數的值("apple")會變成物件的屬性值。
```javascript
var a="apple";
var c="cat";
var obj={a,c}
console.log(obj); //{a:'aple',c:'cat'}
```
### 物件中的方法也可以簡寫
```javascript
var f="fish";
var p="pug";
var obj={
    f,
    p,
    eat(){
        console.log(this.p+' eat '+this.f)
    }
}
```
## Javascript 寫COOKIE
```javascript
// 寫入Cookie
document.cookie="myName=tom";

// 寫入Cookie，並加入過期時間
document.cookie="username=bob;expires=Mon,04 Dec 2017 08:18:32 GMT;path=/";

// GMT時間
new Date().toGMTString();

// 寫入Cookie，設定10秒後失效
document.cookie="username=bob;max-age=10;path=/";
```
## 數組去重
```javascript
var arr1 = [1,2,1,3,4],
arr2 = [3,4,5,1,2,3,6];
var result=[];var arr=[]
arr=arr2.reduce((prev,curr)=>{
	prev.push(curr)
	return prev
},arr1)
for(var i=0;i<arr.length;i++){
	var index=arr[i];
	if(result.indexOf(index) === -1){
		result.push(index)
	}
}
```
## 扁平化數組
```javascript
var more_arr=[[1, 2],[3, 4, 5], [6, 7, 8, 9],10];
// [].concat(...more_arr);
// [].concat.apply([],more_arr)
var newArr=more_arr.reduce((prev,curr)=>{
	return prev.concat(curr)
})
```
## 找出重複數組個數
```javascript
var arr1 = [1,2,1,3,4],
arr2 = [3,4,5,1,2,3,6];
var arr=[];
arr=[...arr1,...arr2]
var obj={};
arr.forEach((item,index)=>{
	if(obj[item]){
		obj[item]++
	}else{
		obj[item]=1
	}
})
// {1: 3, 2: 2, 3: 3, 4: 2, 5: 1, 6: 1}
```
## 找出重複OBJ個數
```javascript
var openData=[
	[1,7,0],
	[3,8,9],
	[4,5,8],
	[6,4,1],
	[3,6,1]
];
var obj={},arr=[],k;
openData.forEach((item,index)=>{
	for(var i=0;i<item.length;i++){
		arr.push(item[i])	
	}
})
for(var i=0;i<arr.length;i++){
	var index=arr[i]
	if(obj[index]){
		obj[index]++
	}else{
		obj[index]=1
	}
}

// {0: 1, 1: 3, 3: 2, 4: 2, 5: 1, 6: 2, 7: 1, 8: 2, 9: 1}
```
## 使用reduce找出重複數量
```javascript
var cars = ['BMW','Benz', 'Benz', 'Tesla', 'BMW', 'Toyota'];
var carrList=cars.reduce((obj,name)=>{
    obj[name]=obj[name]?++obj[name]:1;
    return obj
},{})
```
## replace去掉所有
```javascript
var str='<script>alert('123')</script>'
str.replace(/\//ig,'');
str.replace(/script/ig,'');
```
## JSON.stringify排列
```javascript
JSON.stringify(data,null,4)
```
## JSON.stringify傳參數
```javascript
var str_json=JSON.stringify(data,['name','age'])
```
## JSON.stringify 處理參數
```javascript
var data = [
    {name: "悟空", sex:1, age: 30},
    {name: "八戒", sex:0, age: 20},
    {name: "唐僧", sex:1, age: 30}
];

var str_json = JSON.stringify(data,function (key, value) {
    if(key==="sex"){
        return ["男生","女生"][value];
    }
    return value;
});
```
## 千位數小數點
```javascript
Number.prototype.formatMoney = function(c, d, t){
var n = this, 
    c = isNaN(c = Math.abs(c)) ? 2 : c, 
    d = d == undefined ? "." : d, 
    t = t == undefined ? "," : t, 
    s = n < 0 ? "-" : "", 
    i = String(parseInt(n = Math.abs(Number(n) || 0).toFixed(c))), 
    j = (j = i.length) > 3 ? j % 3 : 0;
    return s + (j ? i.substr(0, j) + t : "") + i.substr(j).replace(/(\d{3})(?=\d)/g, "$1" + t) + (c ? d + Math.abs(n - i).toFixed(c).slice(2) : "");
};
```
## Promise
1. 先宣告一個 函式
    1. 內有 new Promise
    1. 包含 resolve:成功
    1. 包含 reject: 失敗
1. 使用 Promise 函式
    1. 使用 then : 接收成功的訊息
    1. catch: 接收失敗的訊息
```js
var runPromise=function(someone,timer,success){
    console.log(someone+' 開始奔跑!')
    // Promise 從這開始
    return new Promise(function(resolve,reject){
        if(success){
            setTimeout(function(){
                resolve(someone+' '+(timer/1000)+'秒 抵達終點!')
            },timer)
            
        }else{
            reject(new Error(someone+' 失敗!'))
        }
    })
}
runPromise('Ming',2000,true).then(function(res){
    console.log(res)
    return runPromise('胖虎',2000,false)
})
.then(function(res){
    console.log(res)
})
.catch(function(res){
    console.log(res)
        })
```
### Promise.all()
> 同時處理多個函式，全部執行後，一同回傳
```js
Promise.all([runPromise('Ming',2000,true),runPromise('胖虎',2000,true)])
.then((res)=>{
    console.log(res)
})
.catch((err)=>{
    console.log(err)
})
```
### Promise.race()
> 類似比較速度，回傳速度最快的那個函式
### HTML不產生快取問題
```html
<meta http-equiv="Cache-Control" content="no-cache, no-store, max-age=0, must-revalidate">
<meta http-equiv="pragma" content="no-cache" />
<meta http-equiv="expires" content="0" />
```
### 分頁製作
1. 總共有幾頁
1. 目前第幾頁
1. 每頁多少數量
1. 當前頁面資料
```javascript
// 分頁
var totalResult=articles.length; // 目前總長度
var perpage=3; // 每頁三筆資料
var pageTotal=Math.ceil(totalResult/perpage); // 總頁數
var currentPage=1; //當前頁數

// 判斷 如果當前頁數 大於總頁數時
if(currentPage > pageTotal){
    currentPage=pageTotal;
}
var minItem=(currentPage * perpage)-perpage+1; // 最小 1
var maxItem=(currentPage * perpage); // 最大 3

var data=[];
articles.forEach((item,i)=>{
    var itemNum=i+1;
    if( itemNum >= minItem && itemNum <= maxItem){
        data.push(item)
    }
})
```
## rem 自適應增加
```js
var htmlwidth=document.documentElement.clientWidth || document.body.clientWidth;
var htmlDom=document.getElementByTagName('html')[0];
htmlDom.style.fontSize=htmlwidth/20+'px'; // 320/20=16;
```
## 去除某物件屬性重複值
```js
const arr = [
  {name:'alex',value:10},
  {name:'alex',value:20},
  {name:'tom',value:30},
  {name:'tom',value:40}
];
const set = new Set();
const result = arr.filter(item => !set.has(item.name) ? set.add(item.name) : false);
console.log(result); 
// [{name: "alex", value: 10}, {name: "tom", value: 30}]
```
## 去除重複的物件
```js
const arr = [
  {name:'a', value:10},
  {name:'a', value:20},
  {name:'a', value:20},
  {name:'b', value:30},
];
const result = [...new Set(arr.map(item => JSON.stringify(item)))].map(item => JSON.parse(item));
console.log(result); 
// [{name: "a", value: 10}, {name: "a", value: 20}, {name: "b", value: 30}]
```
## 取得重複的物件
```js
const arr = [
  {name:'a', value:10},
  {name:'a', value:20},
  {name:'a', value:20},
  {name:'b', value:30},
  {name:'b', value:40},
  {name:'b', value:40}
];
const set = new Set();
const result = arr.filter(item => {
  if (set.has(JSON.stringify(item))) {
    return true;
  } else {
    set.add(JSON.stringify(item));
  }
});
//[{name: "a", value: 20}, {name: "b", value: 40}]
```
### 驗證 Email
```js
function validateEmail(email) {
    var re = /^(([^<>()\[\]\\.,;:\s@"]+(\.[^<>()\[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
    return re.test(String(email).toLowerCase());
}
```
### 将下面数组转成对象，key/value 对应里层数组的两个值 

```javascript
const objLikeArr = [["name", "Jim"], ["age", 18], ["single", true]]; 

const fromPairs = pairs => pairs.reduce((res, pair) => (
    (res[pair[0]] = pair[1]), res), {});
fromPairs(objLikeArr); 

```

### 将多层数组转换成一层数组 

```javascript
var nestedArr = [1, 2, [3, 4, [5, 6]]];
var flatten=arr=>arr.reduce((flat,next)=>{
    return flat.concat(Array.isArray(next)?flatten(next):next)
},[])
flatten(nestedArr)
```

### 生成由隨機整數組成的數組，數組長度和元素大小可自定義 

```javascript
var genNumArr=(length,limit)=>(
	Array.from({length},_=>Math.floor(Math.random()*limit))
)
genNumArr(10,100)
```

### 從長度為 100 萬的隨機整數組成的數組中取出偶數，再把所有數字乘以 3 

```javascript
var bigArr = genNumArr(1e6, 100)
var isOdd=num=>num % 2 == 0
var triple=num=>num * 3
bigArr.forEach((v,i)=>{
    if(isOdds(v)){
        results.push(triples(v))
    }
})
```

## 類數組

```js
var arrayLike = {
    0: 'zhangsan',
    1: 'lisi',
    2: 'zhaoliu',
    length: 3
}
// 無法使用 push... 方法
// 需透過 Array.prototype.push.call(arrayLike,"PUSH")

// 或通過以下方法轉型成Array
Array.prototype.slice.call(arrayLike)
Array.prototype.splice.call(arrayLike,0)
Array.from(arrayLike)
Array.prototype.concat.apply([],arrayLike)
```

## 分頁製作

### 分頁組件的構成

```
《上一頁》《下一頁》 2 個 ： 2
《首頁》《尾頁》 2 個 ： 2
 省略號 2 個 ： 2
 當前頁 1 個 ： 1
 當前頁左右各2個 ：2 * 2
```

```js
var around=2;
var baseCount = 2 + 2 + 2 + 1 + 2 * around;
```

### 怎麼使用baseCount

```
除掉兩邊的上一頁、下一頁兩個按鈕，中間部分一共有11 - 2 = 9個元素
當total < 9 是不需要省略號的
```

### 省略號的作用

#### 只出現在後面

```
當cur <= 首頁(1) + 省略號最小值(2) + cur左邊的值(2)的時候，前面並不會出現省略號，僅在後面出現省略號
startPosition = 1 + 2 + 2
```

#### 只出現在前面

```
endPosition = 尾頁(total) - 省略號最小值(2) - cur右邊的值(2)。
cur大於等於endPosition時只在前面出現省略號。
```

### 其他的位置怎麼顯示

```
分析完省略號的位置，其他的就超級簡單了，還記得剛才那個很重要的baseCount嗎，還是用它。
剛才得到的baseCount是11，因為上一頁和下一頁一直都會存在。我們可以先不用管這倆，只看中間實際用到的9個位置。
我們可以看到當省略號只出現在後面時，中間可用位置的最後兩個一定是 ... 和  total。所以這個省略號前面的還剩下9 - 2  = 7個，給這個變量也取個名字叫surplus，就從1開始顯示到surplus就好了。也是同樣的道理，當省略號只出現在前面時，前面的兩個位置一定是 1 和 ...。所以後面就是從 (total-surplus) 一直到 total 了。
還有就是兩邊都出現省略號，前面兩個肯定是1 和 ...，後面兩個又肯定是**... 和  total**。中間是cur以及其左右兩邊的各around個。
```

```js
const makeResult=(total,cur,around=2)=>{
            let result=[]
            let baseCount=around * 2+1+2+2+2 // 總共元素個數 11
            let surplus=baseCount-4
            let startPosition=1+2+around;
            let endPosition=total-2-around;

            if(total <= baseCount-2){ //全部顯示 不出現省略號
                result =  Array.from({length: total}, (v, i) => i + 1);
            }else{
                if( cur <= startPosition){
                    result=[...Array.from({length:surplus},(v,i)=>i+1),'...',total]
                }else if( cur > endPosition){
                    result=[1,'...',...Array.from({length:surplus},(v,i)=> total-surplus+i+1 )]
                }else{
                    result=[1,'...',...Array.from({length:around*2+1},(v,i)=> cur-around+i ),'...',total]
                }
            }
            return result;
        }
```

