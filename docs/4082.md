# 举例说明边界填充算法和像素填充正方形

> 原文：<https://www.freecodecamp.org/news/boundary-fill-algorithm-pixel-filling-squares/>

## 什么是边界填充算法？

边界填充是计算机图形中经常使用的算法，用于在所有边都具有相同边界颜色的闭合多边形内填充所需的颜色。

该算法最接近的实现是基于堆栈的递归函数。

### 工作示例:

这个问题非常简单，通常遵循以下步骤:

1.  取起点的位置和边界颜色。
2.  决定你是想去 4 个方向(N，S，W，E)还是 8 个方向(N，S，W，E，NW，NE，SW，SE)。
3.  选择一种填充颜色。
4.  往那个方向走。
5.  如果您停留的像素不是填充颜色或边界颜色，请用填充颜色替换它。
6.  重复第 4 步和第 5 步，直到你到达边界内的所有地方。

### 某些限制:

1.  多边形所有边的边界颜色应该相同。
2.  起点应该在多边形内。

### 代码片段:

```
void boundary_fill(int pos_x, int pos_y, int boundary_color, int fill_color)
{  
current_color= getpixel(pos_x,pos_y);  //get the color of the current pixel position
if( current_color!= boundary_color || currrent_color != fill_color) // if pixel not already filled or part of the boundary then
{    
 putpixel(pos_x,pos_y,fill_color);  //change the color for this pixel to the desired fill_color
 boundary_fill(pos_x + 1, pos_y,boundary_color,fill_color);  // perform same function for the east pixel
 boundary_fill(pos_x - 1, pos_y,boundary_color,fill_color);  // perform same function for the west pixel
 boundary_fill(pos_x, pos_y + 1,boundary_color,fill_color);  // perform same function for the north pixel
 boundary_fill(pos_x, pos_y - 1,boundary_color,fill_color);  // perform same function for the south pixel
}
} 
```

从给定的代码中，您可以看到，对于您到达的任何像素，您首先检查它是否可以更改为 fill_color，然后对它的邻居也这样做，直到检查完边界内的所有像素。

这里有一个视频教程，将在大约 10 分钟内带您详细了解这些内容:

[https://www.youtube.com/embed/au2CslPa4xU?feature=oembed](https://www.youtube.com/embed/au2CslPa4xU?feature=oembed)