# **將資料寫入 Google Sheets**

## Step1:建立資料表

於第一列填入表格的表頭

```js
name,phone,need
```

## Step2:進入 Google Sheets 指令碼編輯器

工具 -> 指令碼編輯器

```js
function doGet(e) {
  //取得參數
  var params = e.parameter; 
  var name = params.name;
  var phone = params.phone;
  var need = params.need;
 
  //sheet資訊
  var SpreadSheet = SpreadsheetApp.openById("請輸入自己的sheet id");
  var Sheet = SpreadSheet.getSheets()[0];
  var LastRow = Sheet.getLastRow();

  //存入資訊
  Sheet.getRange(LastRow+1, 1).setValue(name);
  Sheet.getRange(LastRow+1, 2).setValue(phone);
  Sheet.getRange(LastRow+1, 3).setValue(need);
  
  //回傳資訊
  return ContentService.createTextOutput("成功");
}
```

## Step3:部屬

1. 發佈 -> 部署爲網路應用程式 -> 將具有應用程式存取權的使用者改爲 "任何人，甚至是匿名使用者" ，點選部署
2. 點選核對權限 -> 選擇自己的帳戶 -> 進階 -> 前往 -> 允許，接著就會看到以下畫面，網路應用程式網址即爲 Call API 的 URL

```js
var api="https://script.google.com/macros/s/AKfycbwOXcEgyZMtIQjXfJb71D6I2BNhYT9xBH7qTzhcFPkLRuqWb_c/exec";

var name=$("#name").val();
var phone=$("#phone").val();
var need=$("#need").val();

var params={
    name,phone,need
}

$.ajax({
    url:api,
    data:params,
    success:function(res){
        if(res === "成功"){
            
        }
    }
})
```



# 獲取Google Sheet API

## 方法1

### 1.發佈在網路上

```url
https://docs.google.com/spreadsheets/d/1ySHjH6IMN0aKImYcuVHozQ_TvS6Npt4mDxpKDtsFVFg
```

### 2.取出`ID`

```url
https://spreadsheets.google.com/feeds/list/[ID GOES HERE]/od6/public/full?alt=json
```

```url
https://spreadsheets.google.com/feeds/list/1ySHjH6IMN0aKImYcuVHozQ_TvS6Npt4mDxpKDtsFVFg/od6/public/full?alt=json
```

### 3.使用

```js
fetch("https://spreadsheets.google.com/feeds/list/1ySHjH6IMN0aKImYcuVHozQ_TvS6Npt4mDxpKDtsFVFg/od6/public/full?alt=json")
.then(res=>res.json())
.then(data=>{
    console.log(data.feed.entry)
})
```

## 方法2

>   直接從這網站取的API https://sheety.co/