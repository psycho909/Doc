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
```
## 使用reduce找出重複數量
```javascript
var cars = ['BMW','Benz', 'Benz', 'Tesla', 'BMW', 'Toyota'];
var carrList=cars.reduce((obj,name)=>{
    obj[name]=obj[name]?++obj[name]:1;
    return obj
},{})
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