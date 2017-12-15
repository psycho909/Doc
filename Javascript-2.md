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