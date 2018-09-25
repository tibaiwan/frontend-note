# Animation

CSS animations 使得可以将从一个CSS样式配置转换到另一个CSS样式配置。动画包括两个部分:描述动画的样式规则和用于指定动画开始、结束以及中间点样式的关键帧。

## 属性

> animation-delay

设置延时，即从元素加载完成之后到动画序列开始执行的这段时间。

- `<time>`: 从动画样式应用到元素上到元素开始执行动画的时间差。该值可用单位为秒(s)和毫秒(ms)。

> animation-direction

设置动画在每次运行完后是反向运行还是重新回到开始位置重复运行。
- normal: 每个动画循环结束，动画重置到起点重新开始，这是默认属性。
- alternate: 动画交替反向运行，反向运行时，动画按步后退，同时，带时间功能的函数也反向。
- reverse: 反向运行动画，每周期结束动画由尾到头运行。
- alternate-reverse: 反向交替， 反向开始交替

> animation-duration

设置动画一个周期的时长。

- `<time>`: 一个动画周期的时长，单位为秒(s)或者毫秒(ms)，无单位值无效。

> animation-iteration-count

设置动画重复次数， 可以指定infinite无限次重复动画
- infinite: 无限循环播放动画.
- `<number>`: 动画播放的次数 不可为负值. 可以用小数定义循环。

> animation-name

指定由@keyframes描述的关键帧名称。

> animation-play-state

允许暂停和恢复动画。
- running: 当前动画正在运行。
- paused: 当前动画已被停止。

> animation-timing-function

设置动画速度， 即通过建立加速度曲线，设置动画在关键帧之间是如何变化。

常用示例
```css
animation-timing-function: ease;
animation-timing-function: ease-in;
animation-timing-function: ease-out;
animation-timing-function: ease-in-out;
animation-timing-function: linear;
animation-timing-function: step-start;
animation-timing-function: step-end;
animation-timing-function: cubic-bezier(0.1, 0.7, 1.0, 0.1);
```

> animation-fill-mode

指定动画执行前后如何为目标元素应用样式。

## 使用keyframes定义动画序列

通过使用@keyframes建立两个或两个以上关键帧来实现。每一个关键帧都描述了动画元素在给定的时间点上应该如何渲染。

0%表示动画的第一时刻，100%表示动画的最终时刻。因为这两个时间点十分重要，所以还有特殊的别名：from和to。

## 示例

简单示例
```css
/* The animation code */
@keyframes example {
  from {background-color: red;}
  to {background-color: yellow;}
}
/* The element to apply the animation to */
div {
  width: 100px;
  height: 100px;
  background-color: red;
  animation-name: example;
  animation-duration: 4s;
}
```
复杂动画
```css
div {
  width: 100px;
  height: 100px;
  background-color: red;
  position: relative;
  /* Standard syntax */
  animation-name: example;
  animation-duration: 5s;
  animation-timing-function: linear;
  animation-delay: 2s;
  animation-iteration-count: infinite;
  animation-direction: alternate;
}
/* Standard syntax */
@keyframes example {
  0%   {background-color:red; left:0px; top:0px;}
  25%  {background-color:yellow; left:200px; top:0px;}
  50%  {background-color:blue; left:200px; top:200px;}
  75%  {background-color:green; left:0px; top:200px;}
  100% {background-color:red; left:0px; top:0px;}
}
```

## 动画事件

> 添加动画事件监听器

```js
var e = document.getElementById("watchme");
e.addEventListener("animationstart", listener, false);
e.addEventListener("animationend", listener, false);
e.addEventListener("animationiteration", listener, false);
// 最后设置动画元素的class为slidein，启动动画
e.className = "slidein";
```

> 接收事件

```js
function listener(e) {
  switch(e.type) {
    case "animationstart":
      // do sth
      break;
    case "animationend":
      // do sth
      break;
    case "animationiteration":
      // do sth
      break;
  }
}
```
