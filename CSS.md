# CSS注意
1. [RWD Youtube](#rwd)
1. [User-Select](#user-select)
1. [Flex-grow 9999 Hack](#flex-grow-hack)
--------------------
<h2 id="rwd">RWD Youtube</h2>
--------------------
> 使用`padding-bottom`去撐高
```css
  .video-box{
    border:1px solid #000;
    position: relative;
    max-width: 800px;
  }
  .video-container{
    position: relative;
    padding-bottom: 56.25%;
    padding-top: 30px;
    height: 0;
    overflow: hidden;
  }
  .video-container iframe{
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
  }
```
```html
<div class="video-box">
  <div class="video-container">
    <iframe src="http://www.youtube.com/embed/4aQwT3n2c1Q" width="560" height="315" allowfullscreen="" frameborder="0"> </iframe>
  </div>
</div>
```
> 這段代碼做了幾件事情
>
>1. 設置position的值為relative，用來給iframe設置為absolute值；
>1. 設置padding-bottom值來計算視頻的縱橫比例。在我們的示例中，寬高的比例是16:9，表示高度是寬度的56.25%。如果寬高比是4:3，我們設置padding-bottom值為75%；
>1. 設置padding-top值為30px，主要為Chrome指定一個空間，這是指定給TouTobe的；
>1. height設置為0，因為我們通過padding-bottom來設置元素的高度。我們沒有設置width，那是因為他要配合響應式設計自動調整容器的寬度；
>1. 設置overflow的值為hidden，確保溢出的內容能夠隱藏起來。
```css
    <!--56.25% 為 (315/560)*100%  -->
  .video-container{
    position: relative;
    padding-bottom: 56.25%;
    padding-top: 30px;
    height: 0;
    overflow: hidden;
  }
```
<h2 id="user-select">阻止用戶選中</h2>
------------
```css
.select{
  -webkit-user-select:none;
  -moz-user-select:none;
  -ms-user-select:none;
}
```
```javascript
//使用javascript 多瀏覽器支持
document.body.onselectstart=function(){
  return false;
}

document.body.onmousedown=function(){
  return false
}
```
## <span id="flex-grow-hack">Flex-gorw 9999 Hack</span>
> 可讓 item-a & item-b 自適應欄位
```css
.container{
  display:flex;
  flex-direction:row;
  flex-wrap:wrap;
}
.item-a{
  flex-grow:1;
}
.item-b{
  flex-grow:9999;
  flex-basis:20em;
}
```
```html
<div class="container">
  <div class="item-a"></div>
  <div class="item-b"></div>
</div>
```