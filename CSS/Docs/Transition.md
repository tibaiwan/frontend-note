# Transition

* [属性](#属性)
* [示例](#示例)
* [检测过渡是否完成](#检测过渡是否完成)

[可动画属性列表](https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_animated_properties)

CSS transitions 提供了一种在更改CSS属性时控制动画速度的方法。 其可以让属性变化成为一个持续一段时间的过程，而不是立即生效的。

## 属性

> transition

```css
div {
  transition: <property> <duration> <timing-function> <delay>;
}
```

> transition-property

指定哪个或哪些 CSS 属性用于过渡。只有指定的属性才会在过渡中发生动画，其它属性仍如通常那样瞬间变化。

> transition-duration

指定过渡的时长。或者为所有属性指定一个值，或者指定多个值，为每个属性指定不同的时长。

> transition-timing-function

指定一个函数，定义属性值怎么变化。多数 timing functions 由四点定义一个 bezier 曲线。

此属性有以下可选值：
- ease - a slow start, then fast, then end slowly (this is default)
- linear - the same speed from start to end
- ease-in - a slow start
- ease-out - a slow end
- ease-in-out - a slow start and end
- cubic-bezier(n,n,n,n) - lets you define your own values in a cubic-bezier function

> transition-delay

指定延迟，即属性开始变化时与过渡开始发生时之间的时长。

## 示例

- 简单示例
```css
div {
  transition: width 2s linear 1s;
}
div:hover {
  width: 300px;
}
```

- 宽高过渡
```css
div {
  width: 100px;
  height: 100px;
  background: red;
  transition: width 2s, height 4s;
}
div:hover {
  width: 300px;
  height: 300px;
}
```

- Transition + Transform
```css
div {
  width: 100px;
  height: 100px;
  background: red;
  transition: width 2s, height 2s, transform 2s;
}
div:hover {
  width: 300px;
  height: 300px;
  transform: rotate(180deg);
}
```

## 检测过渡是否完成

> transitionend 事件

```js
el.addEventListener("transitionend", updateTransition, true);
```

transitionend 事件提供两个属性:

- propertyName

字符串，指示已完成过渡的属性。

- elapsedTime

浮点数，指示当触发这个事件时过渡已运行的时间（秒）。这个值不受 transition-delay 影响。
