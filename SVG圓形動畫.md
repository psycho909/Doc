### 填充過渡

可以在兩個圓形SVG屬性的幫助下創建圓形動畫：`stroke-dasharray` 和 `stroke-dashoffset`

“`stroke-dasharray` 定義筆劃中的虛線間隙模式。”

它最多可能需要四個值：

當它被設置為唯一的整數（ `stroke-dasharray:10` ）時，破折號和間隙具有相同的大小;
對於兩個值（ `stroke-dasharray:10 5` ），第一個應用於破折號，第二個應用於間隙;
第三種和第四種形式（`stroke-dasharray:10 5 2` 和 `stroke-dasharray:10 5 2 3` ）將產生各種樣式的虛線和間隙。

![](https://cloud.netlifyusercontent.com/assets/344dbf88-fdf9-42bb-adb4-46f01eedd629/87b50f18-b9b2-47bb-b88b-519662900d51/dasharray-animation.gif)

圖：`stroke-dasharray`屬性值

左邊的圖像顯示屬性`stroke-dasharray`設置為 0 到圓周長度 238px。

第二個圖像表示 `stroke-dashoffset` 屬性，它抵消了dash數組的開頭。 它的取值範圍也是從0到圓周長度

![](https://cloud.netlifyusercontent.com/assets/344dbf88-fdf9-42bb-adb4-46f01eedd629/6e2ad1cc-d1f8-4143-97d3-5e401208d750/stroke-properties.gif)

圖：`stroke-dasharray` 和 `stroke-dashoffset` 屬性

為了產生填充效果，我們將 `stroke-dasharray` 設置為圓周長度，以便它所有長度都能充滿其衝刺範圍而不留間隙。 我們也會用相同的值抵消它，這樣會使它能夠被“隱藏”。 然後，`stroke-dashoffset` 將更新為對應的說明文字，根據過渡持續時間填充其行程。

屬性更新將通過[CSS Variables](https://developer.mozilla.org/en-US/docs/Web/CSS/Using_CSS_variables)在腳本中完成。 下面讓我們聲明變量並設置屬性：

```css
.circle__progress--fill {
  --initialStroke: 0;
  --transitionDuration: 0;
  stroke-opacity: 1;
  stroke-dasharray: var(--initialStroke);
  stroke-dashoffset: var(--initialStroke);
  transition: stroke-dashoffset var(--transitionDuration) ease;
}
```

為了設置初始值並更新變量，讓我們從使用 [`document.querySelectorAll`](https://developer.mozilla.org/en-US/docs/Web/API/Document/querySelectorAll) 選擇所有`.note-display`元素開始。 同時把 `transitionDuration`設置為900毫秒。

然後，我們遍歷顯示數組，選擇它的 `.circle__progress.circle__progress--fill` 並提取HTML中的 `r` 屬性集來計算周長。 有了它，我們可以設置初始的 `--dasharray` 和 `--dashoffset` 值。

當 `--dashoffset` 變量被 `setTimeout `更新時，將發生動畫：

```js
const displays = document.querySelectorAll('.note-display');
const transitionDuration = 900;

displays.forEach(display => {
  let progress = display.querySelector('.circle__progress--fill');
  let radius = progress.r.baseVal.value;
  let circumference = 2 * Math.PI * radius;

  progress.style.setProperty('--transitionDuration', `${transitionDuration}ms`);
  progress.style.setProperty('--initialStroke', circumference);

  setTimeout(() => progress.style.strokeDashoffset = 50, 100);
});
```

要從頂部開始過度，必須旋轉 `.circle__svg` 元素：

```css
.circle__svg {
  transform: rotate(-90deg);
}
```

![](https://cloud.netlifyusercontent.com/assets/344dbf88-fdf9-42bb-adb4-46f01eedd629/9823b0f4-4e9e-4476-a75c-b12a6c235d7c/fill-animation.gif)

圖：Stroke 屬性轉換

現在，讓我們計算相對於 note 的`dashoffset`值。 note 值將通過 `data-*` 屬性插入每個`li`項目。` *` 可以替換為任何符合你需求的名稱，然後可以通過元素的數據集在元數據集中檢索：`element.dataset.*`。

注意：你可以在[*MDN Web Docs*](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/data-*)上得到有關 `data-*` 屬性的更多信息。

我們的屬性將被命名為 “`data-note`”：

```html
<ul class="display-container">
	<li class="note-display" data-note="7.50">
        <div class="circle">
          <svg width="84" height="84" class="circle__svg">
            <circle cx="41" cy="41" r="38" class="circle__progress circle__progress--path"></circle>
            <circle cx="41" cy="41" r="38" class="circle__progress circle__progress--fill"></circle>
          </svg>

          <div class="percent">
            <span class="percent__int">0.</span>
            <span class="percent__dec">00</span>
          </div>
        </div>

        <span class="label">Transparent</span>
  	</li>
</ul>
```

`parseFloat`方法將`display.dataset.note`返回的字符串轉換為浮點數。 `offset` 表示達到最高值時缺失的百分比。 因此，對於 `7.50` note，我們將得到 `(10 - 7.50) / 10 = 0.25`，這意味著 `circumference` 長度應該偏移其值的25％：

```js
let note = parseFloat(display.dataset.note);
let offset = circumference * (10 - note) / 10;
```

```js
const displays = document.querySelectorAll('.note-display');
const transitionDuration = 900;

displays.forEach(display => {
  let progress = display.querySelector('.circle__progress--fill');
  let radius = progress.r.baseVal.value;
  let circumference = 2 * Math.PI * radius;
+ let note = parseFloat(display.dataset.note);
+ let offset = circumference * (10 - note) / 10;

  progress.style.setProperty('--initialStroke', circumference);
  progress.style.setProperty('--transitionDuration', `${transitionDuration}ms`);

+ setTimeout(() => progress.style.strokeDashoffset = offset, 100);
});

```

![](https://cloud.netlifyusercontent.com/assets/344dbf88-fdf9-42bb-adb4-46f01eedd629/894e4046-1c16-4168-8989-9f4bd1fedaaa/fill-to-percent-animation.gif)

sroke屬性轉換為note值

在繼續之前，讓我們將stoke轉換提取到它自己的方法中：

```js
const displays = document.querySelectorAll('.note-display');
const transitionDuration = 900;

displays.forEach(display => {
- let progress = display.querySelector('.circle__progress--fill');
- let radius = progress.r.baseVal.value;
- let circumference = 2 * Math.PI * radius;
  let note = parseFloat(display.dataset.note);
- let offset = circumference * (10 - note) / 10;

- progress.style.setProperty('--initialStroke', circumference);
- progress.style.setProperty('--transitionDuration', `${transitionDuration}ms`);

- setTimeout(() => progress.style.strokeDashoffset = offset, 100);

+ strokeTransition(display, note);
});

+ function strokeTransition(display, note) {
+   let progress = display.querySelector('.circle__progress--fill');
+   let radius = progress.r.baseVal.value;
+   let circumference = 2 * Math.PI * radius;
+   let offset = circumference * (10 - note) / 10;

+   progress.style.setProperty('--initialStroke', circumference);
+   progress.style.setProperty('--transitionDuration', `${transitionDuration}ms`);

+   setTimeout(() => progress.style.strokeDashoffset = offset, 100);
+ }

```

### 注意增長值

還有一件事就是把 note 從`0.00`轉換到要最終的 note 值。 首先要做的是分隔整數和小數值。 可以使用字符串方法`split()`。 之後它們將被轉換為數字，並作為參數傳遞給 `increaseNumber()` 函數，通過整數和小數的標誌正確顯示在對應元素上。

```js
const displays = document.querySelectorAll('.note-display');
const transitionDuration = 900;

displays.forEach(display => {
  let note = parseFloat(display.dataset.note);
+ let [int, dec] = display.dataset.note.split('.');
+ [int, dec] = [Number(int), Number(dec)];

  strokeTransition(display, note);

+ increaseNumber(display, int, 'int');
+ increaseNumber(display, dec, 'dec');
});
```

在 `increaseNumber()` 函數中，我們究竟選擇 `.percent__int` 還是 `.percent__dec` 元素，取決於 `className` ，以及輸出是否應包含小數點。 接下來把`transitionDuration`設置為900毫秒。 現在，動畫表示從0到7的數字，持續時間必須除以note `900 / 7 = 128.57ms`。 結果表示每次增加迭代將花費多長時間。 這意味著 `setInterval`將每隔 `128.57ms` 觸發一次。

設置好這些變量後，接著定義`setInterval`。 `counter` 變量將作為文本附加到元素，並在每次迭代時增加：

```js
function increaseNumber(display, number, className) {
  let element = display.querySelector(`.percent__${className}`),
      decPoint = className === 'int' ? '.' : '',
      interval = transitionDuration / number,
      counter = 0;

  let increaseInterval = setInterval(() => {
    element.textContent = counter + decPoint;
    counter++;
  }, interval);
}
```

![](https://cloud.netlifyusercontent.com/assets/344dbf88-fdf9-42bb-adb4-46f01eedd629/c625f9f1-6d5c-4645-93cc-bb09d95905b3/infinite-counter-animation.gif)

圖：計數增長

太酷了！ 確實增加了計數值，但它在無限循環播放。 當note達到我們想要的值時，還需要清除`setInterval`。 可以通過`clearInterval`函數完成：

```js
function increaseNumber(display, number, className) {
  let element = display.querySelector(`.percent__${className}`),
      decPoint = className === 'int' ? '.' : '',
      interval = transitionDuration / number,
      counter = 0;

  let increaseInterval = setInterval(() => {
+   if (counter === number) { window.clearInterval(increaseInterval); }

    element.textContent = counter + decPoint;
    counter++;
  }, interval);
}
```

![](https://cloud.netlifyusercontent.com/assets/344dbf88-fdf9-42bb-adb4-46f01eedd629/b6445706-49ad-4f26-b99e-ad2a9bf030d9/finished-project.gif)

圖：最終完成

現在，數字更新到note值，並使用`clearInterval()`函數清除。