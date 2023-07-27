#js 
```js
var Obj={
    result:0,
    addNumber:function(a,b){
        this.result=a+b;
        return this;
    },
    mulitplyNumber:function(a){
        this.result=this.result*a;
        return this;
    },
    divideNumber:function(a){
        this.result=this.result/a;
        return this;
    }
};

Obj.addNumber(10,20).mulitplyNumber(10).divideNumber(10);
console.log(Obj.result)
```

