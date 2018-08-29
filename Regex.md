# 正則表達式

## 正則符號

- `\` : 可以脫逸字元
- `[` `]` : [xyz] 比對中括孤內的任一個字元 , 但不會將配對到的字串存入RegExp變數中
- `(` `)` : 合起來 (x) 比對x並將符合的部份存入一個變數, /(a*) and (b*)/ 可以對 ‘aaa and bb’ 中的’aaa’和’bb’, 並將這兩個比對得到的字串設定至數字 RegExp.$1 和 RegExp.$2
- `.` : 比對任何一個字元 , 但換行符號不算
- `^` : 啟始位置
- `$` : 結束位置
- `*` : 比對前一個字元 , 零次或更多次 , 等於{0,}
- `?` : 比對前一個字元 , 零次或一次 , 等於{0,1}
- `+` : 比對前一個字元 , 一次或更多次 , 也等於 {1,}
- `|` : 或者 , 等於OR , 邏輯運算子(Logical Operators)

## 符號對照

- `\d` = `[0-9]` : 0~9(數字)
- `\D` = `[^0-9]` 或 `[^\d]` : 非數字
- `\l` = `[a-z]` : 小寫英文
- `\L` = `[^a-z]` : 非小寫英文
- `\u` = `[A-Z]` : 大寫英文
- `\U` = `[^A-Z]` : 非大寫英文
- `\n` : 比對換行符號
- `\r` : 比對 carriage return
- `\s` : 比對任一個空白字元(White space character) , 等於 `[\f\n\r\t\v]`
- `\S` : 比對任一個非空白字元 , 等於 `[^ \f\n\r\t\v]`
- `\t` : 比對字位字元(Tab)
- `\v` : 比對垂直定位字元(Vertical tab)
- `\w` : 比對數字、字母字元或底線 , 等於 `[A-Za-z0-9_]`
- `\W` : 比對非數字、字母字元或底線 , 等於 `[^A-Za-z0-9_]`
- `\K` : start at this position
- `.{6,30}` : 6 < 字串長度 < 30
- `?=.*` : 用來判斷右邊緊接的字元是否符合比對條件
- `(?!.*[^\x21-\x7e])` : 不允許特殊符號、數字、英文以外的字元
- `(?=.{10,})` : 檢查字元長度是否超過10
- `(?!.*[\W])` : 不允許非(數字、字母、底線)的字元
- `(?=.*\d)`：這是 Positive Lookahead，用來判斷右邊緊接著的字元是否符合比對條件，如果符合條件才會繼續比對下去。拿這個實例來說，右邊的字元必須包含一個數字才算符合這個條件。
- `(?=.*[a-zA-Z])`：跟上面出現過得 Positive Lookahead 一樣，這是說右邊的字必須包含一個 a 到 z 或是 A 到 Z 的字元，說穿了就是右邊的字要包含一個英文字的意思。

## 比對方式

- g：全域比對（Global match）
- i：忽略大小寫（Ignore case）
- s : 表示我要進行跨行(\n)做比對 //似乎沒有這功能 , 有待查證
- o : 表示我只要比對一次 //似乎沒有這功能 , 有待查證
- gi：全域比對並忽略大小寫

## 簡單比對替換

```js
"i321nfo123".replace(/[^a-z]+/i,''); // info
```



## Javascript 正則(sure/false/前sure/後sure)

```js
var str1 = 'foobar';
var str2 = 'foobaz';

(/foo(?=bar)/).test(str1) // true
(/foo(?!bar)/).test(str1) // false
(/(?<=foo)bar/).test(str1) // true
(/(?<!foo)bar/).test(str1) // false
```
## 使用可能な正規表現フラグはg, i, m, u, y

| フラグ | 説明                                          |
| ------ | --------------------------------------------- |
| g      | グローバルサーチ                              |
| i      | 大文字・小文字を区別しない検索                |
| m      | 複数行検索                                    |
| u      | unicodeパターンの扱い変更                     |
| y      | lastIndexで指定したマッチの位置から検索を開始 |