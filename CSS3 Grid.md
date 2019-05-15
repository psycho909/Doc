# CSS3-grid
## 設置`grid`
> 通過`display`屬性值為`grid`或`inline-grid`可創建一個網格容器。
## 顯示網格
> 使用`grid-template-columns`和`grid-template-rows`屬性可顯示設置一個網格的列和行。<br>
### `fr`單位
> `fr`單位可以幫助我們創建一個彈性的惘格軌道<br/>
> 網格容器分成4等份(1+1+2=4),每一份(1fr)是網格容器寬度的1/4，所以要`item1`和`item2`寬度是網格容器1/4寬，`item3`是2/4(2fr)
```css
grid-template-columns:1fr 1fr 2fr;
```
> 當`fr`和其他長度單位的值在一起時，`fr`是基於網個容器可用空間來計算<br>
> 例子中，網格容器可用空間是網格寬度減去`3rem`和`25%`剩下的寬度:
```
1fr=(網格寬度 - 3rem -網格寬度*25%) / 3
```
```css
grid-template-columns:3rem 25% 1fr 2fr;
```
### `minmax()`網格軌道最小和最大尺寸
> `grid-template-columns:minmax(min,max)`，可接受2個參數，第一個參數是最小值，第二個參數是最大值，可接受auto值。auto值允許內容尺寸拉伸或擠壓。
```css
grid-template-columns:minmax(50px,auto);
```
### 重覆網格軌道`repeat()`
> `repeat()`可以創建重複的惘格軌道，可接受兩個參數:第一個參數定義重複次數，第二個參數定義每個網格尺寸
```css
<!-- 1 -->
grid-template-columns:repeat(3,1fr);
<!-- 2 -->
grid-template-columns:30px repeat(3,1fr) 30px;
```
### 間距
> `grid-column-gap`和`grid-row-gap`屬性用來創建列與列，行與行之間的間距。
```css
grid-row-gap:20px;
grid-column-gap:5rem;
```
> `grid-gap`是`grid-row-gap`和`grid-column-gap`的縮寫
## 通過網格線號碼來定位網格
> 網格線是代表開始、結束
>
> 每條線是以網格軌道開始，網格的惘格線從`1`開始，逐步增加`1`<br>
> 改變前

|1|2|
|---|---|
|3|4|
|5|6|

> 改變後

|2|3|
|---|---|
|4|1|
|5|6|
```css
.item1{
    grid-row-start:2;
    grid-row-end:3;
    grid-column-start:2;
    grid-column-end:3;
}
```
> `gird-row`和`grid-coumn`為縮寫，使用`/`分隔`start`和`end`
```css
.item1{
    grid-row:2/3;
    grid-column:2/3;
}
```
> `grid-area`是縮寫，`row-start/column-start/row-end/column-end`
```css
.item1{
    grid-area:2/2/3/3;
}
```
### 網格項目跨行或跨列

|1|1|1|
|---|---|---|
|2|3|4|
```css
.item1{
  grid-column-start:1;
  grid-column-end:4;
}
```
> 可使用`span`後面緊隨數字，表示合併多少列或行<br>
> 如`span 2`指占據2個軌道數<br>

|2|3|4|
|---|---|---|
|1|1|5|
|1|1|6|
|1|1|7|
```css
.item1{
  grid-row:2 / span 3;
  grid-column:span 2;
}
```
## 網格線命名
> 通過`grid-template-rows`和`grid-template-columns`定義網格時，網格線可以被命名，網格線名稱可以設置網格項目位置。
```css
.grid{
  grid-template-rows:[row-1] 100px [row-2] 100px [row-3];
  grid-template-columns:[col-1 ]1fr [col-2] 1fr [col-3] 1fr [col-4];
}
```
### 使用命名

```css
.grid{
     grid-template-columns:[start] minmax(20px,1fr) [wrapper-start] repeat(12,minmax(0,100px)) [wrapper-end] minmax(20px,1fr) [end];
}
.grid__media{
    gird-column:wrapper-start / 9;
}
```

### 通過網個線名稱設置網格項目位置

```css
.item-b:nth-child(1){
    grid-row-start:row-1;
    grid-row-end:row-2;
    grid-column-start:col-2;
    grid-column-end:col-3;
}
```
## 通過網個區域命名和定位網格項目
> 可使用區域名稱快速配置網格軌道

|header|header|header|
|---|---|---|
|content|content|sidebar|
|footer|footer|footer|
```css
.grid{
    display:grid;
    width: 1000px;
    grid-template-areas:"header header"
                        "content sidebar"
                        "footer footer";
    grid-template-rows:100px 200px 100px;
    grid-template-columns:1fr 200px;
    grid-gap:2px;
    margin-top: 1rem;
}
.header{
    background-color: green;
    grid-row:header;
    grid-column:header;
}
.content{
    background-color: pink;
    grid-row:content;
    grid-column:content;
}
.sidebar{
    background-color: orangered;
    grid-row:sidebar;
    grid-column:sidebar;
}
.footer{
    background-color: purple;
    grid-row:footer;
    grid-column:footer;
}
```
```html
<div class="grid-box-c">
    <div class="grid item-c header">header</div>
    <div class="grid item-c content">content</div>
    <div class="grid item-c sidebar">sidebar</div>
    <div class="grid item-c footer">footer</div>
</div>
```
## 隱式網格
> 當網格項目確認在顯示網格之外時就會創建隱式網格，當沒有足夠的空間或顯示的網格軌道來設置網格項目時，此時網格項目就會自動創建隱式網格。
>
> 多出個`row`網格皆以110px大小顯示。
```css
.grid{
  grid-template-rows:70px;
  grid-template-columns:repeat(2,1fr);
  grid-auto-rows:110px;
}
```
## 網格默認流方向
> 網格默認流方向`row`
```css
grid-auto-flow:column;
```
## 網格項目對齊方式
> 可使用`flex`中的`justify-content`和`aligns-content`進行對齊
## 彈性網格設定
> 網格有兩個非常有用的特性來適應可用空間的變化。這兩個屬性叫`auto-fit`和`auto-fill`：<br>
> `auto-fit`會嚐試在不導致列溢出的情況下，放至最大數量的重複元素
```css
.grid{
    grid-template-columns: repeat(auto-fit, minmax(250px, 1fr))
}
```
> `auto-fill`與`auto-fill`類似，但是任何的空白空間都會自動收縮，同時這一行的元素也會被拉升——類似flexbox的效果，列會隨著可用空間變小發生摺疊。

## **grid-auto-flow**

1.  `row`:該關鍵字指定自動佈局算法按照通過逐行填充來排列元素，在必要時增加新行。如果既沒有指定 `row` 也沒有 `column`，則默認為 `row`。
2.  `column`:該關鍵字指定自動佈局算法通過逐列填充來排列元素，在必要時增加新列。
3.  `dense`:該關鍵字指定自動佈局算法使用一種“稠密”堆積算法，如果後面出現了稍小的元素，則會試圖去填充網格中前面留下的空白。這樣做會填上稍大元素留下的空白，但同時也可能導致原來出現的次序被打亂。

```css
grid-auto-flow: row;
grid-auto-flow: column;
grid-auto-flow: dense;
grid-auto-flow: row dense;
grid-auto-flow: column dense;
```

