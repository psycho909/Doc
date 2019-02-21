# SCSS
## @if
```scss
$boolean:true;

@if $boolean {
    .ball{
        color:red;
    }
}
@else {
    .ball{
        color:blue;
    }
}

```
## @for $i from <start> through <end>
```scss
@for $i from 1 through 10{
    .ball-#{i}{
        color:red;
    }
}
```
## mixins
```scss
@mixin Mixins名($valueName:value){
    /*樣式規則*/
}
@include Mixins名();
@mixin error($borderWidth:2px){
    border:$borderWidth solid #000;
}
.ball{
    @include error(3px);
}
```
## mixins + @content
```scss
@mixin bg($text-color,$bg-color){
    background:$bg-color;
    color:$text-color;
    @content;
}
.box{
    @include bg(#fff,#000){
        border:1px solid #000;
    }
}
.box {
    background: #000;
    color: #fff;
    border: 1px solid #000;
}
```
## mixin + @content 用於RWD
```scss
@mixin breakpoint($point){
    @if $point == pc{
        @media only screen and (max-width:1024px){
            @content;
        }
    }
    @else if $point == pad{
        @media only screen and (max-width:768px){
            @content;
        }
    }
}
.one{
    width:600px;
    @include breakpoint(pc){
        width:300px;
        height:50px;
    }
    @include breakpoint(pad){
        width:100%;
    }
}
.one {
    width: 600px;
}
@media only screen and (max-width: 1024px) {
    .one {
        width: 300px;
        height: 50px
    }
}
@media only screen and (max-width: 768px) {
    .one {
        width: 100%;
    }
}
```
## extend
```scss
選擇器{
    @extend 定義的類;
}

.block{
    color:red;
}
.ball{
    @extend .block;
}
```



# Array

```scss
$colors:#fb3569,#ffe26f,#a1dd70,#35477d;

background:nth($colors,1);
// background:#fb3569;
```



## Maps

```scss
$map: (
    key1: value1, 
    key2: value2, 
    key3: value3
);
```
```scss
$colors: (
    lt-grey: #efefef,
    grey: #424242,
    pink: #ff6599,
);

$backgrounds: (
    lt-grey: #ebe4e8
);

/* How to use */
body {
    background: map-get($backgrounds, lt-grey);
    color: map-get($colors, pink);
}
```
## 迴圈 跑 maps
```scss
$menus: (
    menu1: #94a437,
    menu2: #cb0a3a,
    menu3: #5d5d5d,
    menu4: #0095a1,
    menu5: #5273d1
);
@each $menu-item,$menu-color in $menus{
    .#{$menu-item}{
        background:$menu-color;
    }
    .#{$menu-item}:hover{
        background:$menu-color;
    }
}
```
## @function
```scss
p{
    color:darken($f00,70%);
    height:rendom(100);
}
```