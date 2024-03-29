#CSS 
## 計算 rem

### JS方法1

```js
const oHtml=document.getElementsByTagName('html')[0];
const aWidth=oHtml.clientWidth;

// 320px的螢幕基準像素 12px
oHtml.style.fontSize=12*(aWidth/320)+'px';
```

* 這樣iphone8（375px）下html的font-size 就是14.0625px，iphone8p下font-size就是15.525px。
* 如果在iphone8（375px）下設置元素font-size為 1.7066rem， 效果跟設置其font-size為 24px 是一樣的(24 / 14.0625 = 1.7066)。
* 使用JS來獲取屏幕寬度的好處在於可以100%適配所有的機型寬度，因為其元素的基準尺寸是直接算出來的。

### JS方法2

```js
var oHTML=document.querySelector("html");
// 當前螢幕寬度
var aWidth=oHTML.clientWidth;

// 設計稿: 375px
// 默認大小: 16px

oHTML.style.fontSize=(aWidth/375)*16+"px";
```



### 媒體查詢

```css
@media screen and (min-width: 375px){
    html {
        font-size: 14.0625px;   
    }
}
@media screen and (min-width: 360px){
    html {
        font-size: 13.5px;
    }
}
@media screen and (min-width: 320px){
    html {
        font-size: 12px;
    }
}
html {
    font-size: 16px;
}
```

### 用 SASS 和 LESS 方法去更好的時候用 REM

- 我們先設置一個 font-size 的方法
- 通常網頁的 <body> 都會預設文字大小為 16px

```scss
// css 
html {
  // (10/16)*100%=62.5
  font-size: 62.5%; /* 基礎的 font-size: 10px */
}

// sass
@mixin font-size($sizeValue: 1.6) {
  font-size: ($sizeValue * 10) + px;
  font-size: $sizeValue + rem;
}
```

- 具體的用法

```css
p{
    @include font-size(13);
}
p{
    font-size:130px;
    font-size:13rem;
}
```

