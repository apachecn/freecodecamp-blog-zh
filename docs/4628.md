# 清理码头工人

> 原文：<https://www.freecodecamp.org/news/cleaning-up-docker/>

随着 Docker 在开发中运行时间的推移，我们往往会积累大量未使用的图像。有时是为了测试、研究或尝试有趣的新东西。在容器中运行新软件总是很酷，为我们这些对不断学习新技术感兴趣的人点亮了新的可能性。缺点是大量珍贵的固态硬盘内存被很少使用或未使用的图像占用，更糟糕的是我们几乎没有注意到。但是 Docker 公司的人做了一项伟大的工作，记录了 Docker 的所有事情。

向`system`命令问好，它是 docker 管理命令的一部分，非常棒。`system`命令提供了从磁盘使用到系统范围的信息，是不是很酷。

### **磁盘使用使用** `**df**` **命令:**

```
$ docker system df
```

返回类似这样的内容，

```
TYPE              TOTAL     ACTIVE     SIZE         RECLAIMABLE
Images            35        6          8.332GB      7.364GB (88%)
Containers        12        12         417.6MB      0B (0%)
Local Volumes     67        2          2.828GB      2.828GB (100%)
Build Cache                            0B           0B
```

注意`Reclaimable`这是您可以恢复的大小，它是通过从总图像大小中减去活动图像的大小来计算的。

* * *

****实时事件使用**** `****events****` ****命令:****

```
$ docker system events
```

根据 Docker 对象类型，从服务器返回实时事件列表。

格式化输出

```
--format 'Type={{.Type}}  Status={{.Status}}  ID={{.ID}}'
```

或者简单地将输出格式化为 JSON

```
$ docker system events --format '{{json .}}'
```

* * *

****全系统信息使用**** `****info****` ****命令:****

另一个获取所有系统相关信息的很酷的命令是`info`命令。你会惊讶地发现你能获得的信息量。

```
$ docker system info
```

* * *

****使用**** `****prune****` ****命令删除未使用的数据:****

现在我们已经有了所有需要的信息，是时候清理了，但是要小心不要在半睡眠状态下使用这个命令。

```
$ docker system prune
WARNING! This will remove:        
	- all stopped containers        
        - all networks not used by at least one container        
        - all dangling images        
        - all build cache
Are you sure you want to continue? [y/N]
```

此外，我们可以删除我们想要的，使用以下任何命令，大饱眼福女士们先生们。

```
$ docker system prune -a --volumes
$ docker image prune
$ docker container prune
$ docker volume prune
$ docker network prune
```

上面所有的命令都会提示确认，所以在发出这些命令之前，用冷水洗脸或者喝一杯浓咖啡；).