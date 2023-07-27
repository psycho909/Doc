#js #node
## fs

```js
const fs = require('fs');

// 寫入文件：fs.writeFile(path, fileData, cb);
fs.writeFile('./text.txt', 'hello xr!', err => {
  if (err) {
    console.log('寫入失敗', err);
  } else {
    console.log('寫入成功');
  }
});

// 讀取文件：fs.readFile(path, cb);
fs.readFile('./text.txt', (err, fileData) => {
  if (err) {
    console.log('讀取失敗', err);
  } else {
    console.log('讀取成功', fileData.toString()); // fileData 是二進制文件，非媒體文件可以用 toString 轉換一下
  }
});

```

## path

```js
const path = require('path');

let str = '/root/a/b/index.html';
console.log(path.dirname(str)); // 路徑
// /root/a/b
console.log(path.extname(str)); // 後綴名
// .html
console.log(path.basename(str)); // 文件名
// index.html

// path.resolve() 路徑解析，簡單來說就是拼湊路徑，最終返回一個絕對路徑
let pathOne = path.resolve('rooot/a/b', '../c', 'd', '..', 'e');

// 一般用來打印絕對路徑，就像下面這樣，其中 __dirname 指的就是當前目錄
let pathTwo = path.resolve(__dirname, 'build'); // 這個用法很常見，你應該在 webpack 中有見過

console.log(pathOne, pathTwo, __dirname);
// pathOne  =>  /Users/lgq/Desktop/node/rooot/a/c/e
// pathTwo  =>  /Users/lgq/Desktop/node/build
// __dirname  =>  /Users/lgq/Desktop/node
```

## url

```js
const url = require('url');

let site = 'http://www.xr.com/a/b/index.html?a=1&b=2';
let { pathname, query } = url.parse(site, true); // url.parse() 解析網址，true 的意思是把參數解析成對象

console.log(pathname, query);
// /a/b/index.html  { a: '1', b: '2' }

```

## stream 流

```js
const fs = require('fs');

// 讀取流：fs.createReadStream();
// 寫入流：fs.createWriteStream();
let rs = fs.createReadStream('a.txt'); // 要讀取的文件
let ws = fs.createWriteStream('a2.txt'); // 輸出的文件

rs.pipe(ws); // 用 pipe 將 rs 和 ws 銜接起來，將讀取流的數據傳到輸出流（就是這麼簡單的一句話就能搞定）

rs.on('error', err => {
  console.log(err);
});
ws.on('finish', () => {
  console.log('成功');
})

```

