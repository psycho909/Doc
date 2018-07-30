# Mock使用

## 第一步安裝

```js
npm i --save mockjs
// 建立 mock/mock.js
import Mock from 'mockjs'

// main.js
import './mock/mock'
```

## Mock使用

```js
Mock.mock( rurl, rtype, template|function( options ){ })
```

## rtype獲取(get|post|delete|put)

```js
Mock.mock('/api/data', 'post', function(options) {
    return options
})
$.ajax({
    url: 'hello.json',
    dataType: 'json',
    data: {
        foo: 1,
        bar: 2,
        faz: 3
    }
}).done((data)=>{
    console.log(data)
})
===========================================
{
    url:'/api/data',
    type:'post',
    body: data:{
		foo:1,
         bar:2,
         faz:3
    }
}
```



### Template產生

```js


var template = {
    'title': 'Syntax Demo',
	
    // 隨機產生 1-10次
    'string1|1-10': '★',
    // 產生3次
    'string2|3': 'value',
	
    // 屬性值自動加1
    'number1|+1': 100,
    // 隨機產生 1~100之間
    'number2|1-100': 100,
    // 隨機產生 浮點數
    'number3|1-100.1-10': 1,
    'number4|123.1-10': 1,
    'number5|123.3': 1,
    'number6|123.10': 1.123,
	// 随机生成一个布尔值，值为 true 的概率是 1/2，值为 false 的概率同样是 1/2。
    'boolean1|1': true,
    // 随机生成一个布尔值，值为 value 的概率是 min / (min + max)，值为 !value 的概率是 max 
    'boolean2|1-2': true,

    // 从属性值 object 中随机选取 min 到 max 个属性。
    'object1|2-4': {
        '110000': '北京市',
        '120000': '天津市',
        '130000': '河北省',
        '140000': '山西省'
    },
    // 从属性值 object 中随机选取 count 个属性。
    'object2|2': {
        '310000': '上海市',
        '320000': '江苏省',
        '330000': '浙江省',
        '340000': '安徽省'
    },
	// 隨機產生 1-4 次
    'array1|1-4': ['AMD', 'CMD', 'KMD', 'UMD'],
    // 隨機產生 1-10 次
    'array2|1-10': ['Mock.js'],
    // 通过重复属性值 array 生成一个新数组，重复次数为 count。
    'array3|3': ['Mock.js'],

    'function': function() {
        return this.title
    }
}
```



## Mock實例

```js
// Mock.Random 是一個工具類，用於生成各種隨機數據
var Random=Mock.Random;

let arr=[];
for(let i=0;i<10;i++){
    let newArticleObject={
        name:Random.name(),
        title:Random.csentence(5,10),
        thumbnail:Random.dataImage('300x250','mock img'),
        date:Random.date(),
        id:i
    }
    arr.push(newArticleObject)
}

let list=(options)=>{
    // options.type 可獲取 get/post/put/delete...
    let rtype=options.type.toLowerCase()
    switch(rtype){
        case "get":
            break;
        case "post":
            // options.body 獲取 post的數據
            let id=parseInt(JSON.parse(options.body).id)

            arr=arr.filter((v)=>{
                return v.id != id
            })
            break;
        default:
    }
    return {
        data:arr
    }
}

Mock.mock('/api/data',/get|post/i,list)

=============================================

this.axios.get('/api/data')
.then((res)=>{
    console.log(res.data)
})
```

