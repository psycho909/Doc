#js #ts
## 安裝與使用

```js
npm i -g typescript
npm i -g ts-node
```

1. 副檔名`.ts`
2. 轉檔`tsc 檔名.ts`
3. 會轉檔成`js`
4. 遇到錯誤會停止轉檔
5. 自動編譯
   1. 安裝`ts-node`
   2. 安裝`nodemon`
   3.  執行`nodemon --exec ts-node [檔名].ts`

## 類型定義

```typescript
// basic
let a: number;
let b: boolean;
let c: string;

// array
let myArr: number[]=[1,2,3];
let myArr: string[]=["a","b","c"];
let myArr: Array<number>=[1,2,3];

// tuple
let my_tuple: [string,number]=["string",123];

// function
// function fn(x:類型,y:類型=默認值):返回值類型{ return x+y; }
function add(a:number,b:number=10):number{
    return a+b;
}
// 沒有返回值:void
// ?: 為可選參數，一般放到參數後面
// 默認參數(=) 可以不指定類型，會自動判斷和計算
let add=(a:number,c=10,b?:number):void=>{
    if(b){
        console.log(a+b+c)
    }else{
        console.log(a)
    }
}

// Rest ...num:number[]
function add(a:number,b:number,...num:number[]){
    if(c){
        var sum=num.reduce((p,n)=> p+n,0);
        return a+b+sum;
    }else{
        return a+b;
    }
}

// :any 慎用any類型，如果要限定類型得自己判斷
// :any part2
function isNumber(value:any):value is number{
    // 可以進行進一步判斷
    return typeof value === 'number'
}
function isNumber(value:string):value is string{
    // 可以進行進一步判斷
    return typeof value === 'string'
}
let log=(value:any)=>{
    if(isNumber(value)){
        return `ur number is ${value}`
    }
    if(isString(value)){
        return `ur string is ${value}`
    }
    throw new Error(`Expected string or number,gor ${value}`);
}

let anyArr:any[];
anyArr=[1,'b'];

let log=(value:string | number)=>{
    if(isNumber(value)){
        return `ur number is ${value}`
    }
    if(isString(value)){
        return `ur string is ${value}`
    }
    throw new Error(`Expected string or number,gor ${value}`);
}

```

## TypeScript 類

```typescript
// 模板
class Person{
    // 定義數據
    firstName:string;
    lastName:string;
    play_count:number;

    // this 指向生成的 Object 本身
    constructor(firstName:string,lastName:string){
        this.firstName=firstName;
        this.lastName=lastName;
        this.play_count=0;
    }
    
    increase_play_count(){
        this.play_count+=1;
    }

    sayName(){
        return `Hello my name is ${this.firstName} ${this.lastName}`
    }
    display_play_count(padding:string="****"){
        return `${this.play_count} 次數 ${padding}`;
    }

    great(){
        console.log("great!")
    }

    otherGreat(){
        this.great();
        console.log("otherGreat!")
    }
}

// 生成一個Object
let p1=new Person("LB","James");
console.log(p1.display_play_count("----------"))
console.log(p1.increase_play_count())
console.log(p1.display_play_count("----------"))
console.log(p1.otherGreat())
```

### 繼承

```typescript
// 模板
class Person{
    // 定義數據
    firstName:string;
    lastName:string;

    // this 指向生成的 Object 本身
    constructor(firstName:string,lastName:string){
        this.firstName=firstName;
        this.lastName=lastName;
    }
    
    great(){
        console.log("great!")
    }

    otherGreat(){
        this.great();
        console.log("otherGreat!")
    }
}

// 繼承了父類的數據和行為，就是屬性和方法
class Programmer extends Person{
    constructor(firstName,lastName){
        super(firstName,lastName)
    }
    great(){
        console.log("Hello Programmer")
    }

    // super 代表父類
    greatLikeNormalPeople(){
        super.great()
    }
}

let p1=new Programmer("LB","James");
//調用方法時，先找自己本身對象的方法，如果沒有，會找父類的
console.log(p1.otherGreat())
console.log(p1.greatLikeNormalPeople())
```

