#js #canvas
# Canvas
> 我们想要操作 canvas，首先需要获取这个 canvas 对象
```html
<canvas id="canvas" width="250" height="50"></canvas>
```
```javascript
// 获取 canvas 对象
let canvas = document.getElementById('canvas'),  
// 获取 cavans 绘图环境对象，参数为 2d
context = canvas.getContext('2d');
```
#### **绘制基本图形**
* **矩形**

`context.rect($x, $y, $width, $height)`
>  参数： $x $y：分别对应矩形左上角在 canvas 画布中的坐标； $width $height：就是矩形的宽高啦；
* **圆形**

`context.arc($x, $y, $radius, $start_radian, $end_radian [, $clockwise])`
> 参数： $x $y：代表圆的圆心在 canvas 画布中的坐标点； $radius：圆的半径，半径，是半径，不是直径！ $start_radian $end_radian：起始角和终止角，他们接收的值只能是弧度。
* **当前路径**

`context.beginPath()`
> 我们在创建一个集合图形之前，都应当先调用该方法，该方法会清除上一次绘制时留下的路径
* **清除画布内容**

`context.clearRect($x, $y, $width, $height)`
> 该方法，擦除规定矩形中的所有内容；参数同绘制矩形是一样的，只不过这是清除！

* **canvas 的状态**

> canvas 的状态包括了：
1. 当前坐标变换信息，`transform` 的东西，不用担心，本章并不涉及；
2. 剪辑区域，这是本章的核心，`context.clip()` 后面会详细讲到；
3. 所有绘图环境（context）属性，也就刚刚提到的，`fillStyle[规定填充的颜色]` `strokeStyle[规定描边的颜色]`，这些状态决定的 canvas 中绘制的图像的展示效果，他们是全局的，也就是说一旦规定了fillStyle为红色，那么除非在 javascript 后面重置这个属性，否则你将来绘制出来的几何图形永远都是红色的。

* **canvas 的状态保存与恢复**

> 前面已经提到过，canvas 的状态是什么。 而这里引出的是绘图环境中的两个很重要的方法，`save()` 和 `restore()`。

我们知道 javascript 是逐条同步执行的，那么要实现上面的需求，我们需要先定义绘图环境的填充颜色为红色，绘制了第一个红色矩形后，再定义绘图环境的填充颜色为蓝色，绘制，再定义绘图环境的填充颜色为红色，就像这样：
```js
context.fillStyle = 'red';
drawRect(0, 0, 100, 100);   // -> red

context.fillStyle = 'blue';
drawRect(110, 0, 100, 100); // -> blue

context.fillStyle = 'red';
drawRect(220, 0, 100, 100); // -> red
```
此时可以使用save() 和 restore()簡化一切
> `context.save()`：能将执行该方法之前的所有 canvas 状态 保存下来； `context.restore()` ：将 save 方法保存的 canvas 状态 释放出来；
```js
context.fillStyle = 'red';
drawRect(0, 0, 100, 100);    // - red 

context.save();

context.fillStyle = 'blue';
drawRect(110, 0, 100, 100);  // -> blue

context.restore();
drawRect(220, 0, 100, 100);  // -> red
```
save 方法保存的是 `context.fillStyle = 'red'` 这个状态，当设置这个状态为蓝色时，context 绘图环境的 fillStyle 属性变成了蓝色，当执行 restore 方法时，又被恢复成了红色。

* **剪辑区域 clip()**
context 绘图环境对象提供了一个 `context.clip()` 剪辑区域方法。

该方法会将作为一个剪辑区域，使该区域以外的图像不可见。

并且在剪辑区域中，所有填充，清除等操作不会影响剪辑区域以外的内容。

所以我们可以创建一个剪辑区域，在这个剪辑区域中执行清除操作，就可以擦除 canvas 画布中指定的内容。
