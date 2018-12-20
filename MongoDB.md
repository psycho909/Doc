# MongoDB
```cmd
mongod; // 1.啟動 mongo
mongo; // 2.進入 mongo
```
```js
show dbs; // 顯示出所有數據庫
use <dbname>; // 進入某個數據庫
show collections; // 查看該數據庫 有哪些集合

db; // 可查看目前在哪個庫中

use user; // 當沒有該庫時，會自動創建
db.<collection>.insert({'name':'chen'});
db.<collection>.find();  // 查看
db.<collection>.findOne();
db.<collection>.update({'name':'chen'},{'name':'LNH'}); // 修改
db.<collection>.remove({'name':'chen'}) // 刪除
db.<collection>.drop(); // 刪除整個集合
db.dropDatabase(); //刪除整個庫
```
## 使用JS來寫命令
```js
var userName='jspang';
var timestamp=Date.parse(new Date());
var jsonDatabase={'loginName':userName,'loginTime':timestamp};
var db=connect('log');//use log
db.login.insert(jsonDatabase);

print('[demo]:log print success');
```
## update更新方式
```js
// $set 優雅的更新文件
// db.workmate.update({name:'Wade'},{"$set":{sex:0,age:100}})
// db.workmate.update({name:'Wade'},{$set:{"skill.skillThree":"word"}})

// $unset 刪除 某集合內文件的某個數據
// db.workmate.update({name:'Wade'},{$unset:{age:''}})

// $inc 對於數字計算
// db.workmate.update({name:'Wade'},{$inc:{age:-2}})

// multi 幫所有添加新的屬性
// db.workmate.update({},{$set:{interset:[]}},{multi:true})

// upsert 如果沒有就加上，有的話不修改
// db.workmate.update({name:'faker'},{$set:{age:20}},{upsert:true})

// $push 如同JS push功能
// db.workmate.update({name:'faker'},{$push:{skillFour:'excel'}})

// $ne 查找先，在修改之前先查找，沒有就先進行修改，有則不修改
// db.workmate.update({name:'faker',interset:{$ne:'playgame'}},{$push:{interset:'playgame'}})

// $addToSet $ne升級版
// db.workmate.update({name:'faker'},{$addToSet:{interest:'playGGame'}})

// $each 批量追加
// var newInterests=['sing','code','eat']
// db.workmate.update({name:'faker'},{ $addToSet:{interest:{$each:newInterests}} } )

// $pop  1 => 從數組末端刪除  -1 => 開始位置刪除
// db.workmate.update({name:'faker'},{$pop:{interest:1} })

// 數組定位修改 非應答式操作
// db.workmate.update({name:'faker'},{$set:{"interest.2":"run"}} )
```
## 不等修饰符
1. 小于($lt):英文全称less-than
1. 小于等于($lte)：英文全称less-than-equal
1. 大于($gt):英文全称greater-than
1. 大于等于($gte):英文全称greater-than-equal
1. 不等于($ne):英文全称not-equal
## find查找
```js
// findAndModify
// query:需要查詢的條件
// sort: 排序
// remove:[boolean] 是否刪除查找的文黨
// update: 增加
// new:[boolean] 返回更新前的文黨還是更新後的文黨
// fields: 需要返回的字段
// upsert: 沒有這個值是否增加

var myModify={
    findAndModify:"workmate",
    query:{name:'faker'},
    update:{$set:{age:100}},
    new:true
}
```
## find查找
1. $in 一個key多個value
1. $nin $in的相反
1. $or ( 如同 || ) 它用来查询多个键值的情况，就比如查询同事中大于30岁或者会做PHP的信息。主要区别是两个Key值。$in修饰符是一个Key值，这个需要去比较记忆。
1. $and ( 如同 && ) $and用来查找几个key值都满足的情况，比如要查询同事中大于30岁并且会做PHP的信息，这时需要注意的是这两项必须全部满足。当然写法还是比较简单的。只要把上面代码中的or换成and就可以了。
## find數組查找
1. $all 查找多個數組資料，如同 &&
1. $in 查找多個數組資料，如同  ||
1. $size 查找個數
1. $slice 有时候我并不需要显示出数组中的所有值，而是只显示前两项，比如我们现在想显示每个人兴趣的前两项，而不是把每个人所有的兴趣都显示出来。
```js
// db.workmate.find(
//     {"skill.skillOne":"HTML+CSS"},
//     {name:true,"skill.skillOne":true,_id:false}
// )
// db.workmate.find(
//     {age:{$lte:30,$gte:25}},
//     {name:true,"skill.skillOne":true,age:true,_id:false}
// )

// var startDate=new Date('01/01/2018');
// db.workmate.find(
//     {regeditTime:{$gt:startDate}},
//     {name:true,"skill.skillOne":true,age:true,_id:false}
// )
db.workmate.find(
    {interest:{$all:['玩游戏','篮球']}},
    {name:true,interest:true,age:true,_id:false}
)
```
## find 進階用法
```js
// 分頁顯示2 年齡從小到大
// limit：返回的数量，后边跟数字，控制每次查询返回的结果数量
// skip:跳过多少个显示，和limit结合可以实现分页。
// sort：排序方式，从小到大排序使用1，从大到小排序使用-1。
db.workmate.find(
    {},
    {name:true,age:true,_id:false}
).limit(2).skip(2).sort({age:1})
```
### find $where
> 消耗效能大
```js
// $where 它是一个非常强大的修饰符，但强大的背后也意味着有风险存在。它可以让我们在条件里使用javascript的方法来进行复杂查询。我们先来看一个最简单的例子，现在要查询年龄大于30岁的人员。
// age > 30 程式員
db.workmate.find(
    {$where:"this.age > 30"},
    {name:true,age:true,_id:false}
)
```
## 索引使用
> 可增加查找速度
```js
// 查找索引
// db.randomInfo.getIndexes()
// 建立索引
// db.randomInfo.ensureIndex({username:1})
```
## 管理:用户的创建、删除与修改
```js
db.createUser({
    user:"jspang",
    pwd:"123456",
    customData:{
        name:'技术胖',
        email:'web0432@126.com',
        age:18,
    },
    roles:[
        {
            role:"readWrite",
            db:"company"
        },
        'read'
    ]
})
```
```js
// 查找用户信息
db.system.users.find()
// 删除用户：
db.system.users.remove({user:"jspang"})
// 建权
db.auth("jspang","123456")
```
> 重新啟動
```js
// 重新啟動
// mongod --auth

// 登录
// mongo -u jspang -p 123456 127.0.0.1:27017/admin
```
## MongoDB 管理：备份和还原
```js
// 數據 備份
mongodump
    --host 127.0.0.1
    --port 27017
    --out D:/databack/backup
    --collection myCollections
    --db test
    --username username
    --password password
```
```js
// 數據 還原
mongorestore
    --host 127.0.0.1
    --port 27017
    --username username
    --password password
    <path to the backup>
```
```js
var text = 'Nodejs';//动态传入的变量
Article.find({ content: { $regex: text, $options: 'i' }}, function (err, docs) {});
// $options选项值：

// i 大小写不敏感
// m $regex包含正则^,$符号的表达式
// x 忽略空格
// s 允许逗点匹配所有字符串
```

