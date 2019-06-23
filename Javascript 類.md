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
function SuperType(name,gender){
    this.name=name;
    this.gender=gender
}
SuperType.prototype.sayName=function(){
	console.log(this.name)
}

//=========================

function SubType(name,gender,age){
    SuperType.call(this,name,gender)
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
SubType.gender="male";
SubType.sayAge();
```

# ES6

## 創建類

```js
class People{
	constructor(name,age){
		this.name=name
		this.age=age
	}
	sayHello(){
		console.log(`Hello My name is ${this.name}`)
	}
}
```

## 繼承

```js
class Player extends People{
	constructor(name,age,skill){
		super(name,age)
		this.skill=skill
	}
	saySkill(){
		console.log(`My Skill is ${this.skill}`)
	}
}
var player=new Player("LBJ",33,"DUNK")
player.saySkill()
```

