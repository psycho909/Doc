# FB留言板

## 第一步

1. 建立我的應用程式
2. 獲取應用程式編號
3. 進入設定->基本資料->應用程式網域設定

## 第二步

初始化放入`head`

```html
<script>
    window.fbAsyncInit = function () {
        FB.init({
            appId: "1361927494169720",
            autoLogAppEvents: true,
            xfbml: true,
            version: "v9.0",
        });
    };
</script>
<script async defer crossorigin="anonymous" src="https://connect.facebook.net/zh_TW/sdk.js"></script>
```



## 第三步

到這`https://developers.facebook.com/docs/plugins/comments/`使用留言外掛程式的程式碼產生器

`把需要產生留言版的網址放入留言外掛程式快速產生出留言板網址`

獲取程式碼放入`body`

```js
<div id="fb-root"></div>
<div class="fb-comments" data-href="https://distracted-morse-42ffd6.netlify.app/" data-width="" data-numposts="5"></div>
```

## 第四步

設定管理員審核工具，放入`head meta`

```html
<meta property="fb:app_id" content="1361927494169720" />
```



