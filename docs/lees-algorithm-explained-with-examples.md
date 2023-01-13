# 李氏算法举例说明

> 原文：<https://www.freecodecamp.org/news/lees-algorithm-explained-with-examples/>

Lee 算法是迷宫路由问题的一种可能的解决方案。如果存在最优解，它总是给出最优解，但是速度很慢，并且对于密集布局需要大的内存。

### **了解其工作原理**

该算法是基于`breadth-first`的算法，使用`queues`存储步骤。它通常使用以下步骤:

1.  选择一个起点，并将其添加到队列中。
2.  将有效的相邻单元添加到队列中。
3.  从队列中移除当前位置，并继续处理下一个元素。
4.  重复步骤 2 和 3，直到队列为空。

需要理解的一个关键概念是，`breadth-first`搜索范围广，而`depth-first`搜索深度深。

以迷宫求解算法为例，`depth-first`方法将逐一尝试每一条可能的路径，直到它到达死胡同或终点并返回结果。然而，它返回的路径可能不是最有效的，而只是算法能够找到的到达终点的第一条完整路径。

取而代之的是，搜索将到达起点附近的每个空地，然后寻找其他可能的空地。它会一直这样做，一层一层地出去，一前一后地尝试每一条可能的路径，直到找到终点。因为您同时尝试了每条路径，所以可以确定从开始到结束的第一条完整路径也是最短的。

### **实施**

C++已经在`<queue>`库中实现了队列，但是如果你使用其他的东西，欢迎你实现你自己的队列版本。

C++代码:

```
int dl[] = {-1, 0, 1, 0}; // these arrays will help you travel in the 4 directions more easily
int dc[] = {0, 1, 0, -1};

queue<int> X, Y; // the queues used to get the positions in the matrix

X.push(start_x); //initialize the queues with the start position
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