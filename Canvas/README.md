# Canvas 常用知识点整理

[MDN Canvas](https://developer.mozilla.org/zh-CN/docs/Web/API/Canvas_API)

## 简介

`<canvas>` 是 HTML5 新增的元素，可用于通过使用JavaScript中的脚本来绘制图形。  
`<canvas>` 标签只有两个属性—— width和height。  
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






