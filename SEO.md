## Header 添加 讓網頁搜尋的到

```html
<meta name="description" content="全國電子_學長幫你 ∣ 超值獨家優惠UltraBook就找全國電子" />
<meta name="keywords" CONTENT="全國電子_學長幫你,ultrabook,asus超值送,acer超值送,office365超值送">
	
<meta property="og:url"  content="http://www.elifemall.com.tw/ultrabooks/" />
<meta property="og:type" content="website" />
<meta property="og:title" content="全國電子_學長幫你" />
<meta property="og:description" content="全國電子_學長幫你 ∣ 超值獨家優惠UltraBook就找全國電子" />
<!-- 1200 x 630  || 600 x 315 -->
<meta property="og:image" content="http://www.elifemall.com.tw/ultrabooks/images/fb.jpg" />

```

## UTM

> 來源 > 成效

```txt
http://events.tkec.com.tw/events_net/flashday/detail.aspx?
utm_source=facebook&
utm_medium=3post&
utm_campaign=201809flashday_pre0816

utm_source= BLOG
utm_medium=textLink
utm_campaign=booksSale
```

### utm_source

> 來源 (FB、Google、AD、BLOG)

### utm_medium

> 媒介 - 透過哪裡來的

1. CPC - 每次點擊成本
2. CPM - 千次觸擊成本
3. rightBanner - 右側廣告

### utm_campaign

> 活動 (賣書活動、義賣活動...)

### 範例1

```html
https://www.books.com.tw/products/0010795418?

// 從facebook來的
utm_source=facebook &
// 來自socialmedia
utm_medium=socialmedia &
// 來自活動
utm_campaign=fb-bookstw &
// 
utm_content=book180825 &
utm_term=a
```

### 範例2

2020 販售 XX 線上課程活動

1.我的個人部落格

```html
https://codepen.io/onionegg/pen/jvqQXr/?utm_source=BLOG&utm_medium=rightBanner&utm_campaign=2020OnlineCourse
```

2.我的個人臉書

```html
https://codepen.io/onionegg/pen/jvqQXr/?utm_source=FB&utm_medium=myself&utm_campaign=2020OnlineCourse
```

3.下FB廣告

```text
https://codepen.io/onionegg/pen/jvqQXr/?utm_source=FB&utm_medium=AD&utm_campaign=2020OnlineCourse
```

4.下 Google 廣告

```html
https://codepen.io/onionegg/pen/jvqQXr/?utm_source=Google&utm_medium=keyword&utm_campaign=2020OnlineCourse
```

## 轉換與行為

### 行為

> 先定義行為(網站動作)，在依照行為去記錄事件

1. 載入頁面
2. 點擊
3. 滑鼠滾輪

### 漏斗

1. 優化各流程細節
2. 可以將進入結帳頁面但沒購買成功的流量蒐集起來，進行再行銷

### 轉換

1. 希望使用者在網頁上最終完成的行為
2. 以此訂定你的漏斗流程

#### 【範例】

1. 電商1:完成一次的購買行為
2. 電商2:完成加入購物車行為
3. AWS:完成一個註冊流程
4. APP:在APP上購買產品

### 結論

> 先從使用行為，產生出漏斗，進行優化漏斗，再從漏斗看最終完成達到哪個轉換

## 標準事件

| 網站動作     | 簡介                                                         | 標準事件程式碼                                            |
| ------------ | ------------------------------------------------------------ | --------------------------------------------------------- |
| 查看內容     | 追蹤主要網頁瀏覽次數（例如產品頁面、連結頁面或文章）         | fbq('track', 'ViewContent');                              |
| Search       | 追蹤網站上的搜尋次數（例如產品搜尋）                         | fbq('track', 'Search');                                   |
| 加到購物車   | 追蹤項目加到購物車的時機（例如點擊「加到購物車」按鈕／進入「加到購物車」頁面） | fbq('track', 'AddToCart');                                |
| 加到願望清單 | 追蹤項目加到願望清單的時機（例如點擊「加到願望清單」按鈕／進入「加到願望清單」頁面） | fbq('track', 'AddToWishlist');                            |
| 開始結帳     | 追蹤用戶進入結帳流程的時機（例如點擊「結帳」按鈕／進入「結帳」頁面） | fbq('track', 'InitiateCheckout');                         |
| 新增付款資料 | 追蹤用戶在結帳流程中新增付款資訊的時機（例如點擊帳單資訊／進入帳單資訊頁面） | fbq('track', 'AddPaymentInfo');                           |
| 購買         | 追蹤購買次數或完成結帳流程次數（例如進入「感謝您」或確認頁面） | fbq('track', 'Purchase', {value:'0.00', currency:'USD'}); |
| Lead         | 追蹤用戶對您優惠透露出感興趣的時機（例如提交表單、註冊試用版、進入定價頁面） | fbq('track', 'Lead');                                     |
| 完成註冊     | 追蹤完成註冊表單的時機（例如完成訂閱、註冊服務）             | fbq('track', 'CompleteRegistration');                     |