## 取消行動版 safari 自動偵測數字成電話號碼

```html
<meta name="format-detection" content="telephone=no">
```

## 增加撥號功能

```html
<a href="tel:0912345678">撥號</a>
```

## 正確呼叫數字鍵盤並有驗證功能

> `inputmode="numeric"`

```html
<input
  type="text"
  name="token"
  id="token"
  inputmode="numeric"
  pattern="[0-9]*"
/>
```

## HTML autocomplete

`autocomplete="one-time-code"`

在可以獲取簡訊驗證碼

```html
<input
  type="text"
  name="token"
  id="token"
  inputmode="numeric"
  pattern="[0-9]*"
  autocomplete="one-time-code"
/>
```

