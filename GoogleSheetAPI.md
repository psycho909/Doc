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