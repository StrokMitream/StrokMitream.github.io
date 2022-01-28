# 【翻译】如何测量程序执行时间

> 本文翻译自：https://serhack.me/articles/measure-execution-time-program/
>
> 本文首先介绍了计算机领域关于 `时间` 的几个概念，包括**墙上时钟时间**、**CPU 时间**，以及计算机是如何表示时间、日期。接着介绍了测量程序执行时间的几种方法，包括**秒表法**和**时钟周期法**。
>
> **由于译者水平有限，本文不免存在遗漏或错误之处。如有疑问，请查阅原文。**
>
> 以下是译文。



![](https://cdn.nlark.com/yuque/0/2021/png/148899/1634556713146-02f9c51f-acb1-4d68-b6af-cfb94829fd37.png#clientId=u5e2a07d6-1055-4&from=paste&id=ubf2c16aa&margin=%5Bobject%20Object%5D&originHeight=450&originWidth=800&originalType=url&ratio=1&status=done&style=none&taskId=ueea164d6-a696-462a-a5ae-192818aee8e)


**测量程序的性能，意味着对程序资源消耗的持续追踪。**



除了简单的技术性能指标，比如查看 RAM 和 CPU 利用，监测任务的具体执行时间也是非常有用的手段。常见的比如对一组数据的升序排序消耗的时间，与所采用的排序算法密切相关。



在深入优化一个算法之前，理解如何计量程序的执行时间，往往非常有用。本文，我们将首先理解 时间 这个概念，然后再来探索一些入门的计量方法 —— 按照时间精度顺序来介绍。



## 1 理解 `时间` 的概念
测量一个编译执行或 解释执行的程序执行时间，远比我们想象中困难得多。因为有很多的方法，是难以移植到其他平台。选择哪一种合适的测量方法，取决于你的操作系统，编程语言，甚至我们所要测量的 “时间” 。




### 1.1 墙上时钟时间 vs CPU 时间

在测量程序运行时间中，这是两个常用的术语。我们首先来看下这两者的定义和区分。



1.**墙上时钟时间**，即“数字时钟”时间。也就是在测量过程中，总的消耗时间。这是你能通过一个 秒表 测量的时间，前提是在，对应程序执行的恰当节点，开启和结束秒表计时。

2.**CPU 时间**，指的是 CPU 在处理程序指令的时间。在程序运行过程中消耗的等待（比如，等待 I/O 操作）时间，不包括在 CPU 时间里面。



![](https://cdn.nlark.com/yuque/0/2021/png/148899/1634556712628-43b5e3c5-b7a3-4a3a-9ec1-3d1f6d95ace6.png#clientId=u5e2a07d6-1055-4&from=paste&id=ua1a3d11f&margin=%5Bobject%20Object%5D&originHeight=249&originWidth=800&originalType=url&ratio=1&status=done&style=none&taskId=ubabfdbdd-ca4f-43ec-9686-e784c37bb34)





简单来说，墙上时钟时间计量的是，多长时间过去了（假设你正在盯着墙上的时钟）。而 CPU 时间则是 CPU 执行程序指令的工作时长。

通常来说，系统时间更接近于 CPU 时间，尤其是在并行执行的软件里面。如果线程结束不再占用 CPU 时间，以 CPU 时间作为计算任务运行的系统时间，也是更加自然合理。稍后我们会在详细讨论 系统时间。

![CpuTimeonSingleCpuMultiTaskingSystem.](http://cdn.talkaboutos.top/CpuTimeonSingleCpuMultiTaskingSystem..png?imageMogr2/thumbnail/!50p)

> 例如，在上图所示的单核 CPU 多任务操作系统上，程序 P1 从开始运行到结束过程中，CPU 中 P1、P2 和 P3 交替执行。
>
> 从 P1 开始执行到结束的时间，称为 P1 的墙上时钟时间；蓝色方块标记的时间，称为 P1 的 CPU 时间。

为了获得准确的系统时间，你需要计算机主要的只运行你需要计量的程序。计算机可能依然在运行操作系统和一些后台程序，不过没有 GUI 和其他应用程序。这将为测量提供最好的环境，选择 系统时间 是合适的选择。




## 2 技术方法

在接下来的几个小节，我们将一起探讨几种最常用的量化时间的技术方法。



### 2.1 秒表法

最简单直接的测量时间方法是，在程序开始执行的一瞬间摁下秒表计时。

假定你做到了同时启动秒表和程序，这还需要程序在执行的结束或例程的结束节点，打印一个执行完成的提示。


![](https://cdn.nlark.com/yuque/0/2021/png/148899/1634556805884-cc63b336-d6f7-4672-829a-781236e0666a.png#clientId=u519b8e1b-54bd-4&from=paste&id=u33a4b0b5&margin=%5Bobject%20Object%5D&originHeight=299&originWidth=800&originalType=url&ratio=1&status=done&style=none&taskId=u2bfcfb57-84a6-4eb8-bd8c-8860261e282)



#### 秒表一旦摁下，计时就已开始。

这不是最佳的方法，平均来说人类反应时间在 0.25 - 1s 之间，另外摁下机械式或电子式按钮，又要小几秒钟。




### 2.2 `time`测量工具

替代手动秒表，更加快速自动化的方法是 `time`测量工具。这是一种非常容易使用，无需任何特殊设置的工具。使用时只需按照 `time ./program-name` 的方式执行即可。`program-name` 文件名称或要执行的命令。

与 `program-name` 程序执行结果一起输出的，还有 3 种不同的时间：

* **Real Time**：使用系统内部时钟测量得出的真实时间（单位：秒）；
* **User Time**：程序指令在”用户“会话模式执行的时间；
* **System Time**：指令在”特权“模式执行的时间——这部分时间通常远小于 User Time 。





![time_utility](http://cdn.talkaboutos.top/time_utility.png)

> 例如，我在 Debian 上运行的 `nextPrimeNumber ` 测试程序，`time` 命令输出表明：
>
> 该测试程序用户态时间**为 0.13s ，总耗时为 0.13s 。

对于  User_time + System_time = Real_time 这并不是必须的。 `User Time` 和 `System Time` 是以 CPU-秒 来计算的（例如，1s 包含若干个时钟周期）。

产生 `User Time` 与 `System Time` 差异的原因在于，进程执行的内部机制。通常来说，以最简单的情况为例，我们可以把操作系统划分为两部分：用户态部分和特权（内核）态部分。特权（内核）态拥有比用户态更高的权限。

进程的绝大部分指令都是运行在用户态，不需要更高的权限。当需要与 I/O 设备交互，分配内存，或其他干涉其他程序的行为，必须通过 `系统调用（system call )`的方式，由 内核 代为执行相关指令。



这也被称为 **最小权限原则**。

这是非常重要的：通过这种方式，用户不具备最高权限，不能随意执行指令。对于”非法“的系统调用，内核只会终止进程，并不提供更多的额外信息。

> 某些 shell ，如 bash 或 zsh ，有内建的 time 命令提供类似的时间和其他资源使用情况。
>
> 为了访问到 time 命令，可能需要指定其绝对路径（/usr/bin/time）。
>
> 使用 type 可以查看系统上的 time 命令，到底是一个二进制程序还是 shell 内建关键字。



> ![type_time](http://cdn.talkaboutos.top/type_time.png)
>
>  `type` 命令显示，我 shell 上的 `time` 是保留关键字。




### 2.3 计量时钟信号

为了正确地执行程序，确保计算机的各部件同步是非常重要的一件事。

实际上，计算机是由电信号驱动，如果他们之间有延迟或重叠，那么系统可能不能正确响应。



指挥整个系统同步的，是一个统一的信号——叫做”CLOCK（时钟信号）“。他是由 CPU 内的矿物（通常是石英晶体）产生的，信号的频率（例如，何时从 0 变到 1 ）是由 CPU 的主频决定的。

举例来说，一颗　1GHz 的 CPU，时钟信号在 1s 内进行了 10^9 次 0 到 1 的过程。因此，对于每次信号从 0 到 1 ，时间是 10^-9 s 。处理器内部的部件，只有在时钟信号变化的瞬间，才进行”stressed（触发）“ 。如果电子部件是在上升沿（比如，时钟信号从 0 到 1 ）触发，我们说这个部件是 上升沿触发；反之，称为 下降沿触发。


![](https://cdn.nlark.com/yuque/0/2021/png/148899/1634556711923-09febd0b-40fc-4d8c-98e1-bfcd05898a69.png#clientId=u5e2a07d6-1055-4&from=paste&id=u6be3c1d0&margin=%5Bobject%20Object%5D&originHeight=260&originWidth=800&originalType=url&ratio=1&status=done&style=none&taskId=u34cc83e6-cbaf-458e-8c55-db6a513649d)


#### CPU 内部时钟信号，在 ms 间从 0 到 1 翻转。

因此，如果我们得知了 1秒钟内进行了多少个时钟，并能区分出程序执行的起止点，那么我们就可以把计算出程序的执行时间。




为了测量执行时间，你可以计量程序执行起、止点之间，经过的时钟周期。

这个时候，通过计算两点的时钟间隔数，以及 CLOCKS_PER_SEC ，可以计算出从第一个 clock() 到第二个 clock() 调用间经过的时间。代码样例如下：

```c
    clock_t start = clock();
	// let's do some operations
	clock_t end = clock();
	long double seconds = (float)(end - start) / CLOCKS_PER_SEC;
```


CPU 时间用数据类型 clock_t 表示，其涵义是时钟信号的 tick 次数。经过的时钟周期次数，除以 1 秒钟的时钟信号次数，可以得到进程主动使用 CPU 的总时间。

注意，CLOCKS_PER_SEC 已经由编译器定义，需要在头文件中包含 time.h 。

> 额外插一句这种测量方法的精度问题。
>
> CLOCKS_PER_SEC 精度不超过 ms 级（该变量的默认值为 1000000 ），因此通过 clock() 测量出来的时间，与 time 工具，可能会存在比较大的差异。





### 2.4 Time（） 函数和纪元时间

这里，我们再了解一下第三个概念：纪元时间（Epoch Time）。这是计算机计算系统日期时间的方法。大多数计算机系统，将时间表示为，从某个特定时间日期起的秒数。



例如，Unix 和 POSIX 系统以1970-01-01 00:00:00 UTC 为标准的时间，将目标时间与 1970-01-01 00:00:00
时间的差值以秒来计算 ，单位是秒——这就是 ”Unix 纪元“。Windows NT 及其后续版本上的 NT Epoch Time 指的是与 1601-01-01 的时间间隔，单位（10^-7 ）秒 。可以把纪元时间（Epoch Time ）理解为公历里面的公元 0 年。



你可能会问，如果我需要表示一个 Epoch Time 之前的日期怎么办呢？ 完全没问题，用负数表示即可。因此，如果你得到了一个负值，记得提醒你自己：你的计算机没坏，不需要更换内部时钟的电池。



`time.h` 实现的 time() 函数，返回与 1970-01-01 00:00:00 UTC 时间间隔差，单位为 秒。如果出错，则返回 -1 .

Unix 平台的 `time_t `，表示的是某个时间点，直接编码前面小结提到的 Unix 时间数字。其在大多数平台上是 32位有符号整数（不过扩展到 64 位了）。如果是 32 位的话，意味着一共能覆盖到的时间大约 136 年。如果扩展到 64 位的话，可以覆盖到正、负 2930 亿年！


​	

```c
    time_t begin = time(NULL);
 	// Call to a function that takes up time
 	time_t end = time(NULL);
    printf("It has been %d seconds from point begin to end", (end-begin));
```
在这种情形下，由于时间是以 秒为单位计算，这种方法测量出来的时间，精度低于 ms 级。






## 3 更多高级方法

本文，我们描述了几种技术方法，通过使用内建的 C 库，在程序里面实现某种形式的内部秒表。然而，C 语言的 API 接口有限，且大多是以教学为目的。在大型项目中，你不仅仅需要知道如何测量，还需要实时观测性能变化。



为什么在我访问某资源的时候，CPU 猛增？如何定位性能瓶颈？为了回答这些常见的问题，有个叫 ”Profiler“ 的独立应用程序，在追踪程序性能方面非常有用。



Profiler 可以分析每一行代码。对开发者来说，使用 profiler 并不是非常合适，因为他会使得程序的执行速度指数级的降低。尽管大多数人把 profiler 作为一个情境工具而不是日常使用工具，但是在需要的时候使用它还是非常有用的。

此外，profiler 在寻找代码的活动路径时非常有用。可以先用 profiler 定位占用全部代码 CPU 用量 20% 的代码，进一步改进优化。

最后，profiler 可以在内存泄漏的早期检测中提供辅助，同时帮助理解调用性能。





## 4 结论

”如果你可以每天优化 1%，那么一个月下来就是 30% 的优化。“ 

真正产生的变化的，是那些持续不断的努力。


---
![扫码_搜索联合传播样式-白色版](http://cdn.talkaboutos.top/%E6%89%AB%E7%A0%81_%E6%90%9C%E7%B4%A2%E8%81%94%E5%90%88%E4%BC%A0%E6%92%AD%E6%A0%B7%E5%BC%8F-%E7%99%BD%E8%89%B2%E7%89%88.png)
