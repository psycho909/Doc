## 一般物件RWD 使用rem

```js
設計稿 1200w

var oHtml=document.getElementByTagName('html')[0];
var w=oHtml.clientWidth;
oHtml.style.fontSize=(w/1200)*16+'px';
```



## 一般物件RWD 使用%

```js
設計稿 1200w
元素 300*600

元素寬度: 300/1200*100
元素高度: 600/1200*100
```

## Sprite RWD

```js
設計稿 1200w
sprite圖寬高 961*916
元素sprite圖位置 -243*-761
元素 81*81
```
```js
// 先求出DOM 寬高
width: 81/1200*100=6.75%
padding-bottom: 81/1200*100=6.75%
```

```js
// 求出 background-size
// 100 * spriteWidth / tilewidth

x: 100*961/81=1186.4197530864199%
y: 100*916/81=1130.8641975308642%
```

```js
// 求出 background-position
// 100 * tileXOffset / Math.abs(tileWidth - spriteWidth)
// 100 * tileYOffset / Math.abs(tileHeight - spriteHeight)

x: 100*243/Math.abs(81-961)=27.613636363636363%
y: 100*761/Math.abs(81-916)=91.1377245508982%
```



