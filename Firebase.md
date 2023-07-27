#js 
# Firebase
## 基本使用
### 寫入資料庫
```javascript
// ref() 尋找資料庫路徑
// set() 新增資料
firebase.database().ref().set('hi')

// firebase 全部都是物件格式，不能陣列內容
var data={
	people1:{
		name:'chen',
		age:20
	},
	people2:{
		name:'ccc',
		age:1231
	}
}
firebase.database().ref().set(data);
// 找people1並改name
firebase.database().ref('people1/name').set('bb')
```
### 讀取資料庫
```javascript
// firebase.database().ref('myName').set('mark')
var myName=firebase.database().ref('myName');
// once讀取一次資料庫的資料、on隨時監聽
// snapshot 快照
myName.once('value',(snapshot)=>{
    console.log(snapshot.val())
})
myName.on('value',(snapshot)=>{
    console.log(snapshot.val())
})
```
### firebase屬於非同步
### firebase push()新增資料
```javascript
// push() 會自動產生 key值
var todos=firebase.database().ref('todos');
todos.push({content:'今天要幹嘛'})
```
### child、remove() 移除資料
```javascript
// child() 可以找出當下子路徑
var todos=firebase.database().ref().child('todos');
todos.child('-L8Rgc58OEYdPm10l1iz').remove()
```
### foreach 、 orderByChild資料排序
```javascript
// 路就 >> 排序('屬性') >> 讀取 >> forEach 依序撈出資料
// 使用orderByChild()選擇要排序的資料
// 需搭配foreach
var peopleRef=firebase.database().ref('people')
peopleRef.orderByChild('weight').once('value',(snapshot)=>{
	snapshot.forEach(function(item){
		console.log(item.val())
	})
})
```
### orderByChild()排序規則
1. 指定key 的 value為null的子項目排在最前
1. 指定key 的 value為false時，如果多個子項目的value為false時，依照key的字典順序排序
1. 指定key 的 value為true的子項目排在其後，如果多個子項目的value為true時，依照key的字典順序排序
1. 指定key 的 value為數字，則依數字大小
1. 字串
1. object
```javascript
// null >> false >> true >> number >> string >> object
```
### startAt() endAt() equalTo() 搜尋區間規則
```javascript
// startAt() 多少以上
// endAt() 多少以下
// equalTo() 等於
var peopleRef = firebase.database().ref('people')
peopleRef.orderByChild('weight').startAt(3500).endAt(4500).once('value', (snapshot) => {
	snapshot.forEach(function (item) {
		// 查看key
		console.log(item.key)
		console.log(item.val())
	})
})
```
### limit() 限制筆數
```javascript
// limitToFirst() 從最前面開始
// limitToLast() 從最後面開始
```
### reverse() 資料翻轉調整
```javascript

```

## Cloud Firestore

### 調用順序:

```
建立Ref=>調用方法(get,add)=>整合為payload=>commit
```

### 建立Reference:

```js
const Ref = db.collection("Articles")
```

### 讀取資料

```js
const Ref = db.collection("Articles")
const result = await Ref.get()
result.forEach(art=>{
    payload.push({id:art.id,...art.data()})
})
```

### 建立資料

有`add`和`set`

add會自動生成id

set會需要建立一個doc，並給他一個id，並去尋找，如果沒有相同id，就會去生成一個。(可用於自己建立id的內容去使用)

```js
const Ref = db.collection("Articles")
let addRef = Ref.add({
  name: 'Tokyo',
  country: 'Japan'
})

commit("addArticle",{id:addRef.id,...payload})
```

### 更新資料

update會把你設定的資料去做比對，如果有差異的資料才會重新去複寫

set會把你原本的資料去刪除，然後在把全部的資料覆蓋上去(大量資料更新時使用)

```js
const Ref = db.collection("Articles").doc("<id>")
const result=await Ref.update(<new Article>) // 與下方的set二選一
const result=await Ref.set(<new Article>)
```

### 刪除資料

```js
const docRef=db.collection("Articles").doc("<id>");
await docRef.delete()
```

