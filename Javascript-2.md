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
cat.sayCatName(); // undefined
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
firstElementChild : 指向第一個子元素; firstChild的元素版
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