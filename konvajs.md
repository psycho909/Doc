# konvajs

```js
白色
247,244,223
藍色
16,167,230
粉
255,146,146
黃
255,194,76
咖啡
111,77,40
紫
131,64,194
綠
96,193,91
紅
255,63,63
```



#### 改變圖片顏色

```js
var shape = stage.find('.cake')[0];
shape.cache()
shape.filters([Konva.Filters.RGB])
shape["red"](16)
shape["green"](167)
shape["blue"](230)
```

#### 獲取所有屬性

```js
stage.find(id)[0].getAttrs()
```

#### 設置所有屬性

```js
stage.find(id)[0].setAttrs(configs)
```



#### 匯出base64

```js
stage.toDataURL();
```

### 清除畫面

```js
layer.removeChildren()
```