### public,private,protected

1. Public 公有的:任何屬性和方法都可以再生成的對象中調用，繼承的對象也能調用，默認屬性
2. Private 私有的:只有在內部對象內才能訪問，生成的對象調用，要調用私有方法和屬性，可以在class裡定義public的方法來調用。
   1. 繼承的對象也是不能夠直接用生成的對象來訪問
   2. 子類繼承的時候也可以繼承私有屬性和方法，也是要通過父類class裡定義public方法來調用
3. Protected 受保護:只有在內部class還有子類才能訪問，生成的對象訪問不了，要調用私有方法和屬性，可以在class裡定義public的方法來調用。
   1. 繼承的對象也是不能夠直接用生成的對象來訪問
   2. 子類繼承的時候也可以繼承保護屬性和方法，可以通過子class裡定義public方法來調用
4. Private 和 Protected 異同:
   1. 繼承的方法一樣的表現形式，可以訪問繼承過來的Public屬性和方法
   2. 子類可以內部訪問 Protected 屬性和方法，不可以內部訪問 Private屬性和方法，子類也不可外部訪問 Protected 和 Private
   3. 子類定義的方法，只能訪問public和protected的方法和屬性，不能訪問繼承過來的private屬性和方法，如果要訪問父類的private屬性和方法，可以通過繼承過來的Public Protected 方法在內部來訪問

### constructor

1. 如果申明為`protected`或者`private`，當前類不能new
2. 當父類申明為`protected`，子類重寫constructor方法後可以 `new`(子類可以new)
3. 如果父類申明為`private`，子類不能`new`和`extends`
4. `super()`在constructor方法中是調用父類的構造方法

#### 作用

1. 當不想被實例化，而只想讓子類繼承後實例化，可以申明為protected

### 靜態方法

1. 通過類似`Person.`加方法或屬性來調用(比如`Person.age`)
2. 默認為public
3. 如果是protected或private的話，當前的類都不能調用，通過public的靜態方法來調用
4. 如果父類是protected或private的話，子類也能繼承所有的靜態方法和屬性，子類還是不能調用protected或private的方法和屬性，只能通過繼承的public的方法來調用

```js
// 模板
class Person{
    // 定義數據
    private firstName:string;
    protected lastName:string;
    protected age:number;

    // 靜態屬性
    public static job:string="Progammer";
    private static gender:string="male";
    protected static hobby:string="games";

    protected static setStaticAge(){
        return `my age is ${Person.job}`;
    }
    public static getStaticAge(){
        console.log(Person.setStaticAge())
    }
    public static getGender(){
        console.log(Person.gender)
    }
    // this 指向生成的 Object 本身
    protected constructor(firstName:string,lastName:string,age:number){
        this.firstName=firstName;
        this.lastName=lastName;
        this.age=age;
    }
    
    // 通過此方法 去修改內部private的屬性
    setFirstNmae(firstName):void{
        this.firstName=firstName
    }
    protected getFirstNmae(){
        console.log(`firstName is ${this.firstName}`);
    }

    setLastNmae(lastName):void{
        this.lastName=lastName
    }
    protected getLastNmae(){
        console.log(`lastName is ${this.lastName}`);
    }

    private great(){
        console.log("great!")
    }

    public otherGreat(){
        this.great();
        console.log("otherGreat!")
    }

    public getJob(){
        console.log(Person.job);
    }
}

class Programmer extends Person{
    constructor(firstName:string,lastName:string,age:number){
        super(firstName,lastName,age)
    }
    public getFullName():string{
        console.log(this.getLastNmae())
        console.log(this.age)
        return `My name is ${this.getFirstNmae()} ${this.getLastNmae()}`;
    }
    public getGreat(){
        super.getLastNmae()
    }
    public getExtendsJob(){
        console.log(Programmer.job)
    }
    public getExtendsHobby(){
        console.log(Programmer.hobby)
    }
}

// let p=new Person("LB","James",33);
console.log(Person.getStaticAge());
console.log(Person.getGender());
let p1=new Programmer("LB","James",33);
console.log(Programmer.job)
console.log(Programmer.getStaticAge())
console.log(Programmer.getGender())
console.log(p1.getExtendsJob())
console.log(p1.getExtendsHobby())
// p1.setFirstNmae("Harris");
// p1.setLastNmae("Tobis");
// console.log(p1.getLastNmae())
// console.log(p1.getFullName())
// console.log(p1.otherGreat())
// console.log(p1.getJob())
```

