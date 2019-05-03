# ES6

## <span id="object">ES6遍歷`Object`的新方法</span>
### `Object.keys()`
> `Object.keys()`是一種跌代對象並返回對向所有key的方法
```javascript
var pop = {
    tokyo: 37833000,
    delhi: 24953000,
    shanghai: 22991000
}
Object.keys(pop)
//['tokyo', 'delhi', 'shanghai']
```
### `Object.values()`
> Object.values()跌代對象並返回對象的values
```javascript
Object.values(pop)
//[37833000, 24953000, 22991000]
```
### `Object.entries()`
> Object.entries()遍歷對象，並同時返回key和values
```javascript
Object.entries(pop)
// [['tokyo', 37833000],['delhi', 24953000],['shanghai', 22991000]]
```
## `For of`
> 可使用ES6 `for of`搭配Object.entries(),來遍歷
```javascript
for(let [key,value] of Object.entries(obj)){
    console.log(`${key} ${value}`)
}
```
## <span id="assign">`Object.assign`可用來複製Object</span>
> Object.assign()通常使用來複製Object
```javascript
var src={a:1,b:2,c:3}
var cloneA=Object.assign({},src)
```
## <span id="pad">`pad`字符串填充</span>
> `pad`方法可以在原來的字符串上填充
> `padStart`在字符串開頭填充一組字符串，可以用於補充01,02,03,04,05...
```javascript
'1'.padStart(2,0) //'01'
'1'.padStart(5,'0') //'00005'
```
> `padEnd`在字符串末尾填充一組字符串
```javascript
'1'.padEnd(2,0) //'10'
'1'.padEnd(5,0) //'10000'
```
### `padStart`和`padEnd`例子

> 可以使用`padStart`和`padEnd`來格式化
```javascript
var data={
    'bbbbbb':'78/50',
    'cds':'99/100',
    'swwwww':'66/30'
}
Object.entries(data).map(([name,num])=>{
	console.log(`Name: ${name} Num: ${num}`)
})
//Name: bbbbbb Num: 78/50
//Name: cds Num: 99/100
//Name: swwwww Num: 66/30
Object.entries(data).map(([name,num])=>{
	console.log(`Name: ${name.padEnd(16)} Num: ${num}`)
})
//Name: bbbbbb           Num: 78/50
//Name: cds              Num: 99/100
//Name: swwwww           Num: 66/30
```
## <span id="use-class">`Class`基礎物件導向</span>
```javascript
class Person{
    //需先使用constructor
    //自定義對象類型的屬性
    constructor(name,age){
        this.name=name;
        this.age=age;
    }
    //自定義方法
    sayName(){
        return this.name+' Hi!'
    }
    sayAge(){
        return 'My age '+this.age
    }
}
var person1=new Person('Mark','42')
person1.sayName();//Mark Hi!
//如需繼承父類屬性和方法，需使用extends
class newPerson extends Person{
    //需使用constructor並定義所需繼承的屬性，並添加
    constructor(name,age,skill){
        //並一定要使用super()並輸入對應的屬性
        super(name,age)
        this.skill=skill
    }
    //增加自用方法
    saySkill(){
        return `My name is ${this.name},skill ${this.skill}`
    }
}
var person2=new newPerson('Joe','22','sleep')
//可使用父類的方法
person2.sayName()
//可使用自定義的方法
person2.saySkill()
```
## Map和Set
### Map
> `Map`是一組鍵值的結構<br/>
> 例子:假設一個成績表
```javascript
var names = ['Michael', 'Bob', 'Tracy'];
var scores = [95, 75, 85];
```
> 给定一个名字，要查找对应的成绩，就先要在names中找到对应的位置，再从scores取出对应的成绩，Array越长，耗时越长。
>
> 如果用Map实现，只需要一个“名字”-“成绩”的对照表，直接根据名字查找成绩，无论这个表有多大，查找速度都不会变慢。用JavaScript写一个Map如下：
```javascript
var m = new Map([['Michael', 95], ['Bob', 75], ['Tracy', 85]]);
m.get('Michael'); // 95
```
> 初始`Map`需要一個二為數組，或者直接初始化一個空`Map`。
```javascript
var m=new Map();//空Map
m.set('Adam',95);//添加新的key-value
m.has('Adam')//是否存在key
m.get('Adam')//67
m.delete('Adam')//刪除key 'Adam'
m.get('Adam')//undefined
```
#### 遍歷

##### 方法1

```js
for(var i of m.keys()){
	console.log(i)
}
for(var i of m.values()){
	console.log(i)
}
for(var [i,v] of m.entries()){
	console.log(i,v)
}
```

##### 方法2

```js
[...m.keys()]
[...m.values()]
[...m.entries()]
[...m]
```



### Set 不能重複，可使用於不可重複的值設定

1. `Set`和`Map`类似，也是一组key的集合，但不存储value。由于key不能重复，所以，在`Set`中，没有重复的key
2. 要创建一个`Set`，需要提供一个`Array`作为输入，或者直接创建一个空`Set`：
3. 可用於去除重複Array
4. 重复元素在Set中自动被过滤

```javascript
var s = new Set();
s.add(1) // 增加
s.add(2)
s.has(2) // 判斷有否key值
s.delete(1) // 刪除指定key
s.size // 取出長度
s.clear() // 清除全部
```
```js
let set = new Set(['red', 'green', 'blue']);

for (let item of set.keys()) {
  console.log(item);
}
// red
// green
// blue

for (let item of set.values()) {
  console.log(item);
}
// red
// green
// blue

for (let item of set.entries()) {
  console.log(item);
}
// ["red", "red"]
// ["green", "green"]
// ["blue", "blue"]

[...set]
// red
// green
// blue
```



## 解构赋值

> 从ES6开始，JavaScript引入了解构赋值，可以同时对一组变量进行赋值。
>
> 对数组元素进行解构赋值
```javascript
var [x,y,z]=['js','java','php'];
console.log(x,y,z)//js,java,php
```
>从一个对象中取出若干属性，也可以使用解构赋值，便于快速获取对象的指定属性
```javascript
var person = {
    name: '小明',
    age: 20,
    gender: 'male',
    passport: 'G-12345678',
    school: 'No.4 middle school'
};
var {name, age, passport} = person;
console.log(name,age,passport)//小明,20,G-12345678
```
### 使用场景
> 解构赋值在很多时候可以大大简化代码。例如，交换两个变量x和y的值，可以这么写，不再需要临时变量
```javascript
var x=1,y=2;
[x,y]=[y,x]
```
> 快速获取当前页面的域名和路径
```javascript
var {hostname:domain, pathname:path} = location;
```

## 產生多個預設Array

### Array.from()產生

```js
Array.from(new Array(N),(v,i)=>i)
```



### `new Array().fill()自動產生多個預設array`

```js
var arr=new Array(6).fill(0);
// [0,0,0,0,0,0]
```

