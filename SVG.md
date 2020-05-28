# SVG

## 語法

### `<circle>`

`<circle>`標籤的`cx`、`cy`、`r`屬性分別為橫坐標、縱坐標和半徑，單位為像素。坐標都是相對於`<svg>`畫布的左上角原點。 

```html
<svg width="300" height="180">
  <circle cx="30"  cy="50" r="25" />
  <circle cx="90"  cy="50" r="25" class="red" />
  <circle cx="150" cy="50" r="25" class="fancy" />
</svg>
```

```css
.red {
  fill: red;
}

.fancy {
  fill: none;
  stroke: black;
  stroke-width: 3pt;
}
```

### `<line>`

`<line>`標籤的`x1`屬性和`y1`屬性，表示線段起點的橫坐標和縱坐標；`x2`屬性和`y2`屬性，表示線段終點的橫坐標和縱坐標 

```html
<svg width="300" height="180">
  <line x1="0" y1="0" x2="200" y2="0" style="stroke:rgb(0,0,0);stroke-width:5" />
</svg>
```

### `<polyline>`

`<polyline>`標籤用於繪製一根折線 

```html
<svg width="300" height="180">
  <polyline points="3,3 30,28 3,53" fill="none" stroke="black" />
</svg>
```

`<polyline>`的`points`屬性指定了每個端點的坐標，橫坐標與縱坐標之間與逗號分隔，點與點之間用空格分隔。 

### `<rect>`

```html
<svg width="300" height="180">
  <rect x="0" y="0" height="100" width="200" style="stroke: #70d5dd; fill: #dd524b" />
</svg>
```

`<rect>`的`x`屬性和`y`屬性，指定了矩形左上角端點的橫坐標和縱坐標；`width`屬性和`height`屬性指定了矩形的寬度和高度（單位像素） 

### `<polygon>`

`<polygon>`標籤用於繪製多邊形。 

```html
<svg width="300" height="180">
  <polygon fill="green" stroke="orange" stroke-width="1" points="0,0 100,0 100,100 0,100 0,0"/>
</svg>
```

`<polygon>`的`points`屬性指定了每個端點的坐標，橫坐標與縱坐標之間與逗號分隔，點與點之間用空格分隔。 

### `<path>`

`<path>`標籤用於制路徑 

```html
<svg width="300" height="180">
<path d="
  M 18,3
  L 46,3
  L 46,40
  L 61,40
  L 32,68
  L 3,40
  L 18,40
  Z
"></path>
</svg>
```

`<path>`的`d`屬性表示繪製順序，它的值是一個長字符串，每個字母表示一個繪製動作，後面跟著坐標。

> - M：移動到（moveto）
> - L：畫直線到（lineto）
> - Z：閉合路徑

### `<text>`

`<text>`標籤用於繪製文本。 

```html
<svg width="300" height="180">
  <text x="50" y="25">Hello World</text>
</svg>
```

`<text>`的`x`屬性和`y`屬性，表示文本區塊基線（baseline）起點的橫坐標和縱坐標。文字的樣式可以用`class`或`style`屬性指定。 

### `<use>`

`<use>`標籤用於複製一個形狀 

```html
<svg viewBox="0 0 30 10" xmlns="http://www.w3.org/2000/svg">
  <circle id="myCircle" cx="5" cy="5" r="4"/>

  <use href="#myCircle" x="10" y="0" fill="blue" />
  <use href="#myCircle" x="20" y="0" fill="white" stroke="blue" />
</svg>
```

`<use>`的`href`屬性指定所要複製的節點，`x`屬性和`y`屬性是`<use>`左上角的坐標。另外，還可以指定`width`和`height`坐標。 

### `<g>`

`<g>`標籤用於將多個形狀組成一個組（group），方便復用。 

```html
<svg width="300" height="100">
  <g id="myCircle">
    <text x="25" y="20">圓形</text>
    <circle cx="50" cy="50" r="20"/>
  </g>

  <use href="#myCircle" x="100" y="0" fill="blue" />
  <use href="#myCircle" x="200" y="0" fill="white" stroke="blue" />
</svg>
```

### `<defs>`

`<defs>`標籤用於自定義形狀，它內部的代碼不會顯示，僅供引用。 

```html
<svg width="300" height="100">
  <defs>
    <g id="myCircle">
      <text x="25" y="20">圓形</text>
      <circle cx="50" cy="50" r="20"/>
    </g>
  </defs>

  <use href="#myCircle" x="0" y="0" />
  <use href="#myCircle" x="100" y="0" fill="blue" />
  <use href="#myCircle" x="200" y="0" fill="white" stroke="blue" />
</svg>
```

### `<pattern>`

`<pattern>`標籤用於自定義一個形狀，該形狀可以被引用來平鋪一個區域。 

```html
<svg width="500" height="500">
  <defs>
    <pattern id="dots" x="0" y="0" width="100" height="100" patternUnits="userSpaceOnUse">
      <circle fill="#bee9e8" cx="50" cy="50" r="35" />
    </pattern>
  </defs>
  <rect x="0" y="0" width="100%" height="100%" fill="url(#dots)" />
</svg>
```

