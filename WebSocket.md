#	WebSocket

##	使用Node建立伺服器

### 安裝套件

1. `express` 透過express來建立本地server連線
2. `ws` WebSocket server

```node
npm install express ws -save
```

###	建立基本WebSocket連線

####	創一個檔案 `server.js`

```javascript
const express = require("express"); // 透過express來建立本地server連線
const WebSocket = require("ws").Server; // 載入ws檔案
const app = express(); // 建立express伺服器
const PORT = 3000;

const wss = new WebSocket({ port: 8000 }); // 設定WebSocket Port為3000

app.listen(PORT, () => console.log(`Listening on ${PORT}`));

wss.on("connection", (ws, req) => { // 當有新的使用者連線時執行
	ws.send(" Hello !"); // 伺服器端傳送Hello

	ws.on("close", () => {
		console.log("Close connected");
	});
});
```

####	執行連線

```node
nodemon server.js
```

####	WebSocket連線為

1. `ws` HTTP
2. `wss` HTTPS

```js
ws://localhost:8000
```

###	與Server 串聯

建立HTML載入client.js

```js
let ws = new WebSocket("ws://localhost:8000");

ws.onopen = () => {
	console.log("open connection");
};

ws.onclose = () => {
	console.log("close connection");
};

//接收 Server 發送的訊息
ws.onmessage = (event) => {
	console.log(event.data);
};

$("#btn").on("click", function () {
    var val = $("#text").val();
    // 發送訊息給 Server
    ws.send(val);
});
```

##	進階多人串聯

修改`server.js`

```js
const express = require("express"); // 透過express來建立本地server連線
const WebSocket = require("ws").Server; // 載入ws檔案
const app = express(); // 建立express伺服器
const url = require("url");
const PORT = 3000;

const wss = new WebSocket({ port: 8000 }); // 設定WebSocket Port為3000

var Index = ["0"]; // 宣告陣列用來存放目前線上數量
var wsArray = {}; // 宣告物件，用來宣告為ws物件，接收伺服器端傳送的訊息
wss.on("connection", (ws, req) => {
	console.log("Client connected");
	const location = url.parse(req.url);
	const name = location.path.substring(1);
	ws.send(name + " Hello !");

	for (var i = 0; i <= Index.length; i++) {
		if (!Index[i]) {
			Index[i] = i; // 儲存目前總共有幾個連線
			ws.id = i; // 儲存id到ws物件內，讓後續發送訊息時可以用到
			ws.name = name; // 儲存名子到ws物件內
			wsArray[ws.id] = ws; // 宣告陣列內容為ws物件
			break;
		}
	}

	ws.on("message", (mes) => {
		// 當WebSocket收到新訊息時執行，mes為使用者端傳來的訊息
		for (var i = 1; i <= Index.length - 1; i++) {
			// index.length目前連線人數
			if (i != ws.id) {
				// 當id是自己時不執行
				wsArray[i].send(ws.name + ":" + mes); // 伺服器端將mes傳給wsArray[]裡的人
			}
		}
	});

	ws.on("close", () => {
		console.log("Close connected");
	});
});


app.listen(PORT, () => console.log(`Listening on ${PORT}`));

```

