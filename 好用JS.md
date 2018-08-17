## 千位符轉換

```js
var toThousands = function(number) {
    return (number + '').replace(/(\d)(?=(\d{3})+$)/g, '$1,');
}
```

