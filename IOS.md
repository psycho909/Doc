## IOS數字過長會變成撥打電話

```html
<meta name="format-detection" content="telephone=no">
```



## IOS fullScreen模式

> viewport-fit=cover'

```html
<meta name='viewport' content='initial-scale=1, viewport-fit=cover'>
```

## IOS 左右下安全邊距

```css
.post {
    padding: 12px;
    padding-left: env(safe-area-inset-left);
    padding-right: env(safe-area-inset-right);
    padding-bottom: env(safe-area-inset-bottom);
}
```

```css
@supports(padding: max(0px)) {
    .post {
        padding-left: max(12px, env(safe-area-inset-left));
        padding-right: max(12px, env(safe-area-inset-right));
    }
}
```

## IOS height 100vh

```css
#app {
    width: 100%;
    height: 100vh;
    height: calc(var(--vh, 1vh) * 100);
    background-color: #eee;
}
```

```js
var app=document.getElementById("app");
window.addEventListener("load",function(){
    let vh = window.innerHeight * 0.01;
    document.documentElement.style.setProperty('--vh', `${vh}px`);
})
```

