# 使用 Python(和其他很酷的技巧)检查 CPU 的温度

> 原文：<https://www.freecodecamp.org/news/using-psutil-in-python-8623d9fac8dd/>

作者奥莉萝莎

# 使用 Python(和其他很酷的技巧)检查 CPU 的温度

![rEV20g78X3XQH2m7m4rtBX5VkdwfzhnvUXg9](img/38e8aa71cfdbcecf83899c8482295332.png)

Python 的 [psutil 模块](https://psutil.readthedocs.io/en/latest/)提供了与所有 PC 资源和进程的接口。

无论我们是想获得特定资源的数据，还是想根据资源的状态来管理资源，这个模块都非常有用。

在本文中，我将向您展示这个模块的主要特性以及如何使用它们。

### **获取电脑资源信息**

让我们看看如何获得一些关于我们电脑当前系统状态的信息。

我们可以获得一些关于 CPU 自启动以来的信息，包括它进行了多少次[系统调用](https://www.geeksforgeeks.org/operating-system-introduction-system-call/)和[上下文切换](http://www.linfo.org/context_switch.html):

```
In [1]: psutil.cpu_stats()Out[1]: scpustats(ctx_switches=437905181,interrupts=2222556355L,soft_interrupts=0,syscalls=109468308)
```

我们可以获得一些关于磁盘和内存状态的信息:

```
In [1]: psutil.disk_usage("c:")Out[1]: sdiskusage(total=127950385152L,                   used=116934914048L,                   free=11015471104L,                   percent=91.4)
```

```
In [2]: psutil.virtual_memory()Out[2]: svmem(total=8488030208L,              available=3647520768L,              percent=57.0,              used=4840509440L,              free=3647520768L)
```

我们甚至可以获得一些物理信息，如电池寿命还剩多少秒，或者当前的 CPU 温度:

```
In [1]: psutil.sensors_battery()Out[1]: sbattery(percent=77, secsleft=18305, power_plugged=False)
```

```
In [2]: psutil.sensors_temperatures() # In CelsiusOut[2]: {'ACPI\\ThermalZone\\THM0_0':         [shwtemp(label='',          current=49.05000000000001,          high=127.05000000000001,          critical=127.05000000000001)]}
```

### **获取进程信息**

这个模块为我们提供的最强大的特性之一是“Process”类。我们可以访问每个进程的资源和统计数据，并做出相应的响应。

(有些进程需要一些管理员或系统权限，因此在尝试访问它们的实例后，它将失败，并显示“AccessDenied”错误。)

让我们来看看这个特性。

首先，我们通过给出所需的进程 ID 来创建一个实例:

```
In [1]: p = psutil.Process(9800)
```

然后，我们可以访问流程的所有信息和统计数据:

```
In [1]: p.exe()Out[1]: 'C:\\Windows\\System32\\dllhost.exe'
```

```
In [2]: p.cpu_percent()Out[2]: 0.0
```

```
In [3]: p.cwd()Out[3]: 'C:\\WINDOWS\\system32'
```

让我们创建一个将开放连接端口链接到进程的函数。

首先，我们需要迭代所有打开的连接。正是我们所需要的！

```
In [1]: ps.net_connections?Signature: ps.net_connections(kind='inet')Docstring:Return system-wide socket connections as a list of(fd, family, type, laddr, raddr, status, pid) namedtuples.In case of limited privileges 'fd' and 'pid' may be set to -1and None respectively.The *kind* parameter filters for connections that fit thefollowing criteria:
```

```
+------------+----------------------------------------------------+| Kind Value | Connections using                                  |+------------+----------------------------------------------------+| inet       | IPv4 and IPv6                                      || inet4      | IPv4                                               || inet6      | IPv6                                               || tcp        | TCP                                                || tcp4       | TCP over IPv4                                      || tcp6       | TCP over IPv6                                      || udp        | UDP                                                || udp4       | UDP over IPv4                                      || udp6       | UDP over IPv6                                      || unix       | UNIX socket (both UDP and TCP protocols)           || all        | the sum of all the possible families and protocols |+------------+----------------------------------------------------+
```

我们可以看到 net_connections 返回的属性之一是“pid”。

我们可以将其与流程名称联系起来:

```
In [1]: def link_connection_to_process():    ...:     for connection in ps.net_connections():    ...:         try:    ...:             yield [ps.Process(pid=connection.pid).name(),    ...:                   connection]    ...:         except ps.AccessDenied:    ...:             continue # Keep going if we don't have access
```

我们应该记住，除非我们有一些根特权，否则我们不能访问特定的进程。因此，我们需要将它包装在一个 try-catch 语句中，以处理“AccessDenied”错误。

让我们检查输出。

它将输出大量数据，所以让我们打印第一个成员:

```
In [1]: for proc_to_con in ps.net_connections():    ...:     print proc_to_con    ...:     raw_input("...")    ...:['ManagementServer.exe', sconn(fd=-1, family=2, type=1, laddr=addr(ip='127.0.0.1', port=5905), raddr=addr(ip='127.0.0.1', port=49728), status='ESTABLISHED', pid=5224)]...
```

正如我们所看到的，第一个成员是进程名，第二个是连接数据:ip 地址、端口、状态等等。

这个函数对于了解每个进程使用了哪些端口非常有用。

我们都曾经遇到过“这个地址已经被使用”的错误，不是吗？

### 结论

psutil 模块是一个非常棒的系统管理库。这对于将资源作为代码流的一部分进行管理非常有用。

我希望这篇文章教会你一些新的东西，我期待着你的反馈。请告诉我——这对你有用吗？