# Node
## Node套件推薦
```javascript
nodemon //nodemon app.js 在update app.js時頁面自動刷新
body-parser // 可以把前端表單資料傳送到後端去
```

## Express
```javascript
npm i -g express
var express=require('express');
var app=express();
```
### params 讀取路由的資料
> 可以取得網址路徑上的訊息
>
> `params`會去讀取路由的資料
```javascript
app.get('/user/:name/:date',function(req,res){
    // params - 取得指定路徑
    // http://localhost:3000/user/chen/1020722/
    var myName=req.params.name; 
    onsole.log(req.params) // { name: 'chen'.date: '1020722'}
    console.log(myName) // chen
})
```
### query 抓取網址的參數
> `query` 抓取網址的參數
```javascript
app.get('/user/:name?q=POE&limit=30',function(req,res){
    // query - 取得網址參數
    // http://localhost:3000/user/chen?q=POE&limit=30&q=POE
    var limit=req.query.limit; // 30
    var q=req.query.q; // POE
    console.log(req.query) // {limot:30,q:'POE'}
})
```
### Middleware 中介軟體
#### use next()
> `use()`&&`next()`****
>
> 可先設定use()作為守門員處理一些router驗證狀態
```javascript
// 設定守門員
// use()
app.use((req,res,next)=>{
  console.log('有人進來了')
  next() // 進入下一個關卡
})
app.use((req,res,next)=>{
  console.log('有人進來了2')
  next() // 進入下一個關卡
})
// 因為設定了use() 守門員
// 但沒有指定 next() 進入下一個關卡
// 頁面會卡住
app.use((req,res,next)=>{
  console.log('有人進來了3')
  //next()
})
app.get('/',function(req,res){
    res.send('<div>Hello</div>')
})
```
> 先後順序問題
```javascript
// 如果get先執行,則就中斷了
app.get('/',function(req,res){
    res.send('<div>Hello</div>')
})
app.use((req,res,next)=>{
  console.log('有人進來了')
  next()
})
```
#### 404路由設定
```javascript
// 如果使用者輸入錯誤網址沒有某個頁面時
// 直接進入自訂404
app.use(function(req,res,next){
    res.status(404).send('Sorry')
})
```
```javascript
// 再出現在錯誤頁面及出現錯誤的程式碼時可觸發
app.use(function(req,res,next){
    console.log('AAA')
    kk();
    next()
})
app.use(function(err,req,res,next){
    console.error(err.stack)
    res.status(500).send('出現問題囉')
})
```
#### 中介使用種類
> 可把use()設定獨立個function()
```javascript
var login=function(req,res,next){
    console.log('Login')
    next()
}
app.use(login)
```
> 可把use()設定獨立個function()定放置某個router監聽
```javascript
var login=function(req,res,next){
    var _url=req.url;
    if(_url == '/'){
        console.log('Login')
        next()
    }else{
        res.send('Login Error')
    }
}

app.get('/',login, function (req, res) {
    res.send('<div>Hello</div>')
})
```
### static-載入靜態檔案

```javascript
// 增加靜態檔案的路徑
// 指定public資料夾
app.use(express.static('public'));

app.get('/', function (req, res) {
    res.send('<div>Hello <img src="/images/logo.png" alt=""/></div>')
})
```
## EJS - template 樣板語言
> 先安裝EJS,並引用
```javascript
npm i ejs-locals --save
var engine=require('ejs-locals');
// 使用ejs
app.engine('ejs',engine);
// 指定資料夾,所有ejs檔案都會在這執行
app.set('views','./views');
// 指定什麼engine去跑
app.set('view engine','ejs');
```
> 開始用EJS
```javascript
// 在views資料夾建立index.ejs
app.get('/',(req,res)=>{
    // 會去views 找index.ejs
    res.render('index');
})
```
#### EJS - 參數導入
> 初入參數載入
```html
<ul>
    <li><%= title %></li>
    <li><%= boss %></li>
</ul>
```
```javascript
app.get('/',(req,res)=>{
    res.render('index',{
        'title':'六角學院',
        'boss':'liao'
    });
})
```
> 載入內容種類
```html
<ul>
    <% if(show) { %>
        <span>IF show</span>
    <% } else { %>
        <span>IF false</span>
    <% } %>
    <li><%= title %></li><!-- =會把傳入內容轉成字符串 -->
    <li><%- boss %></li><!-- -會把傳入內容轉成網頁格式 -->
</ul>
```
```javascript
app.get('/',(req,res)=>{
    res.render('index',{
        'show':true,
        'title':'六角學院',
        'boss':'<h1>liao</h1>'
    });
})
```
> 載入陣列
```html
<% for(var i=0;course.length > i ;i++) { %>
    <li><%- course[i] %></li>
<% } %>
```
```javascript
app.get('/',(req,res)=>{
    res.render('index',{
        'show':true,
        'title':'六角學院',
        'boss':'<h1>liao</h1>',
        'course':['html','js','bs']
    });
})
```
### EJS - 設定layout
> 建立layout.ejs檔案
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <%- body %>
</body>
</html>
```
> 在其他ejs裡面放入
```html
<% layout('layout') %>
    <div>Hello User</div>
```
## Express generator產生器
```javascript
npm i express-generator -g
express -ejs project
cd project
npm start
```
### connect-flash
> flash是一个暂存器，而且暂存器里面的值使用过一次即被清空，这样的特性很方面用来做网站的提示信息。
```javascript
npm i connect-flash --save
var flash=require('connect-flas');
app.use(flash())
```
### express-session
> express-session,可以把一些機密資訊記錄在session,然後利用這樣的方式去做溝通
```javascript
var session=require('express-session')
// express-session加密
app.use(session({
    secret:'mysupersecret',
    resave:true,
    saveUninialized:true
}))
```
> 可以在會員登入頁面使用,再登入後使用`req.session.uid=user.uid;`,之後就可以在登入後的頁面使用`req.session.uid`
### express-validator
> express驗證模組
```javascript
// 放入要check表單的name
req.checkBody('content',"內容不能為空").notEmpty();
var errors=req.validationErrors();
console.log(errors)
```
### body-parser 取得表單資料
```javascript
// 增加 body 解析
// 獲取前端傳送到後端的資料
// 解析 JSON 格式
app.use(bodyParser.json())
// 解析 表單 模式
app.use(bodyParser.urlencoded(
    {extended:false}
))

app.post('/searchList',(req,res,next)=>{
    console.log(req.body)
})
```
## 使用 Postman
### 傳統表單
```javascript
// http://localhost:3000/searchList
// 選擇 POST
// Body
// x-www-form-urlencoded
```
### AJAX 表單
```javascript
// http://localhost:3000/searchList
// 選擇 POST
// Body
// raw
// JSON
```
## Router進階管理
```javascript
// 在 router 資料夾
// user.js
var express=require('express');
var router=express.Router();

router.get('/edit-profile',function(req,res){
    res.send('profile')
})

router.get('/photo',function(req,res){
    res.send('photo');
})
module.exports=router;

// app.js
var user=require('./routes/user');
app.use('/user',user)
```