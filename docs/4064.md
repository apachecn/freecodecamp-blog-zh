# 李氏算法解释:走迷宫和寻找最短路径

> 原文：<https://www.freecodecamp.org/news/lee-algorithm-maze-explained/>

## 什么是李氏算法？

Lee 算法是迷宫路由问题的一种可能的解决方案。如果存在最优解，它总是给出最优解，但是速度很慢，并且对于密集布局需要大的内存。

### 了解它是如何工作的

该算法是基于`breadth-first`的算法，使用`queues`存储步骤。它通常使用以下步骤:

1.  选择一个起点，并将其添加到队列中。
2.  将有效的相邻单元添加到队列中。
3.  从队列中移除当前位置，并继续处理下一个元素。
4.  重复步骤 2 和 3，直到队列为空。

### 履行

C++已经在`<queue>`库中实现了队列，但是如果你使用其他的东西，欢迎你实现你自己的队列版本。

### C++代码:

```
int dl[] = {-1, 0, 1, 0}; // these arrays will help you travel in the 4 directions more easily
int dc[] = {0, 1, 0, -1};

queue<int> X, Y; // the queues used to get the positions in the matrix

X.push(start_x); // initialize the queues with the start position
Y.push(start_y);

void lee()
{
  int x, y, xx, yy;
  while(!X.empty()) // while there are still positions in the queue
  {
    x = X.front(); // set the current position
    y = Y.front();
    for(int i = 0; i < 4; i++)
    {
      xx = x + dl[i]; // travel in an adiacent cell from the current position
      yy = y + dc[i];
      if('position is valid') //here you should insert whatever conditions should apply for your position (xx, yy)
      {
          X.push(xx); // add the position to the queue
          Y.push(yy);
          mat[xx][yy] = -1; // you usually mark that you have been to this position in the matrix
      }
    }

    X.pop(); // eliminate the first position, as you have no more use for it
    Y.pop();    
  }
} 
```