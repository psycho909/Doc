## TweenMax.to(el)

> el可以是所有物件

```js
 var demo={
                int:20
            }
            $('.box').html(demo.int)
            TweenMax.to(demo,8,{
                int:0,
                onUpdate:function(){
                    $('.box').html(Math.ceil(demo.int))
                },
                onComplete:function(){
                    $('.box').html("Done")
                },
                ease:Power4.easeOut
            })
```



## staggerTo

> 可依序調用el去做動畫

```js
TweenMax.staggerTo(el,0.5,{
            transform:"translate(0px)",
            opacity:1
        },0.2)
```



## staggerFrom

> 透過staggerFrom設置動畫的初始狀態去過場到CSS的設置狀態

```js
TweenMax.staggerFrom(el,1,{
                    scaleX:0.8,
                    scaleY:0.8,
                    opacity:0
                },0.3)
```

## killAll()

> 刪掉當前場景所有TweenMax動畫

## TweenMax.set()

> 設定動畫初始直，回歸動畫因該有的樣子

```js
TweenMax.set(el,1,{
                    scaleX:1,
                    scaleY:1,
                    opacity:1
                }
            )
```



## TimelineLite()

```js
var tl=new TimelineLite();

        tl.to(".box1",1,{opacity:1})
        tl.to(".box2",1,{opacity:1})
        tl.to(".box3",1,{opacity:1})
        tl.to(".box4",1,{opacity:1})
        tl.to(".box5",1,{opacity:1})
        tl.to(".box6",1,{opacity:1})
        tl.to(".box7",1,{opacity:1})
        tl.to(".box8",1,{opacity:1})
        tl.to(".box9",1,{opacity:1})
        tl.to(".box10",1,{opacity:1})
        tl.pause()
        $(window).scroll(function(){
            var scrollTop=$(window).scrollTop()
            var docHeight=$(document).height()
            var winHeight=$(window).height();
            var progress=scrollTop/(docHeight-winHeight)
            
            tl.progress(progress)
            console.log(progress,scrollTop,docHeight,winHeight)
        })
```

## TextPlugin

```html
https://cdnjs.cloudflare.com/ajax/libs/gsap/2.0.2/plugins/TextPlugin.min.js
```

```js
TweenMax.to('#title',5,{text:"Hello World"})
        TweenMax.to('#txt',5,{text:"Hello World",delay:2})
```

