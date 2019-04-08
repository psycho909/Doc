## 在非同步之下取值的問題
```js
const fs=require("fs");

const getData=()=>{
    fs.readFile("./data.json",(err,content)=>{
        return content;
    })
}

const start=()=>{
    const data=getData(); // undefined
    console.log(data)
}
start();
```

## 解決非同步問題 取值問題

### 1.callback

callback 在完成一個非同步工作之後去執行你給我的函式，並把結果傳入這個函式的參數

```js
const fs=require("fs");

const getData=(callback)=>{
    fs.readFile("./data.json",(err,content)=>{
        callback(content);
    })
}

const start=()=>{
    getData((data)=>{
        console.log(data)
    });
}
start();
```

### 2.Promise

```js
const fs=require("fs");

const getData=()=>{
    return new Promise((resolve,reject)=>{
        fs.readFile("./data.json",(err,content)=>{
            if(err){
                reject(err)
            }
            resolve(content)
        })
    })
}

const start=()=>{
    getData().then((data)=>{
        console.log(data)
    })
}
start();
```

### 3.async && await

1. 指 data資料要等 Promise => await getData() 執行完的結果，才會async到data裡面
2. 當要await時，要搭配一個async function 包起來

```js
const fs=require("fs");

const getData=()=>{
    return new Promise((resolve,reject)=>{
        fs.readFile("./data.json",(err,content)=>{
            if(err){
                reject(err)
            }
            resolve(content)
        })
    })
}

const start= async ()=>{
    const data=await getData(); 
    // 指 data資料要等 Promise => await getData() 執行完的結果，才會async到data裡面
    // 當要await時，要搭配一個async function 包起來
    console.log(data)
}
start();
```

## Fetch + Async Await

```js
var api="https://randomuser.me/api";
        
var getData=async ()=>{
    var res=await fetch(api)
    var data=await res.json()
    return data
}

var start=()=>{
    var data=getData();
    data.then(user=>{
        console.log(user.results)
    })
}
start()
```

## Fetch + Promise

```js
var api="https://randomuser.me/api";

var getData=()=>{
    return new Promise((resolve,reject)=>{
        var data=fetch(api).then(res=>res.json())
        resolve(data)
    })
}
var start=()=>{
    var data=getData();
    data.then(user=>{
        console.log(user)
    })
}
start()
```

