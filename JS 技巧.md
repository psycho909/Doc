## 數組所有數據是否滿足某條件

```js
// 该源码来自于 https://30secondsofcode.org
const all = (arr, fn = Boolean) => arr.every(fn)
```

```js
var MIN_SALES = 100000 // 100000 分钱

// 抽取
var disciples = [
    { name: 'xiaoer', sales: 100000 },
    { name: 'xiaosi', sales: 50000 },
    { name: 'menty', sales: 150000 },
]

var canAward=disciples.every((item,idnex)=> item > MIN_SALES) // false
```

## 根據條件將數組分成兩個集合

```js
// 该源码来自于 https://30secondsofcode.org
const bifurcateBy = (arr, fn) =>
  arr.reduce((acc, val, i) => (acc[fn(val, i) ? 0 : 1].push(val), acc), [[], []])
```

```js
var CUT_OFF_SCORES = 60
var students = [
    { name: 'xiaoer', score: 80 },
    { name: 'xiaosi', score: 90 },
    { name: 'menty', score: 50 },
]
var group=students.reduce((student,val,i)=> (student[val.score > CUT_OFF_SCORES ? 0 : 1].push(val),student) ,[[],[]])
```

## 對數組項目進行統計

```js
// 该源码来自于 https://30secondsofcode.org
const countBy = (arr, fn) =>
  arr.map(typeof fn === 'function' ? fn : val => val[fn]).reduce((acc, val) => {
    acc[val] = (acc[val] || 0) + 1;
    return acc;
  }, {});
```

```js
var students = [
    { name: 'xiaoer', score: 80 },
    { name: 'xiaosi', score: 90 },
    { name: 'menty', score: 50 },
]
var scoreStat = students.reduce((acc,val)=>{
    acc[val.score]=(acc[val.score] || 0)+1;
    return acc
},{})

console.log(scoreStat)

var costStat = users.reduce((acc,val)=>{
    acc[val.cost > 10000 ? 'height':(val.cost > 5000 ? 'mid':'low')]=(acc[val.cost > 10000 ? 'height':(val.cost > 5000 ? 'mid':'low')] || 0)+1;
    return acc
},{})

console.log(costStat)
// {height: 1, mid: 1, low: 1}
```

## 兩個數組中的差集

```js
const differenceBy = (a, b, fn) => {
  const s = new Set(b.map(fn));
  return a.filter(el => !s.has(fn(el)));
};
```

```js
var superHeroCompany = [
    { name: 'xiaoer', job: '程序员' },
    { name: 'xiaosi', job: '图书管理员', },
    { name: 'menty', job: '会计' },
]

var happyCompany = [
    { name: 'xiaofu', job: '程序员' },
    { name: 'panghu', job: '会计' },
]

var s = new Set(happyCompany.map(v=>v.job))

var diffUsers = superHeroCompany.filter(el=>!s.has(el.job))
```

## 根據對象屬性對對象數組進行分組

```js
const groupBy = (arr, fn) =>
  arr.map(typeof fn === 'function' ? fn : val => val[fn]).reduce((acc, val, i) => {
    acc[val] = (acc[val] || []).concat(arr[i]);
    return acc;
  }, {});
```

```js
// 原始数据
var items = [
    { name: 'Apple iPhone X', category: '手机数码' },
    { name: '索尼 NW-A55 音乐播放器', category: '手机数码' },
    { name: '舒克 海洋之风牙膏', category: '日常用品' },
    { name: '洁丽雅 纯棉强吸水毛巾', category: '日常用品' },
]

// 分类后的商品数据
var categoryItems = items.reduce((acc,val,i)=>{
	acc[val.category]=(acc[val.category] || []).concat(items[i])
	return acc
},{})

// 分类种类
var categoryKeys = Object.keys(categoryItems)
```

## 取出對象數組中唯一的數據集

```js
// 该源码来自于 https://30secondsofcode.org
const filterNonUniqueBy = (arr, fn) =>
  arr.filter((v, i) => arr.every((x, j) => (i === j) === fn(v, x, i, j)));
```

```js
// 查询到参加 2019厦门马拉松的数据
var join2019 = [
    { id: 1, name: 'xiaoer', join: ['2019厦门马拉松', '2018厦门马拉松'] },
    { id: 2, name: 'xiaosi', join: ['2019厦门马拉松'] },
]

// 查询到参加 2018年马拉松的数据
var join2018 = [
    { id: 1, name: 'xiaoer', join: ['2019厦门马拉松', '2018厦门马拉松'] },
    { id: 3, name: 'menty', join: ['2018厦门马拉松'] },
]

// 合并数据
var users = [...join2019, ...join2018]

// 取出只參加過一次的名單
var joinOnce = users.filter((v,i)=> users.every((x,j)=>(i===j) === (v.id === x.id)))

console.log(joinOnce)
```

## 前端 input 輸入框可能被攻擊的幾種方式及防範

```js
var escapeHTML=str=>str.replace(/[&<>'"]/g,tag=>({
    	'&': '&amp;',
        '<': '&lt;',
        '>': '&gt;',
        "'": '&#39;',
        '"': '&quot;'
    }[tag]||tag)
```

