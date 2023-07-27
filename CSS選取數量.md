#CSS
### 取前N個

```css
.num1 li:nth-of-type(-n+3){
    background-color: red;
}
```

### 從第N個開始

```css
.num2 li:nth-of-type(n+3){
    background-color: green;
}
```

### 從第N個開始到第N個結束(取交集)

```css
.num3 li:nth-of-type(n+2):nth-of-type(-n+4){
    background-color: #000;
}
```

### 從最後一個開始取N個

```css
.num4 li:nth-last-child(-n+3){
    background-color: blue;
}
```

### 從最後N個開始

```css
.num5 li:nth-last-child(n+3){
    background-color: orange;
}
```

### 選擇第N個

#### 1.第五個就變色

```css
.num5 li:nth-of-type(3n+2){
    background-color: orange;
}
```