### 只讀屬性:只讀屬性，不能修改

```typescript
// 只讀屬性
readyonly name:string="LBJ";
```

## enum:枚舉類型

1. 枚舉類型
2. 他的值是數字序號，從0開始
3. 代碼可讀性強
4. 可能會常用於下拉框等應用

```typescript
enum DaysOfTheWeek{
    SUN,MON,TUE,WED,THU,FRI,SAT
}

let day: DaysOfTheWeek;
day=DaysOfTheWeek.MON;
console.log(day); // 1
```

## TypeScript 函數

### 函數的定義

```ts
// 函數聲明
function fn(x:number,y:number):number{}

var fn=(x:number,y:number):number=>{}
```





## 類型別名

1. 定義類別別名 type alias
2. 以後可以用 Name 來代替 string 類型
3. 不可以重複定義別名

```typescript
// 定義類別別名 type alias
// 以後可以用 Name 來代替 string 類型
type Name=string;

type User={
    name:string;
    age:number;
}

const user:User={
    name:"LBJ",
    age:23
}
```



## interface 接口

接口定義：接口是對傳入參數進行約束；或者對類裡面的屬性和方法進行聲明和約束，實現這個接口的類必須實現該接口裡面屬性和方法；typescript中的接口用interface關鍵字定義。

接口作用：接口定義了某一批類所需要遵守的規範，接口不關心這些類的內部狀態數據，也不關心這些類裡方法的實現細節，它只規定這批類裡必須提供某些方法，提供這些方法的類就可以滿足實際需要。 typescrip中的接口類似於java，同時還增加了更靈活的接口類型，包括屬性、函數、可索引和類等。

內容概述：接口分類：（屬性接口、函數類型接口、可索引接口、類類型接口），接口的繼承

1. 可以互相繼承

### 屬性接口

```ts
interface FullName{
    firstName: string; // 注意;结束
    secondName: string;
    age?: number // 接口的可选属性用?
}

function printFullName(name:FullName) {
    // 传入对象必须包含firstName和secondName，可传可不传age
    return name
}
var obj = {
    firstName:'小',
    secondName:'明',
    age: 20
}
console.log(printFullName(obj))
```

### 函数类型接口

```ts
interface encrypt{
    (key: string, value: string): string; // 传入的参数和返回值的类型
}

var md5:encrypt = function(key:string, value:string):string{
    // encrypt对加密方法md5进行约束，同时md5方法的参数和返回值类型和encrypt要保持一致
    return key + value
}

console.log(md5('name', '小明'))
```

### 可索引接口

```ts
// 对数组的的约束
interface UserArr{
    // 索引为number，参数为string
    [index:number]: string
}
var userarr:UserArr = ['a', 'b']
console.log(userarr)
```

### 类类型接口

```ts
interface Animal{
    // 对类里面的属性和方法进行约束
    name:string;
    eat(str:string):void;
}
// 类实现接口要用implements关键字，必须实现接口里面声明的方法和属性
class Cat implements Animal{
    name:string;
    constructor(name:string){
        this.name = name
    }
    eat(food:string){
        console.log(this.name + '吃' + food)
    }
}
var cat = new Cat('小花')
cat.eat('老鼠')
```

### 接口的继承

