## 拖動事件

繼續之前，有必要先瞭解拖動時會觸發哪些事件。考慮拖動 Source Element，途中經過 Intermediate Element，最終進入 Target Element 並鬆開鼠標，則路徑上會觸發的事件如下圖所示：

![](https://lotabout.me/2018/HTML-5-Drag-and-Drop/drag-and-drop-events.svg)

1.  `dragstart`：當我們“拖”起元素時會觸發。
2.  `dragenter`：當拖動元素 A 進入另一個元素 B 時，會觸發 B 的 `dragenter` 事件。
3.  `dragleave`：與 `dragenter` 相對應，當拖動元素 A 離開元素 B 時，觸發 B 的 `dragleave` 事件。
4.  `dragover`：當拖動元素 A 在另一個元素 B 中移動/停止時觸發 B 的 `dragover` 事件。文檔說是每幾百毫秒觸發一次，Chrome 實測 1ms 左右觸發；Firefox 大概是 300ms
5.  `drop`：當在拖動元素 A 到元素 B 上，釋放鼠標時觸發 B 的 `drop` 事件，相當於元素 B 接收了元素 A 。
6.  `dragend`：在 `drop` 事件之後，還會觸發元素 A 的 `dragend` 事件，這裡可以對元素 A 作一些清理工作。

除了上面的事件外，還有兩個一般用不到的事件：

1.  `drag`：和 `dragover` 類似，當元素 A 被拖動時，每隔一段時間就會觸發這個事件。與 `dragover` 不同，`drag` 事件是觸發在源元素 A 上，而 `dragover` 是觸發上潛在目標元素 B 上的。
2.  `dragexit`：這個事件只有 Firefox 支持，和 `dragleave` 作用幾乎相同，發生在 `dragleave` 之前。

如果想實際驗證一下這些事件是何時觸發的，可以看看[這個 jsfiddle](https://jsfiddle.net/lotabout/gq52cn3w/)，console 裡會輸出拖放的元素及對應的事件。下面我們開始一起實現咱們的拖放示例吧。

## 第一步 設置

```css
#drag-container {
    width: 600px;
    display: flex;
    justify-content: space-between;
}

#drag-container > .dropzone {
    width: 150px;
    height: 150px;
    background: #dae8fc;
    padding: 10px;
    border: 1px solid #6c8ebf;
    display: flex;
    flex-direction: column;
    justify-content: center;
}

#draggable {
    width: 90px;
    height: 90px;
    line-height: 90px;
    text-align: center;
    background: #f8cecc;
    border: 1px solid #b85450;
    margin: 0 auto;
    cursor: pointer;
}
```
一般在 HTML 裡，元素默認是不可以作為源元素的（除了 <a>，<img>），例如一個div ，我們是“拖不動”它的。這時只需要為它加上 `draggable="true"` 屬性它就能“拖”了。下面是我們的 DOM 結構：
```html
<div id="drag-container">
    <div class="dropzone">
        <div id="draggable" draggable="true">
            Drag Me
        </div>
    </div>
    <div class="dropzone"></div>
    <div class="dropzone"></div>
</div>
```



## 第二步 拖

`draggable` 元素上加了 `draggable="true"`，這樣我們就能拖動它了，起碼在 Chrome 裡可以，在 Firefox 裡我們還需要在 `dragstart` 裡為 `dataTransfer` 設置一些數據，因此需要加上下面的代碼。具體的作用我們之後會說。

#### 拖動初始化

```js
let draggable=document.getElementById("draggable");

draggable.addEventListener('dragstart',(ev)=>{
    ev.dataTransfer.setData("text/plain",null)
})
```

#### 拖動特效

首先，我們想在拖起元素讓原始的元素變成半透明，這樣當我們拖動時就會知道它是“真的可以拖動的”，而不是瀏覽器的什麼奇怪行為。為此，我們可以監聽 `dragstart` 事件：

```js
draggable.addEventListener('dragstart',(ev)=>{
    ev.target.style.opacity=".5"
    ev.dataTransfer.setData("text/plain",ev.target.id)
})
```

這樣一來我們開始拖動元素，它就變得透明了，然而我們鬆開鼠標，它依舊保持透明！這可不是我們想要的結果，因此我們需要監聽 `dragend` 在拖動結束後還原透明度：

```js
draggable.addEventListener("dragend",(ev)=>{
    ev.target.style.opacity="1"
})
```



## 第三步 移動

下面，我們希望拖著元素 A 進入目標 B 時讓 B 的邊框變成虛線，以示意我們可以放入元素。

```js
let dropzones = document.querySelectorAll('.dropzone');
dropzones.forEach((dropzone) => {

  dropzone.addEventListener('dragenter', (ev) => {
    ev.preventDefault();
    dropzone.style.borderStyle = 'dashed';
    return false;
  });

  dropzone.addEventListener('dragover', (ev) => {
    ev.preventDefault();
    return false;
  });

  dropzone.addEventListener('dragleave', (ev) => {
    dropzone.style.borderStyle = 'solid';
  });
});
```

我們為所有的 `dropzone` 都監聽了 `dragenter` 及 `dragleave` 事件，當拖動元素進入它們時，邊框會變成虛線，離開時變回實線。這裡有幾個注意點：

-   在 `dragenter` 與 `dragover` 裡我們調用了 `ev.preventDefault()`，事實上幾乎所有元素默認都是不允許 drop 發生的，這裡調用`ev.preventDefault()` 可以阻止默認行為。
-   在 `dragenter` 中我們通過 `dropzone` 變量來修改樣式而不是 `ev.target`，你可能覺得 `ev.target` 指向的是目標 B 元素，然而它指向的是源元素 A。
-   我們在 `dragenter` 而不是 `dragover` 中修改樣式，是因為 `dragover` 會觸發太頻繁了。

## 第四步 放

### 數據傳輸 DataTransfer

拖動是最終目的是為了對源和目標元素做一些操作。為了完成操作，需要在源和目標傳輸數據，我們可以通過設置/讀取全局變量來完成，這並不是一個好習慣。在 HTML 5 中，我們通過 [DataTransfer](https://developer.mozilla.org/en-US/docs/Web/API/DataTransfer) 完成。

我們在 `dragstart` 時設置需要傳輸的數據，在 drop 中獲取需要的數據。 `event.dataTransfer` 提供了兩個主要函數：

-   `setData(format, data)`：用於添加數據，一般 format 對應於 MIME 類型字符串，常見的有 `text/plain`、`text/html` 及 `text/uri-list`等，但同時也可以是任意自定義的類型；不幸的是 data 只能是 `string` 或 `file`。
-   `getData(format)`：用於獲取數據。

我們要實現將 `Drag Me` 放到其它藍色元素中，需要傳輸它的 ID ，通過下面的代碼實現：

```js
draggable.addEventListener('dragstart', (ev) => {
  ev.target.style.opacity = ".5";

  // 設置 ID
  ev.dataTransfer.setData('text/plain', ev.target.id);
});

dropzones.forEach((dropzone) => {
  dropzone.addEventListener('drop', (ev) => {
    ev.preventDefault()
    ev.target.style.borderStyle = 'solid';

    // 獲取 ID
    const sourceId = ev.dataTransfer.getData('text/plain')
    ev.target.appendChild(document.getElementById(sourceId))
  })
});
```

-   在 `dragstart` 時通過 `setData` 將 ID 放入 `DataTransfer` 中
-   在 `drop` 事件中，通過 `getData` 獲取元素 ID 並通過 `appendChild` 加入到藍色元素中。