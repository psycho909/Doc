# Pug

## 結構語法

### 標籤

```pug
ul
	li A
	li B
	li C

// <div><h2>Hello</h2></div>
div: h2 Hello
```

### 內容

```pug	
p
    | Hello
    | PPPP
    | PPPPPPP
```

### 使用script

```pug
// 有時可能想要寫一個大段文本塊。比如嵌入腳本或者樣式。只需在標籤後面接一個 .即可
script.
    if(true){
        console.log(true)
    }else{
        console.log(false)
    }
```

### 屬性

```pug
a(class='button', href='www.google.com') google
```

#### 多行屬性

```pug
input(
	type="checkbox",
	name="username",
	checked
)
```

#### 長屬性

```pug
input(data-json=`
	{
     	"long":"longer",
        "data":"datas"
	}
`)
```

### 標籤嵌入

```pug
p.
  這是一個很長很長而且還很無聊的段落，還沒有結束，是的，非常非常地長。
  突然出現了一個 #[strong 充滿力量感的單詞]，這確實讓人難以 #[em 忽視]。
```

## 邏輯語法

### JS代碼

#### if / else

```pug
- var user = { description: 'foo bar baz' }
- var authorised = false
#user
  if user.description
    h2.green 描述
    p.description= user.description
  else if authorised
    h2.blue 描述
    p.description.
      用户没有添加描述。
      不写点什么吗……
  else
    h2.red 描述
    p.description 用户没有描述
```

#### 循環 each

```pug
ul
	each val in [1,2,3,4]
		li= val
ul
	each val, index in ['〇', '一', '二']
		li= index + ': ' + val
```

#### 變量

```pug
- var title="Title";

h1=title

h1 #{title}

a(href=title) 另一個鏈接
```

### mixin

```pug
mixin list
	ul
		li oo
		li bb
		li aa

// 使用
+list

mixin pet(name)
  li.pet= name
ul
  +pet('猫')
  +pet('狗')
  +pet('猪')
```

#### 混入的快

```pug
mixin article(title)
  .article
    .article-wrapper
      h1= title
      if block
        block
      else
        p 没有提供任何内容。

+article('Hello world')

+article('Hello world')
  p 这是我
  p 随便写的文章
```

### 文件繼承

Pug 支持使用 `block` 和 `extends` 關鍵字進行模板的繼承。一個稱之為“塊”（block）的代碼塊，可以被子模板覆蓋、替換。這個過程是遞歸的。 

```pug
//- layout.pug
html
  head
    title 我的站點 - #{title}
    block scripts
      script(src='/jquery.js')
  body
    block content
    block foot
      #footer
        p 一些頁腳的內容
```

```pug
//- page-a.pug
extends layout.pug

block scripts
  script(src='/jquery.js')
  script(src='/pets.js')

block content
  h1= title
  - var pets = ['貓', '狗']
  each petName in pets
    include pet.pug
```

同樣，也可以覆蓋一個塊並在其中提供一些新的塊。如下面的例子所展示的那樣，`content` 塊現在暴露出兩個新的塊 `sidebar` 和 `primary` 用來被擴展。當然，它的子模板也可以把整個 `content` 給覆蓋掉。 

```pug
//- sub-layout.pug
extends layout.pug

block content
  .sidebar
    block sidebar
      p 什麼都沒有
  .primary
    block primary
      p 什麼都沒有
```

```pug
//- page-b.pug
extends sub-layout.pug

block content
  .sidebar
    block sidebar
      p 什麼都沒有
  .primary
    block primary
      p 什麼都沒有
```

#### 塊內容的添補 append / prepend

Pug 允許您去替換（默認的行為）、`prepend`（向頭部添加內容），或者 `append`（向尾部添加內容）一個塊。 假設您有一份默認的腳本要放在 `head` 塊中，而且希望將它應用到 *每一個頁面*，那麼您可以這樣做：