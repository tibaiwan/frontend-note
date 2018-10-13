# Canvas 常用知识点整理

[MDN Canvas 教程](https://developer.mozilla.org/zh-CN/docs/Web/API/Canvas_API)  
[Canvas Demos](./Demos.md)

## 简介

`<canvas>` 是 HTML5 新增的元素，可用于通过使用JavaScript中的脚本来绘制图形。  
`<canvas>` 标签只有两个属性 —— width和height。  
当没有设置宽度和高度的时候，canvas会初始化宽度为300像素和高度为150像素。  

一个模板骨架
```html
<html>
  <head>
    <title>Canvas tutorial</title>
    <script type="text/javascript">
      function draw(){
        var canvas = document.getElementById('tutorial');
        if (canvas.getContext){
          var ctx = canvas.getContext('2d');
          // do sth
        }
      }
    </script>
    <style type="text/css">
      canvas { border: 1px solid black; }
    </style>
  </head>
  <body onload="draw();">
    <canvas id="tutorial" width="150" height="150"></canvas>
  </body>
</html>
```

## 栅格

如下图所示，canvas元素默认被网格所覆盖。通常来说网格中的一个单元相当于canvas元素中的一像素。栅格的起点为左上角（坐标为（0,0））。所有元素的位置都相对于原点定位。

![grid](./Images/Canvas_default_grid.png)

## 绘制图形

### 绘制矩形

- `fillRect(x, y, width, height)` 绘制一个填充的矩形
- `strokeRect(x, y, width, height)` 绘制一个矩形的边框
- `rect(x, y, width, height)` 绘制一个左上角坐标为（x,y），宽高为width以及height的矩形。
- `clearRect(x, y, width, height)` 清除指定矩形区域，让清除部分完全透明。

[CodePen](https://codepen.io/tibaiwan/pen/yRgwpe?editors=1010)

### 绘制路径

图形的基本元素是路径。路径是通过不同颜色和宽度的线段或曲线相连形成的不同形状的点的集合。

1、首先，你需要创建路径起始点。  
2、然后你使用画图命令去画出路径。  
3、之后你把路径封闭。  
4、一旦路径生成，你就能通过描边或填充路径区域来渲染图形。  


- `moveTo(x, y)`  将笔触移动到指定的坐标x以及y上
- `lineTo(x, y)` 绘制一条从当前位置到指定x以及y位置的直线。
- `beginPath()` 新建一条路径，生成之后，图形绘制命令被指向到路径上生成路径。
- `closePath()` 闭合路径之后图形绘制命令又重新指向到上下文中。
- `stroke()` 通过线条来绘制图形轮廓。
- `fill()` 通过填充路径的内容区域生成实心的图形。
- `arc(x, y, radius, startAngle, endAngle, anticlockwise)` 画一个以（x,y）为圆心的以radius为半径的圆弧（圆），从startAngle开始到endAngle结束，按照anticlockwise给定的方向（默认为顺时针）来生成。
- `arcTo(x1, y1, x2, y2, radius)` 根据给定的控制点和半径画一段圆弧，再以直线连接两个控制点。

注意：  
1、调用fill()函数时，所有没有闭合的形状都会自动闭合，但是调用stroke()时不会自动闭合。  
2、角度与弧度换算公式：弧度=(Math.PI/180)*角度。  

### 二次贝塞尔曲线及三次贝塞尔曲线

- `quadraticCurveTo(cp1x, cp1y, x, y)`  
绘制二次贝塞尔曲线，cp1x,cp1y为一个控制点，x,y为结束点。
- `bezierCurveTo(cp1x, cp1y, cp2x, cp2y, x, y)`  
绘制三次贝塞尔曲线，cp1x,cp1y为控制点一，cp2x,cp2y为控制点二，x,y为结束点。

### Path2D 对象

Path2D 对象用来缓存或记录绘画命令，这样你将能快速地回顾路径。

- `new Path2D()` 空的Path对象
- `Path2D.addPath(path [, transform])​` 添加了一条路径到当前路径

代码示例：
```js
function draw() {
  var canvas = document.getElementById('canvas');
  if (canvas.getContext){
    var ctx = canvas.getContext('2d');

    var rectangle = new Path2D();
    rectangle.rect(10, 10, 50, 50);

    var circle = new Path2D();
    circle.moveTo(125, 35);
    circle.arc(100, 35, 25, 0, 2 * Math.PI);

    ctx.stroke(rectangle);
    ctx.fill(circle);
  }
}
```

## 添加样式和颜色

### 色彩 Colors

- `fillStyle = color` 设置图形的填充颜色。
- `strokeStyle = color` 设置图形轮廓的颜色。
- `globalAlpha = transparencyValue`  设置图形的透明度（0.0~1.0）。

### 线型 Line styles

- `lineWidth = value` 设置线条宽度
- `lineCap = type` 设置线条末端样式
  - `butt` 线段末端以方形结束。
  - `round` 线段末端以圆形结束。
  - `square` 线段末端以方形结束，但是增加了一个宽度和线段相同，高度是线段厚度一半的矩形区域。
- `lineJoin = type` 设定线条与线条间接合处的样式。
  - `round` 通过填充一个额外的，圆心在相连部分末端的扇形，绘制拐角的形状。
  - `bevel` 在相连部分的末端填充一个额外的以三角形为底的区域， 每个部分都有各自独立的矩形拐角。
  - `miter` 通过延伸相连部分的外边缘，使其相交于一点，形成一个额外的菱形区域。
- `miterLimit = value` 限制当两条线相交时交接处最大长度；
- `setLineDash(segments)` 设置当前虚线样式。
- `lineDashOffset = value` 设置虚线样式的起始偏移量。
- `getLineDash()` 返回一个包含当前虚线样式，长度为非负偶数的数组。

### 渐变 Gradients
- `createLinearGradient(x1, y1, x2, y2)` 线性渐变，渐变的起点 (x1,y1) 与终点 (x2,y2)
- `createRadialGradient(x1, y1, r1, x2, y2, r2)` 径向渐变，r为半径
- `gradient.addColorStop(position, color)` position 参数必须是一个 0.0 与 1.0 之间的数值，表示渐变中颜色所在的相对位置

### 图案样式 Patterns

- `createPattern(image, type)` 创建图案  
`type:` repeat, repeat-x, repeat-y, no-repeat

代码示例：
```js
var img = new Image();
img.src = 'someimage.png';
// 确认 image 对象已经装载完毕，否则图案无效
img.onload = function(){
  var ptrn = ctx.createPattern(img,'repeat');
  ctx.fillStyle = ptrn;
  ctx.fillRect(0,0,150,150);
}
```

### 阴影 Shadows

- `shadowOffsetX = float` 设定阴影在 X 轴的延伸距离
- `shadowOffsetY = float` 设定阴影在 Y 轴的延伸距离
- `shadowBlur = float` 设定阴影的模糊程度
- `shadowColor = color` 设定阴影颜色效果，默认是全透明的黑色

## 绘制文本

### 绘制方法

- `fillText(text, x, y [, maxWidth])` 在指定的(x,y)位置填充指定的文本，绘制的最大宽度是可选的.
- `strokeText(text, x, y [, maxWidth])` 在指定的(x,y)位置绘制文本边框，绘制的最大宽度是可选的.

### 有样式的文本

`font = value` 文本的样式，默认的字体是 10px sans-serif。  
`textAlign = value` 文本对齐选项. 可选的值包括：start, end, left, right or center。  
`textBaseline = value` 基线对齐选项. 可选的值包括：top, hanging, middle, alphabetic, ideographic, bottom。  
`direction = value` 文本方向。可能的值包括：ltr, rtl, inherit。

### 预测量文本宽度

- `measureText()` 将返回一个 TextMetrics对象的宽度、所在像素。

## 使用图片

### 绘制图片

- `drawImage(image, x, y)` 使用 drawImage 方法将图片渲染到 canvas 里。  
- `drawImage(image, x, y, width, height)` 缩放 Scaling，width 和 height，用来控制向canvas画入时应该缩放的大小。  
- `drawImage(image, sx, sy, sWidth, sHeight, dx, dy, dWidth, dHeight)` 切片 Slicing，见图解。

![解析图](./Images/Canvas_drawimage.jpg)

## 变形

### 状态的保存和恢复 Saving and restoring state

- `save()` 保存canvas状态到栈中
- `restore()` 恢复上一个canvas状态

### 移动 Translating

- `translate(x, y)` x 是左右偏移量，y 是上下偏移量。

### 旋转 Rotating

- `rotate(angle)` 旋转的角度(angle)，它是顺时针方向的，以弧度为单位的值。

### 缩放 Scaling

- `scale(x, y)` x,y 分别是横轴和纵轴的缩放因子，它们都必须是正值。

### 变形 Transforms

- `transform(m11, m12, m21, m22, dx, dy)` 变形矩阵

## 合成与裁剪

### 组合 Compositing

- `globalCompositeOperation = type` 设定了在画新图形时采用的遮盖策略。

[组合 Compositing](https://developer.mozilla.org/zh-CN/docs/Web/API/Canvas_API/Tutorial/Compositing)

## 基本动画

### 基本步骤

1、清空 canvas  
2、保存 canvas 状态  
3、绘制动画图形（animated shapes）  
4、恢复 canvas 状态  

### 操控动画

- setInterval(function, delay)
- setTimeout(function, delay)
- requestAnimationFrame(callback)

## 像素操作

### 保存图片

- canvas.toDataURL('image/png')
- canvas.toDataURL('image/jpeg', quality)
  - quality: 可以有选择地提供从0到1的品质量，1表示最好品质，0基本不被辨析但有比较小的文件大小。
- canvas.toBlob(callback, type, encoderOptions)

## 点击区域

- boolean ctx.isPointInPath(x, y)
- boolean ctx.isPointInPath(x, y, fillRule)
  - fillRule: 用来决定点在路径内还是在路径外的算法。  
  允许的值：  
    - "nonzero": 非零环绕规则 ，默认的规则。  
    - "evenodd": 奇偶环绕原则 。  
- CanvasRenderingContext2D.addHitRegion() (实验室功能)
- CanvasRenderingContext2D.removeHitRegion()
- CanvasRenderingContext2D.clearHitRegions()

示例：
```js
var canvas = document.getElementById("canvas");
var ctx = canvas.getContext("2d");

ctx.beginPath();
ctx.arc(70, 80, 10, 0, 2 * Math.PI, false);
ctx.fill();
ctx.addHitRegion({id: "circle"});

canvas.addEventListener("mousemove", function(event){
  if(event.region) {
    alert("hit region: " + event.region);
  }
});
```

## canvas 的优化

- 在离屏canvas上预渲染相似的图形或重复的对象
- 避免浮点数的坐标点，用整数取而代之
- 不要在用drawImage时缩放图像
- 使用多层画布去画一个复杂的场景
- 用CSS设置大的背景图
- 用CSS transforms特性缩放画布
- 关闭透明度（var ctx = canvas.getContext('2d', { alpha: false });）

更多贴士：

- 将画布的函数调用集合到一起（例如，画一条折线，而不要画多条分开的直线）
- 避免不必要的画布状态改变
- 渲染画布中的不同点，而非整个新状态
- 尽可能避免 shadowBlur特性
- 尽可能避免text rendering
- 使用不同的办法去清除画布(clearRect() vs. fillRect() vs. 调整canvas大小)
- 有动画，请使用window.requestAnimationFrame() 而非window.setInterval()
- 请谨慎使用大型物理库

