## CSS變量

`.box`如果有變量設定`--color`時顏色為orange，沒有則red

```css
:root{
    --w:100px;
}
.box{
    width:var(--w);
}
```



```css
.box{
    --color:orange;
    background-color:var(--color,red);
}
```

## CSS 3D 

### Card翻轉

1. 先有3個要素 攝影機、空間、物件
2. 攝影機 `perspective:1000px`
3. 空間 `transform-style:preserve-3d`
4. 物件 `transform:rotateY(180deg)`

```css
img {
    display: block;
}
/*Custom Styles*/
.cards-container {
    max-width: 100%;
    width: 1000px;
    margin: 0 auto;
}

.card-container.camera{
    perspective: 600px;
    width: 150px;
    height: 150px;
}
.card-container.active .card.space{
    transform: rotateY(180deg);
}
.card.space{
    width: 150px;
    height: 150px;
    position: relative;
    transition: all .7s;
    transform-style: preserve-3d;
}

.front,.back{
    position: absolute;
    width: 100%;
    top: 0;
    bottom: 0;
    backface-visibility: hidden;
    display: flex;
    justify-content: center;
    align-items: center;
}
.back{
    transform: rotateY(180deg);
}
```

```html
<div class="card-container camera">
    <div class="card space">
        <div class="front">
            <img src="https://api.adorable.io/avatars/150/1" />
        </div>
        <div class="back">
            <img src="https://api.adorable.io/avatars/150/2" />
        </div>
    </div>
</div>
```

## 響應式 Iframe

### CSS

> 使用`padding-bottom`去撐高
```css
  .video-box{
    border:1px solid #000;
    position: relative;
    max-width: 800px;
  }
  .video-container{
    position: relative;
    padding-bottom: 56.25%; /* 16:9 */
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
### 使用 JS 響應 iframe

```js
// 找到所有的 iframe
var $iframe = $( "iframe" );
// 保存所有 iframe 的縱橫比
$iframe.each(function () {
  $( this ).data( "ratio", this.height / this.width )
    // 移除 width 和 height 屬性
    .removeAttr( "width" )
    .removeAttr( "height" );
});
 
// 當窗口被調整大小時，調整 iframe 的大小
$( window ).resize( function () {
  $iframe.each( function() {
    // 獲取父元素內容的寬
    var width = $( this ).parent().width();
    $( this ).width( width )
      .height( width * $( this ).data( "ratio" ) );
  });
// 調整大小以適應頁面加載上的所有iframe。
}).resize();
```



阻止用戶選中
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
Flex-gorw 9999 Hack
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
## CSS3 counter-increment 計數器
> 在父屬性使用`counter-reset:;`定义我们的计数器变量。我们必须给出任意的名字和可选的开始值。默认的开始值是 0。这个属性是包装器元素。
>
> 在子屬性的`:before`或`:after`裡面的`content:;`定義`counter()` 函数,並在在裡面定義运用 `counter-increment` 属性，我们可以递增或者递减计数器的值。该属性还有一个可选的值，用于指定递增/递减量
>
> 如有`ul`嵌套可以用`counters()`
```css
ol{
  counter-reset: section;
  list-style: none;;
}
ol li{
  position: relative;
}
ol li:before{
  content: counters(section,".") " ";
  position: relative;
  color:rgb(247, 26, 26);
  counter-increment: section; 
}
```
```html
<ol>
	<li>item</li>
	<li>item</li>
	<ol>
		<li>items</li>
		<li>items</li>
		<li>items</li>
	</ol>
</ol>
```
## img圖片去做到自適應
```html
<div class="img-wrapper">
    <img :src="food.picture" alt="" class="food-img">
</div>
```
```scss
.img-wrapper{
  position: relative;
  width: 100%;
  height: 0;
  // 高度如何撐開
  // 在定位中，使用padding-top或padding-bottom設置為100%，其實盒子高度會根據盒子高度進行計算
  padding-top:100%; 
  .food-img{
      position: absolute;
      bottom: 0;
      left: 0;
      width: 100%;
      height: 100%;
  }
}
```
## 支援手機直式&&橫式
```css
/*orientation: portrait   直式*/
@media screen and  (orientation:portrait) {
    /*
        css ...
    */
}
/*orientation: landscape  橫式*/
@media screen and  (orientation:landscape) {
    /*
        css ...
    */
}
```
```javascript
	window.addEventListener("orientationchange",onOrientationchange ,false);
	function onOrientationchange() {
		if (window.orientation === 180 || window.orientation === 0) {
			/*orientation: portrait 直式*/
			$("#orientation").hide();
		}
		if (window.orientation === 90 || window.orientation === -90 ){
			/*orientation: landscape 橫式*/
			$("#orientation").show();
		}
	}
