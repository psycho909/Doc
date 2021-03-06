## 快速入門

```js
npm i -S express
```

```js
const express=require("express");

const app=express();

app.get("/",(req,res)=>{
    res.send("Hello World")
})

app.listen(3000,()=>console.log("listen 3000"))
```

## express中的靜態服務

### 方法一:(常用)

```js
app.use("/public/",express.static("./public"))
```

### 方法二:

```js
app.use(express.static('./public'))
```

### 方法三:

```js
app.use('/static/', express.static('./public'))
```

### 方法四:

```js
app.use(express.static(__dirname+"/static"));
```



## express结合art-template模板引擎

### art-template寫法

#### 標準語法

```ejs
{{if user}}
  <h2>{{user.name}}</h2>
{{/if}}

```

#### 原始語法

```ejs
<% if (user) { %>
  <h2><%= user.name %></h2>
<% } %>
```

原始語法兼容 [EJS](http://ejs.co/)、[Underscore](http://underscorejs.org/#template)、[LoDash](https://lodash.com/docs/#template) 模板。

### 1.安裝對應的NPM

```js
npm install --save art-template
npm install --save express-art-template
```

```js
app.engine("art",require("express-art-template"))
```

### 安裝ejs

```js
app.set("views",path.join(__dirname,'views'));
app.set("view engine",'ejs')
```



## POST請求 使用`body-parser`

```js
npm i body-parser -S
```

```js
var bodyParser = require('body-parser');
var urlencodedParser = bodyParser.urlencoded({
    extended:false
})
app.get("/",(req,res)=>{
    res.render("login.art",{
        title:"這事登入頁面"
    })
})

app.post('/',urlencodedParser,(req,res)=>{
    // 接收數據
    console.log(req.body)
    // 處理數據後處理next的問題
    res.redirect("/")
})
```

## 中間件

*   執行任何代碼
*   對請求和響應對象進行更改。
*   結束請求響應週期。
*   調用堆棧中的下一個中間件。

### 中間件的數據是可以傳輸的

```js
app.use((req,res,next)=>{
    console.log("路過---------")
    next()
})
app.get("/",(req,res,next)=>{
    req.title="這事登入頁面";
    next();
},(req,res,next)=>{
    console.log(req.title)
    res.render("login.art",{
        title:req.title
    })
})
```

應用級中間件

```js
//沒有掛載路徑的中間件，應用的每個請求都會執行該中間件
app.use((req,res,next)=>{
    console.log("Time",Date.now())
    next()
})

//掛載至 /user/:id 的中間件，任何指向 /user/:id 的請求都會執行它
app.use('/user/:id',function(req,res,next){
    console.log("Request Type:",req.method);
    next();
})

//路由和句柄函數（中間件系統），處理指向/user/:id 的GET請求
app.get('/user/:id',function(req,res,next){
    res.send("USER");
})



```



## express 錯誤處理

```js
app.use(logErrors)
app.use(clientErrorHandler)
app.use(errorHandler)

function logErrors (err, req, res, next) {
  console.error(err.stack)
  next(err)
}
function clientErrorHandler (err, req, res, next) {
  if (req.xhr) {
    res.status(500).send({ error: 'Something failed!' })
  } else {
    next(err)
  }
}
function errorHandler (err, req, res, next) {
  res.status(500)
  res.render('error', { error: err })
}
```



## cookie介紹

* 能夠儲存資料在瀏覽器上的小型資料庫(4k)
* 能夠在client(瀏覽器)、server(後端成是)進行讀取、寫入
* 是由key/value方式組成，並由分號跟空格來隔開
* 可以設定失效時間，讓cookie在指定時間內消失
### cookie欄位介紹
* Name: key
* Value: value
* Domain: 可以取得開Cookie的網域
* expires: 限制Cookie有效期間
* path: 設定可以存取該cookie的路徑
* secure: 設定Cookie是否要 https 網址才可進行傳送
### Cookie 瀏覽器端寫法
```javascript
// 寫入 cookie
document.cookie="myName=tom";

// insert cookie,destory 10s
document.cookie="myName=tom;max-age=10;path=/"

```
### Node使用Cookie
```javascript
// app.js
var cookieParser = require('cookie-parser');
// index.js
console.log(req.cookies)
res.cookie('name','mary',{
    // 有效時間
    maxAge:5000,
    // 不讓JS讀取
    httpOnly:true
})
```
## session
* 儲存在伺服器的暫存資料，此暫存可放在記憶體或資料庫上
* session 可在 cookie 上儲存一筆便是你是誰的 session id
### Node使用session
> 在每次進入頁面(app.js)時，就隨機產生id傳送過去
>
> 在每次登入頁面時，就隨機產生id傳送過去
>
> [要求物件](https://expressjs.com/zh-tw/4x/api.html#req) (`req`)、[回應物件](https://expressjs.com/zh-tw/4x/api.html#res) (`res`)
```javascript
// app.js
var session=require('express-session');
app.use(session({
    // 加強私密安全
    // 自己添加序號
    secret:'keyboard cat',
    // true: 每次載入時重新寫入
    resave:true,
    saveUninitialized:true,
    // 設定多久失效
    cookie:{
        maxAge:10000
    }
}))
// index.js
router.get('/', function(req, res, next) {
    res.render('index', { 
        userName: req.session.username,
        email:req.session.email 
    });
});
router.post('/',function(req,res){
    req.session.username=req.body.username;
    req.session.email=req.body.email;
    res.redirect('/');
})
```