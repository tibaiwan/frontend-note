# Transform

* translate 平移
* rotate 旋转
* scale 比例，缩放
* skew 歪斜
* matrix 矩阵
* perspective 远景，透视

## translate 平移

- translate(x,y)
- translate3d(x,y,z)
- translateX(x), translateY(y), translateY(z)

示例：
```css
transform: translate(50px, 50px);
transform: translateX(50px, 50px);
```

## rotate 旋转

- rotate(angle)	
- rotate3d(x,y,z,angle)
- rotateX(angle), rotateY(angle), rotateZ(angle)

示例：
```css
transform: rotate(60deg);
transform: rotateX(60deg);
```

## scale 比例，缩放

- scale(x,y)	
- scale3d(x,y,z)
- scaleX(x), scaleY(y), scaleZ(z)

示例：
```
transform: scale(0.5, 2);
transform: scaleX(3);
```

## skew 歪斜

- skew(x-angle,y-angle)	
- skewX(angle)

示例：
```css
transform: skew(60deg, 60deg);
transform: skewX(40deg)
```

## matrix 矩阵

- matrix(n,n,n,n,n,n)

示例：
```css
transform: matrix(0.866,0.5,-0.5,0.866,0,0);
```

## perspective 远景，透视



