# ES6
- [ES6](#es6)
    - [<span id="object">ES6遍歷`Object`的新方法</span>](#span-idobjectes6%E9%81%8D%E6%AD%B7object%E7%9A%84%E6%96%B0%E6%96%B9%E6%B3%95span)
        - [`Object.keys()`](#objectkeys)
        - [`Object.values()`](#objectvalues)
        - [`Object.entries()`](#objectentries)
    - [`For of`](#for-of)
    - [<span id="assign">`Object.assign`可用來複製Object</span>](#span-idassignobjectassign%E5%8F%AF%E7%94%A8%E4%BE%86%E8%A4%87%E8%A3%BDobjectspan)
    - [<span id="pad">`pad`字符串填充</span>](#span-idpadpad%E5%AD%97%E7%AC%A6%E4%B8%B2%E5%A1%AB%E5%85%85span)
        - [`padStart`和`padEnd`例子](#padstart%E5%92%8Cpadend%E4%BE%8B%E5%AD%90)
    - [<span id="use-class">`Class`基礎物件導向</span>](#span-iduse-classclass%E5%9F%BA%E7%A4%8E%E7%89%A9%E4%BB%B6%E5%B0%8E%E5%90%91span)
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
> `padStart`在字符串開頭填充一組字符串
```javascript
'pig'.padStart(5) //'   pig'
'pig'.padStart(5,'a') //'aaapig'
```
> `padEnd`在字符串末尾填充一組字符串
```javascript
'pig'.padEnd(5) //'pig   '
'pig'.padEnd(5,'a') //'pigaaa'
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