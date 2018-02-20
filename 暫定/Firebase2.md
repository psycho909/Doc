# Firebase
## 創建一個專案
>
> 選取連結的應用程式，可獲取需使用的程式碼片段
>
> 複製下方的程式碼片段，然後貼到 HTML 底端 (其他 script 標記前)。
```html
<script src="https://www.gstatic.com/firebasejs/4.1.5/firebase.js"></script>
<script>
  // Initialize Firebase
  var config = {
    apiKey: "AIzaSyDMSWZGpM49sOVnnLkS6_rCpT4cvufBAbg",
    authDomain: "demo01-26160.firebaseapp.com",
    databaseURL: "https://demo01-26160.firebaseio.com",
    projectId: "demo01-26160",
    storageBucket: "demo01-26160.appspot.com",
    messagingSenderId: "220973867831"
  };
  firebase.initializeApp(config);
</script>
```
## Use Firebase services
>1. `firebase.auth()` => 獲取各種驗證資訊
>1. `firebase.storage()` => 存儲檔案使用
>1. `firebase.database()` => 獲取資料庫
>
## `firebase.database()`
> `firebase.database().ref()`ref指的是資料節點的路徑。<br>
> `firebase.database().ref('/posts/'+postId)`與特定資料夾產生關聯
```javascript
//路徑是'users/'加上每個user的uid,set則是寫入資料的方式!
firebase.database().ref('users/'+loginUser.uid).set({
})
```
> 資料庫sturcture是使用JSON格式
```json
{
  "users": {
    "alovelace": {
      "name": "Ada Lovelace",
      "contacts": { "ghopper": true },
    },
    "ghopper": { ... },
    "eclarke": { ... }
  }
}
```
## `set()`創立寫入
> 寫入資料庫
```javascript
firebase.database().ref('user/'+userId).set({
    username:name,
    email:email,
    profile_picture : imageUrl
})
```
## `on()`or`once()`讀取
> 讀取data的change，使用`on()` or `once()`
>
> `once` 代表我只取一次資料而非持續關注，接著在`snapshot`中可以得到所有的資料
>
```javascript
//on() 每當更動時會更新
var starCountRef = firebase.database().ref('posts/' + postId + '/starCount');
starCountRef.on('value', function(snapshot) {
  updateStarCount(postElement, snapshot.val().username);
});
//once() 只顯示一次，之後的更新變動就不會跟著變動顯示
var starCountRef = firebase.database().ref('posts/' + postId + '/starCount');
starCountRef.once('value', function(snapshot) {
  updateStarCount(postElement, snapshot.val());
});
```
## `update()`更新修改
> 使用`update()`可以修改資料庫
```javascript
var postData = {
  age: 3388,
  name: 'ChenChingYang',
  programme:'Firebase'
};
var updates = {};
updates['mydata/'+'AAA']=postData;

return firebase.database().ref().update(updates)
```
## `remove()`刪除
> 使用`remove()`可以刪除資料庫內容，等同於`set(null)`
> 取得當下的驗證狀態可以使用
```javascript
firebase.database().ref('mydata/'+'FFF').child('name').remove();
firebase.database().ref('/users/' + loginUser.uid + "/name").remove();
```
## `push()`在清單中新增資料
> 可在新增一個新的節點，但是多個Post沒有專屬的id可以辨別，這時候可以使用`push()`。
>
> 一開始我先指到節點`var postRef = firebase.database().ref('/posts/' + loginUser.uid);`，接著使用`postRef.push()`，這個時候DB會回傳一個基於時間雜湊的Unique Key，接著使用set()方法創建資料
```javascript
var postRef=firebase.database().ref('/mydata/'+'AAA');
postRef.push().set({
    uid:loginUser.uid,
    title:'psuh Title'+i,
    content:'push Content'+i
}).then(function(){
    console.log('success')
}).catch(function(err){
    console.log('Error',err)
})
```
## `firebase.auth()`取得通過驗證的使用者資料
```javascript
var user = firebase.auth().currentUser;
```
>
## 加入資料權限
> 目前的資料權限是任何人都可以讀寫User和Post的資料，現在要來修改這部分
>
> Firebase針對每個節點可以設定的四種規則型態
>
|Title|說明|
|:-----:|:----:|
|`.red`|針對讀的權限，淺層規則覆蓋深層|
|`.write`|針對寫的權限，淺層規則覆蓋深層|
|`.validate`|資料驗證，不會影響子節點|
|`.indexOn`|節點的索引設定，輔助排序與過濾功能|