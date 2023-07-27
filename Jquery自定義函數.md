#js #jquery
```js
(function ($) {
    var $window = $(window);

    $.fn.isVisible = function () {
        var $this = $(this),
            Left = $this.offset().left,
            visibleWidth = $window.width();

        return Left < visibleWidth;
    };
})(jQuery);
```

```js
$("#btn").on("click",function(){
    console.log($(this).isVisible()) // true
})
```

