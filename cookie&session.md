# cookie介紹
* 能夠儲存資料在瀏覽器上的小型資料庫(4k)
* 能夠在client(瀏覽器)、server(後端成是)進行讀取、寫入
* 是由key/value方式組成，並由分號跟空格來隔開
* 可以設定失效時間，讓cookie在指定時間內消失
## cookie欄位介紹
* Name: key
* Value: value
* Domain: 可以取得開Cookie的網域
* expires: 限制Cookie有效期間
* path: 設定可以存取該cookie的路徑
* secure: 設定Cookie是否要 https 網址才可進行傳送
## Cookie 瀏覽器端寫法
```javascript
// 寫入 cookie
document.cookie="myName=tom";

// insert cookie,destory 10s
document.cookie="myName=tom;max-age=10;path=/"

```
## Node使用Cookie
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
# session
* 儲存在伺服器的暫存資料，此暫存可放在記憶體或資料庫上
* session 可在 cookie 上儲存一筆便是你是誰的 session id
## Node使用session
> 在每次進入頁面(app.js)時，就隨機產生id傳送過去
>
> 在每次登入頁面時，就隨機產生id傳送過去
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

```