## NPM mongoose

### 連接 -> 定義模式(Schema) -> 生成Model -> 生成Model實例

```js
npm i -S mongoose
```
### 連接數據庫

```js
import mongoose from 'mongoose'
mongoose.connect("mongodb://localhost/accounts")
var db=mongoose.connection;
```

### 定義一個`Schema`模式

```js
var userSchema=new mongoose.Schema({
    username:String,
    email:String
})
```

### 將`Schema`發佈為`Model`

```js
var userModel=mongoose.model("user",userSchema)
```

### 最後`model`可以對數據庫進行操作`find`

```js
userModel.find((err,data)=>{
    if(err) return console.log(err)
    console.log("資料: "+data)
})
```

### 查找

#### 查找`find()`全部

```js
userModel.find({}).then((result)=>{})
.catch((err)=>{})
```

#### 查找數據`find()`

```js
userModel.find({
    username:"AAA",
    age:{$gt:17,$lt:66}, //限定年齡範圍
    likes:{$in:["a",'b']} // 限定喜好必須要在這個數組中
}).
limit(10). // 限定取出數量
sort({occupation:-1}). // 1表示正序，-1表示逆序
select({ name: 1, occupation: 1 }).  // 選擇取出的字段，1表示取出，0表示不取出，如果不需要id，需要顯式寫{_id : 0}
exec(callback);  // 這裡使用then也行，完全支持Promise，但是查詢不是Promise，這個要注意，後面會說
```
#### 更好查找方式`find()`

```js
// Using query builderPerson.
userModel.find({
    username:"AAA",
    age:{$gt:17,$lt:66}, //限定年齡範圍
    likes:{$in:["a",'b']} // 限定喜好必須要在這個數組中
}).
where('name.last').equals('Ghost').
where('age').gt(17).lt(66).
where('likes').in(['vaporizing', 'talking']).
limit(10).
sort('-occupation').
select('name occupation').
exec(callback);
```

#### 查詢某一數據`findOne()`

```js
userModel.findOne({_id:id},(err,data)=>{
    if(err) return console.log(err)
    console.log(data)
})
```

#### 統計查詢的文檔數據`countDocuments`

```js
userModel.find({})
.countDocuments()
.then()
```

### 插入數據

#### 插入數據`create()`

```js
var newUser=[{
	username:"AAA",
    email:"aaa@gmail.com"
}]
userModel.create(newUser,(err)=>{
    if(err) return console.log(err)
    console.log("數據插入成功")
})
```

#### 插入多條數據`create()`

```js
var newUsers=[{
	username:"AAA",
    email:"aaa@gmail.com"
},{
    username:"BBB",
    email:"bbb@gmail.com"
}]
userModel.create(newUser,(err)=>{
    if(err) return console.log(err)
    console.log("數據插入成功")
})
```

