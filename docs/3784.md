# SVG 矩形和其他 SVG 形状

> 原文：<https://www.freecodecamp.org/news/svg-rectangle-and-other-svg-shapes/>

使用 SVG 绘图可以创建多种形状。SVG 绘图可以使用和组合七种形状:路径、矩形、圆形、椭圆形、直线、折线和多边形。

### **路径**

`path`元素是最常见的，通常由用于导出 SVG 代码的程序生成。

```
 <path d="M2 1 h1 v1 h1 v1 h-1 v1 h-1 v-1 h-1 v-1 h1 z" />
```

上面的例子`path`在 SVG 绘图中使用时会生成一个“加号”符号(+)。SVG `path`元素不是手工构建的，而是通过可以操纵矢量图形的设计程序生成的，比如 Illustrator 或者 Inkscape。

### **矩形**

rectangle 元素`rect`在屏幕上绘制一个矩形，它接受六个属性。

```
 <rect x="0" y="0" width="100" height="50" rx="10" ry="10" />
```

`x`和`y`指定矩形左上角的位置，`width`和`height`指定矩形的大小。`rx`和`ry`指定矩形角的半径，类似于 CSS 的 border-radius 属性。

### **圆圈**

circle 元素`circle`接受三个属性。

```
 <circle cx="100" cy="100" r="50" />
```

`cx`和`cy`指定圆心的位置，`r`指定圆的半径(大小)。

### **椭圆**

椭圆元素`ellipse`类似于`circle`元素，除了半径被分成两个属性。

```
 <ellipse cx="100" cy="100" rx="50" ry="20" />
```

再次，`cx`和`cy`指定椭圆中心的位置，现在`rx`和`ry`分别指定椭圆的水平和垂直半径。较大的`rx`将给出一个“胖”椭圆，较大的`ry`将给出一个瘦椭圆。如果`rx`和`ry`相等，就会形成一个圆。

### **线**

`line`元素很简单，接受四个属性。

```
 <line x1="0" x2="100" y1="50" y2="70" />
```

属性`x1`和`y1`指定直线的第一个点，属性`x2`和`y2`指定直线的第二个点。

### **折线**

一个`polyline`是一系列连接的直线，分配在单个属性中。

```
 <polyline points="0 100, 50 70, 60 40, 20 0" />
```

属性指定了一个点列表，每个点用逗号分隔。

### **多边形**

`polygon`元素也是一系列相连的直线，但在这种情况下，最后一条线会自动重新连接到初始点。

```
 <polygon points="0 100, 50 70, 60 40, 20 0" />
```

该示例将绘制与上面的`polyline`相同的形状，但它将绘制一条额外的线来“封闭”这一系列线。

## **更多信息**

MDN 文档: [MDN](https://developer.mozilla.org/en-US/docs/Web/SVG/Tutorial/Basic_Shapes)