## CSS變量

`.box`如果有變量設定`--color`時顏色為orange，沒有則red

```css
box{
    --color:orange;
    background-color:--var(--color,red);
}
```

## CSS 3D Card翻轉

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

.card-container{
    perspective: 600px;
    width: 150px;
    height: 150px;
}
.card-container.active .card{
    transform: rotateY(180deg);
}
.card{
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

RWD Youtube
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