# ES5

## 創建類

```js
function Worker(name,age){
    this.name=name;
    this.age=age;
    this.run=function(){
        console.log(`${this.name} is Runing`)
    }
}

Worker.prototype.sex="Male";
Worker.prototype.work=function(){
    console.log(`${this.name} is Work`);
}

var ww=new Worker("Chen","33");
ww.work()

```

## 繼承

```js
function SuperType(name){
    this.name=name;
}
SuperType.prototype.sayName=function(){
	console.log(this.name)
}

//=========================

function SubType(name,age){
    SuperType.call(this,name)
    this.age=age
}
SubType.prototype=new SuperType();
SubType.prototype.constructor=SubType;

SubType.prototype.sayAge=function(){
    console.log(this.age)
}

var subType=new SubType();
SubType.name="LBJ";
SubType.age="31";
SubType.sayAge();
```

