#js 
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