#### 以下不過時不用

#### 插入數據 `save()`

```js
var newUser=new userModel({
    username:"AAA",
    email:"aaa@gmail.com"
})
newUser.save((err,data)=>{
    if(err) return console.log(err)
    console.log("數據插入成功")
})

```



### 更新數據

1.  `update()`
2.  `updateOne()`
3.  `updateMany()`

#### 更新數據update`

```js
var conditions={_id:id};
var query={$set:{username:"BBB",email:"bbb@gmail.com"}}
userModel.update(conditions,query,(err,result)=>{
    if(err) return console.log(err)
    console.log("更新成功")
})
```

#### 更新數據`updateOne`

```js
var conditions={_id:id};
var query={$set:{username:"BBB",email:"bbb@gmail.com"}}
userModel.updateOne(conditions,query,(err,result)=>{
    if(err) return console.log(err)
    console.log("更新成功")
})
```

#### 更新數據`updateMany`

```js
var conditions={_id:id};
var query={$set:{username:"BBB",email:"bbb@gmail.com"}}
userModel.updateMany(conditions,query,(err,result)=>{
    if(err) return console.log(err)
    console.log("更新成功")
})
```

#### 以下過時不用

#### 更新數據`findByIdAndUpdate`

```js
var query={$set:{username:"BBB",email:"bbb@gmail.com"}}
userModel.findByIdAndUpdate(id,query,{new:true},(err,result)=>{
	if(err) return console.log(err)
    console.log("更新成功")
})
```

#### 更新數據findById`

```js
userModel.findById(id,(err,data)=>{
    if(err) return console.log(err)
    data.username="BBB"
    data.email="bbb@gmail.com"
    data.save((err)=>{
        if(err) return console.log(err)
        console.log("更新成功")
    })
})
```

### 刪除數據

#### 刪除數據`deleteOne()`

```js
var conditions={_id:id}
userModel.deleteOne(conditions,(err,data)=>{
    if(err) return console.log(err)
    console.log("刪除成功")
})
```

#### 刪除數據`deleteMany()`

```js
var conditions={_id:id}
userModel.deleteMany(conditions,(err,data)=>{
    if(err) return console.log(err)
    console.log("刪除成功")
})
```

### 高級查詢

#### 查詢特定字段

>   查找`age`字段

```js
User.find({},'age').then((docs)=>{
  console.log(docs);
}).catch((err)=>{})
```

#### 條件判斷查詢

>   查詢年齡大於等於18歲的所有數據

```js
let conditions = {age:{$gte:18}};
User.find(conditions).then((docs)=>{
  console.log(docs);
});
```

#### 游標操作和排序

`skip()`和`limit()`

```js
User.find().skip(2).limit(2).then((docs)=>{
    console.log(docs);
});
```

排序`sort()`

>   sort 排序 1：升序 / -1：降序

```js
 User.find().sort({age:1}).skip(3).limit(3).then((docs)=>{
    console.log(docs);
  });
```



## NPM mongodb

### Basic

```js
npm i -S mongodb
```

```js
import express from 'express'
import mongodb from 'mongodb'

const app=express()
const MongoClient=mongodb.MongoClient
const dbUrl="mongodb://localhost/"

MongoClient.connect(url,(err,client)=>{
    // 獲取 database
    const db=client.db('crud')
    
    // 獲取全部資料
    app.get('/api/games',(req,res)=>{
        // 獲取 table
        db.collection('games').find({}).toArray((err,games)=>{
            res.json({games})
        })
    })
})
```

### `find()`

```js
app.get('/api/games',(req,res)=>{
    db.collection('game').find({}).toArray((err,games)=>{
        res.json({games})
    })
})
```

### `findOne()`

```js
app.get('/api/games/:_id',(req,res)=>{
    db.collection('game').findOne({_id:new mongodb.ObjectId(req.params._id)},(err,games)=>{
        res.json({game})
    })
})
```

### `deleteOne()`

```js
app.delete('/api/games/:_id',(req,res)=>{
    db.collection('game').deleteOne(
        {_id:new mongodb.ObjectId(req.params._id)},
        (err,game)=>{
            res.json({})
        }
    )
})
```

### findOneAndUpdate()

```js
app.put('/api/games/:_id',(req,res)=>{
    db.collection('game').findOneAndUpdate(
    	{_id:new mongodb.ObjectId(req.params._id)},
        {$set:{title,cover}},
        {returnOriginal:true},
        (err,result)=>{
            // result.value 修改前的資料
            res.json({game:result.value})
        }
    )
})
```

### `insert()`

```js
app.post('/api/games',(req,res)=>{
    db.collection('games',(err,collection)=>{
    collection.insert(
        {
            title:req.body.title,
            cover:req.body.cover
        },(err,result)=>{
            // result.ops[0] 輸出 成功insert的資料
            res.json({info:result.ops[0]})
        }
    )
})
})
```