```
## Facebook分享時注意事項
```html
    <meta property="og:title" content="Hahow 好學校"> <!-- 分享的標題  -->
    <meta property="og:description" content="Hahow 好學校課程幕資中，RWD課程熱烈招生中"> <!-- 分享的內容敘述 -->
    <meta property="og:url" content=""> <!-- 分享的網頁連結 -->
    <meta property="og:image" content="http://www.joostrap.com/images/homepage_multiple_devices.jpg"> <!--分享在FB的縮圖 -->
```

## HTML 圖片優化(響應式圖片)

### img

```html
<img class='pic' 
     srcset="https://picsum.photos/200/200?image=1063 200w,
             https://picsum.photos/300/300?image=1064 300w,
             https://picsum.photos/600/600?image=1065 600w,
              https://picsum.photos/800/400?image=1066 800w,
              https://picsum.photos/1000/500?image=1067 1000w,
              https://picsum.photos/1200/600?image=1068 1200w"
     sizes="100vw"
     src="https://picsum.photos/1200/600?image=1069" alt="">
```

### picture

```html
<picture>
   <source media="(min-width: 1200px)" srcset="https://picsum.photos/1200/600?image=1068">
   <source media="(min-width: 1000px)" srcset="https://picsum.photos/1000/500?image=1067">
   <source media="(min-width: 800px)" srcset="https://picsum.photos/800/400?image=1066">
   <source media="(min-width: 600px)" srcset="https://picsum.photos/600/600?image=1065">
  <source srcset="https://picsum.photos/400/200?image=1064 ">
   <img src="https://picsum.photos/1200/600?image=1069" alt="A photo of London by night">
</picture>

```

## CSS Background 屬性

### **Background Clip**

>   background-clip 顧名思義，背景剪切，用來設置元素的背景（背景圖片或顏色）是否延伸到邊框下面。

`background-clip: border-box;`  背景延伸至邊框外沿（但是在邊框下層）

`background-clip: padding-box;` 背景延伸至內邊距（padding）外沿。不會繪製到邊框處

`background-clip: content-box;` 背景被裁剪至內容區（content box）外沿

`background-clip: text;` 背景被裁剪成文字的前景色（實驗屬性，需要加瀏覽器前綴）

### **Background Origin**

>   此屬性需要與`background-position`配合使用。你可以用`background-position`計算定位是從border，padding或content boxes內容區域算起。（類似`background-clip`）

`background-origin：border-box;` 從border邊框位置算起

`background-origin：padding-box;` 從padding位置算起

`background-origin：content-box;` 從content-box內容區域位置算起；

### 搭配background製作漸層border

```css
.btn {
    display: inline-flex;
    align-items: center;
    justify-content: center;
    min-width: 290px;
    height: 90px;
    position: relative;
    border-radius: 50px;
    font-weight: 500;
    border: solid 5px transparent;
    color: #5e3700;
    font-size: 32px;
    margin: 20px;
}
.btn-default {
    color: #fff;
    box-shadow: 0 5px 15px rgba(0, 0, 0, 0.5);
    background-image: linear-gradient(
        to right,
        #ff7c2d 3%,
        #ff016e 97%
    ),
        linear-gradient(to bottom, #fff3b6, #e27d2c);
    background-origin: border-box;
    background-clip: padding-box, border-box;
}
```

```html
<div class="btn btn-default"></div>
```

### 搭配background製作漸層border

```css
.btn-anim{
    display: inline-flex;
    width: 290px;
    height: 90px;
    padding: 6px;
    position: relative;
}
.btn-anim:before {
    content: "";
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: linear-gradient(45deg, #0ebeff, #ffdd40, #ae63e4, #47cf73, #0ebeff, #ffdd40, #ae63e4, #47cf73);
    border-radius: 50px;
    background-size: 200%;
    transition: background-color .3s linear;
    animation: rainbow-border 3s linear infinite;
}
.btn-box{
    width: 100%;
    height: 100%;
    background-color: #fff;
    position: relative;
    border-radius: 50px;
}
@keyframes rainbow-border{
    0%{
        background-position: 0% 0%;
    }
    100%{
        background-position: 200% 50%;
    }
}
```

```html
<div class="btn-anim">
    <div class="btn-box"></div>
</div>
```

