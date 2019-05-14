# Node.js 部署至 Heroku
## 部署至 Heroku
1. 註冊帳號
1. 下載`Heroku CLI`
1. 在使用`heroku login`輸入在`heroku`建立的帳號密碼
1. 在要上傳上去的資料夾先建立`git init`並`commit`
1. `commit`完成後使用`heroku creat`後自動建立了`git remote`
1.  在使用`git push herku master`就push成功後可以使用
1. 在使用`heroku open`，會打開所建立的網站
> 在`package.json`注意事項
```javascript
// 可以指定heroku使用node的版本去跑
"engines": {
  "node": "8.9.4"
},
// 可以讓heroku使用node app.js去開啟
"scripts": {
  "start": "node app.js"
},
```
> 在使用PORT注意事項
```javascript
// 在上傳到其他主機時監聽的PORT
var http=require('http);
http.createServer((req,res)=>{
// somthing.....
})
// 要添加process.env.PORT，他會對應上傳上去主機環境變數對應的PORT
.listen(process.env.PROT || 8080)
```
## 使用Node所使用的NPM
```javascript
// npm 安裝時去使用--save || --save-dev
// heroku 才會認得並去安裝
"dependencies": {
  "nodemon": "^1.15.1"
}
```
## Heroku 使用dotenv
> 使用`dotenv`把重要資料隱藏起來並使用其他方式載入
1. 在`heroku`主機上面
1. `setting` -> `Config Variables`管理變數資料
1. 這樣子就可以透過`process.env`去取得變數

# 部屬Heroku步驟

## step1: 初始化 git init

>   初始化

```js
> git init
```

## step2: 使用.gitignore

>   創建`.gitignore`忽略`node_modules`

```js
node_modules > .gitignore
```

## step3: git commit

>   `git commit`

```js
> git add -A
> git commit -m 'initial commit'
```

## step4: 安裝Heroku並Login

>   安裝Heroku並Login

```js
> heroku login
```

## step5: 創建 Heroku app

```js
> heroku create nodejs-demo
```

## step6: Make sure `package.json`have`start`

```js
...
"scripts": {
    "dev": "nodemon index.js", <-- for local development purpose
    "start": "node .", <-- Heroku will use to start the application
    "test": "echo \"Error: no test specified\" && exit 1"
  },
...
```

## step7: Deploy the code

>   部屬code到Heorku，如果沒有出錯，會產生一個URL

```js
> git push heroku master
```

## step8: Check deploument logs

```js
> heroku logs --tail
```

