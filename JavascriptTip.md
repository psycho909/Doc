## 循環創建createElement

```js
var fragment = document.createDocumentFragment();
data.forEach(function(v,i){
    var img=document.createElement('img');
    var a=document.createElement('a');

    img.src="images/"+v.image+".jpg";
    a.setAttribute('href',"javascript:;");
    a.setAttribute('target',"_blank:;");
    a.setAttribute('class',"news-slick__item");
    a.appendChild(img);
    fragment.appendChild(a);
})
document.getElementById("NEWSSlickWarp").appendChild(fragment)
```

```html
<div id="NEWSSlickWarp" class="news-slick">
    <a href="javascript:;" target="_blank" class="news-slick__item">
        <img src="images/news01.jpg" alt="">
    </a>
    <a href="javascript:;" target="_blank" class="news-slick__item">
        <img src="images/news01.jpg" alt="">
    </a>
    <a href="javascript:;" target="_blank" class="news-slick__item">
        <img src="images/news01.jpg" alt="">
    </a>
    <a href="javascript:;" target="_blank" class="news-slick__item">
        <img src="images/news01.jpg" alt="">
    </a>
    <a href="javascript:;" target="_blank" class="news-slick__item">
        <img src="images/news01.jpg" alt="">
    </a>
</div>
```

## 前端 input 輸入框可能被攻擊的幾種方式及防範

```js
var escapeHTML=str=>str.replace(/[&<>'"]/g,tag=>({
    	'&': '&amp;',
        '<': '&lt;',
        '>': '&gt;',
        "'": '&#39;',
        '"': '&quot;'
    }[tag]||tag)
```

## Cookie

### 增

```js
function setCookie(key, value, expiredays) {
  var exdate = new Date();
  exdate.setDate(exdate.getDate() + expiredays);
  document.cookie =
    key +
    "=" +
    escape(value) +
    (expiredays == null ? "" : ";expires=" + exdate.toGMTString());
}
```

### 刪

```js
function delCookie(name) {
  var exp = new Date();
  exp.setTime(exp.getTime() - 1);
  var cval = getCookie(name);
  if (cval != null) {
    document.cookie = name + "=" + cval + ";expires=" + exp.toGMTString();
  }
}
```

### 查

```js
function getCookie(name) {
  var arr,
    reg = new RegExp("(^| )" + name + "=([^;]*)(;|$)");
  if ((arr = document.cookie.match(reg))) {
    return arr[2];
  } else {
    return null;
  }
}
```

