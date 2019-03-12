# ES5

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

function Web(name,age){
    Worker.call(this,name,age);
}
Web.prototype=Worker.prototype;

var w=new Web("Lin",33);
w.run();
w.work()
```