```ts
interface Animal {
    eat(): void;
}
// 继承Animal接口，则实现Person接口的类必须也实现Animal接口里面的方法
interface Person extends Animal {
    work(): void;
}

class Programmer {
    public name: string;
    constructor(name: string) {
        this.name = name;
    }
    coding(code: string) {
        console.log(this.name + code)
    }
}

// 继承类并且实现接口
class Web extends Programmer implements Person {
    constructor(name: string) {
        super(name)
    }
    eat() {
        console.log(this.name + '吃')
    }
    work() {
        console.log(this.name + '工作');
    }
}

var w = new Web('小李');
w.eat();
w.coding('写ts代码');
```



### 基本接口

```typescript
// 接口
// 傳過來的參數必須包含接口定義的屬性和方法
interface Named{
    // 屬性
    name: string;

    // 方法
    // 沒有方法體
    // 具體的對象中實現方法體
    print(name:string):void;
}

// 函數
const sayName=(o:Named)=>{
    o.print(o.name);
    // console.log(o.name)
}

// 對象
const perosn={
    age:27,
    name:"sadfk",
    print:(name)=>{
        console.log(name)
    }
}

class SuperMan{
    name:string;
    constructor(name){
        this.name=name;
    }
    print(name){
        console.log(name)
    }
}

var s=new SuperMan("BATMAN");

sayName(perosn)
sayName(s)
```

### 類實現接口

```typescript
// 接口
// 支付接口
interface Person{
    name: string;
    greet();
}

const sayName=(o:Person)=>{
    console.log(o.greet())
}

// 實現接口


// 類實現接口
class Employee implements Person{
    name:string;

    greet(){
        return "Hello greet";
    }
}

class Customer implements Person{
    name:string;
    email:string;
    greet(){
        return "Hello greet";
    }
}

let cu=new Customer();
sayName(cu);

// 支付接口
interface Pay{
    post():void;
}

// 可能會發送 http 請求
// 真正支付的請求
const do_pay=(pay:Pay)=>{
    // 有一些邏輯
    pay.post();
}

// A支付
class WePay implements Pay{
    // 調A支付的接口
    post(){

    }
}

// B支付
class AliPay implements Pay{
    // 調B支付的接口
    post(){

    }
}

let we_pay:Pay=new WePay();
let ali_pay:Pay=new AliPay();

do_pay(we_pay)
```

### 函數接口

```typescript
interface Person{
    first_name?:string;
    last_name?:string;

    print(callback:PrintCallback):void;
}

interface PrintCallback{
    // 可以簡單理解為匿名函數
    (success:boolean,sname:string):void
}

let printCallback:PrintCallback;
printCallback=(suc:boolean,name:string):void=>{
    if(suc){
        console.log(`Hello ! ${name}`)
    }else{
        console.log("no")
    }
}

printCallback(true,"Chen");

let person:Person={
    first_name:"JD",
    last_name:"BB",
    print:(callback:PrintCallback):void=>{
        callback(true,"KBH")
    }
}

person.print(printCallback)
```

## TypeScript 泛型

泛型：很多時候，類型是寫死的，不利於復用，泛型可以簡單理解為給類型這種值設置變量，解決類、接口、方法的複用性，以及對不特定數據類型的支持。

### 基本使用

```ts
function dataT<T>(value:T):T{
    return value
}

console.log(dataT<string>('1'));
```

### 泛型類

```ts
class MinClass<T>{
    list:T[]=[];
    add(num:T){
        this.list.push(num);
    }
    min():T{
        var minNum=this.list[0];
        for(var i=0;i<this.list.length;i++){
            if(minNum>this.list[i]){
                minNum=this.list[i]
            }
        }
        return minNum;
    }
}

var m=new MinClass<number>();
m.add(3);
m.add(1);
console.log(m.min())
```

### 泛型街口

```ts
interface ConfigFnOne{
    <T>(value:T):T;
}

var setDataOne:ConfigFnOne=function<T>(value:T):T{
    return value;
}

console.log(setDataOne<string>('SSS'));
```