`<pattern>`標籤將一個圓形定義為`dots`模式。`patternUnits="userSpaceOnUse"`表示`<pattern>`的寬度和長度是實際的像素值。然後，指定這個模式去填充下面的矩形。 

### `<image>`

`<image>`標籤用於插入圖片文件。 

```html
<svg viewBox="0 0 100 100" width="100" height="100">
  <image xlink:href="path/to/image.jpg"
    width="50%" height="50%"/>
</svg>
```

### `<animate>`

`<animate>`標籤用於產生動畫效果。 

```html
<svg width="500px" height="500px">
  <rect x="0" y="0" width="100" height="100" fill="#feac5e">
    <animate attributeName="x" from="0" to="500" dur="2s" repeatCount="indefinite" />
  </rect>
</svg>
```

`<animate>`的屬性含義如下。

> - attributeName：發生動畫效果的屬性名。
> - from：單次動畫的初始值。
> - to：單次動畫的結束值。
> - dur：單次動畫的持續時間。
> - repeatCount：動畫的循環模式。

###  `<animateTransform>`

`<animate>`標籤對 CSS 的`transform`屬性不起作用，如果需要變形，就要使用`<animateTransform>`標籤 

```html
<svg width="500px" height="500px">
  <rect x="250" y="250" width="50" height="50" fill="#4bc0c8">
    <animateTransform attributeName="transform" type="rotate" begin="0s" dur="10s" from="0 200 200" to="360 400 400" repeatCount="indefinite" />
  </rect>
</svg>
```

`<animateTransform>`的效果為旋轉（`rotate`），這時`from`和`to`屬性值有三個數字，第一個數字是角度值，第二個值和第三個值是旋轉中心的坐標。`from="0 200 200"`表示開始時，角度為0，圍繞`(200, 200)`開始旋轉；`to="360 400 400"`表示結束時，角度為360，圍繞`(400, 400)`旋轉。 

## 圖片模糊效果+遮罩清晰效果 不兼容IE9

filter效果:`feGaussianBlur`模糊效果，`stdDeviation`參數模糊程度

```html
<filter id="filter2">
    <feGaussianBlur stdDeviation="5" />
</filter>
```

1.  第一層圖片image對應`filter="url(#filter)"`那一層，達到模糊效果
2.  `mask`對應`filter`那一層，並第二層圖片對應`mask="url(#mask)"`那一層，達到遮罩清晰效果

```html
        <svg class="pic" width="100%">
            <image filter="url(#filter)" xlink:href="https://images.pexels.com/photos/249798/pexels-photo-249798.png" width="100%" height="100%"></image>
            <filter id="filter">
                <feGaussianBlur stdDeviation="5" />
            </filter>
            <mask id="mask">
                <circle cx="-50%" cy="-50%" r="80" fill="white" filter="url(#filter)" />
            </mask>
            <image xlink:href="https://images.pexels.com/photos/249798/pexels-photo-249798.png" width="100%" height="100%" mask="url(#mask)"></image>
        </svg>
```

```js
$(".pic").mousemove(function(e){
    e.preventDefault();
    var upX=e.clientX;
    var upY=e.clientY;
    var mask=$("#mask circle")[0];
    mask.setAttribute("cy",(upY-5)+"px");
    mask.setAttribute("cx",(upX-5)+"px");
})
```

## SVG 多張圖拼接

1. 先在`illustrator `上畫好每一塊的路徑，在每一塊的路徑上修改名稱，並在每一塊路徑上面添加 `物件`  ->`圖樣`

2. 開啟SVG修改

```html
<!-- 圖片 -->
<defs>
    <pattern id="新增圖樣" data-name="新增圖樣" width="109" height="109" patternUnits="userSpaceOnUse" viewBox="0 0 109 109">
      <rect width="109" height="109" style="fill: none"/>
      <rect width="50" height="50"/>
    </pattern>
  </defs>
<!-- 區塊 -->
<path d="M150,14.4l44.06,89.27L292.58,118l-71.29,69.49,16.83,98.12L150,239.28,61.88,285.6l16.83-98.12L7.42,118l98.52-14.32Z" transform="translate(-7.42 -14.4)" style="fill: url(#新增圖樣)"/>
```

修改成

```html
<!-- 切好圖片 在每一塊路徑上做一塊 圖樣樣式 -->
        <!-- width 100% height 100% -->
        <pattern id="image-01" patternUnits="objectBoundingBox" patternContentUnits="userSpaceOnUse" width="100%" height="100%">
            <!-- 放入圖片 設定好寬度 -->
            <!-- 可製作圖片雪碧圖 -->
            <image xlink:href="images15th-sm/images-sm-01.png" width="152" height="118" x="0" y="0" />
        </pattern>
```

製作圖片變化

```js
$("#image-01 image").attr("y",-($("#image-01 image").height()/2))
```

