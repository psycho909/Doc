# Firebase
## 第一步
> `ref()`可以把你帶到數據庫根目錄<br/>
> `child('object')`是指向下一位置的子對象<br/>
> 之後就可以存儲我們所需的任何值<br/>
> 使用`on()`調用的是數據庫參數，這個參數指向的是對象的位置<br/>>
> `snapshot`這個參數基本可以稱做`"數據快照"`<br/>
> `val()`有點像萬能工具，`val()`是獲取整個對象
```javascript
//get element
const preObject=document.getElementById('object');
//create reference
const dbRefObject=firebase.database().ref().child('object');
//sync object changes
dbRefObject.on('value',function(snapshot){
  console.log(snapshot.val())
})
```
## 上傳一個文件
> `put`命令會把文件上傳到Firebase Storage裡面<br>
> 但想要監聽文件何時被上傳，`put`命令會返回上傳任務<br>
> 使用這個任務，我們可以獲取任何狀態被改變的通知<br>
> 狀態改變一共有三種類型，這些狀態改變都由函數來表達<br>
>1. 第一個函數會通知上傳進度，每有文件上傳，就會收到一個上傳進度快照的通知，可以通過`sanp`裡面的兩個參數進行計算
>1. 第二個函數會通知一切可能發生的錯誤
>1. 第三個函數是完成函數，通知文件何時上傳結束<br>
> `notice`可以控制台會出錯`403`，那是存儲器默認的安全規則造成的，可從firebase控制台，改變，Storage -> RULE -> `if auth != null`改成`if true`
```javascript
//Get element
var uploader=document.getElementById('uploader');
var fileButton=document.getElementById('fileButton');
//Listen for file select
fileBtton.addEventListener('change',function(e){
  //get file
  var file=e.target.file[0];
  //create a storage ref
  //firebase.storage().ref('folder_name/file_name')
  var storageRef=firebase.storage().ref('cute_dogs/'+file.name);
  //upload file
  var task=storageRef.put(file);
  // update progress bar
  // 狀態改變一共有三種類型，這些狀態改變都由函數來表達
  taske.on('state_changed',
    function progress(snapshot){
      var percentage=(snapshot.bytesTransferred / sanpshot.totalBytes)*100;
      uploader.value=percentage;
    },
    function error(err){
    },
    function complete(){
    }
  );
})
```
## Firebase Hosting
>1. 先通過`npm`安裝`npm install -g firebase-tools`<br>
>1. 初始化`firebase init`<br>
>>1. 出現選項，選擇`Hosting site`託管網站<br>
>
> 未完
## Firebase Auth
> 基本使用方法
```javascript
const auth=firebase.auth();
//使用於已經有用戶，可返回promise
auth.signInWithEmailAndPassword(email,pass);
const promise=auth.signInWithEmailAndPassword(email,pass);
promise.catch(e=>console.log(e.message))
//假設尚未註冊時，可用於創建帳戶，可返回promise
auth.createUserWithEmailAndPassword(email,pass);
const promise=auth.createUserWithEmailAndPassword(email,pass);
promise.catch(e=>console.log(e.message))
//可用於監控帳戶登入&登出
auth.onAuthStateChanged(function(firebaseUser){
  if(firebaseUser){
    console.log(firebaseUser)
  }else{
    console.log("not logged in")
  }
})
//登出
firebase.auth().signOut();
```