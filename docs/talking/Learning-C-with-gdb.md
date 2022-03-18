我写作本文的目的是，是向大家展示 gdb 这一学习 C 语言的强大工具。我将介绍一些我最常用的 gdb 命令，同时还将会演示如何用 gdb 来理解 C 语言中最令人头疼的内容：指针和数组的区别。

## gdb 简介
我们用这个 `minimal.c` 的小程序来开始：
```c
int main()
{
   int i = 1337;
   return 0;
}
```
我们的程序简单到连一个 `printf`  都没使用。

来吧，拿出勇气来，用 gdb 来探索 C 语言的新世界吧！

用 `-g` 选项编译，以生成带调试信息的可执行文件，导入 gdb :
```bash
$ gcc -g minimal.c -o minimal
$ gdb minimal
```
现在，如果你看到的是一堆 gdb 提示信息，说明你已经进入 gdb 了。接下来：
```bash
(gdb) print 1 + 2
$1 = 3
```

Amazing！`print` 是 gdb 用于处理 C 表达式的内建命令。如果你对 gdb 的某个命令不清楚， 试试` help 命令`查看 gdb 的帮助。
接下来，看一个更有趣的例子：
```bash
(gdb) print (int) 2147483648
$2 = -2147483648
```
至于为什么 `2147483648 = -2147483648` ，我且不做解释。这里要表达的意思是，在 Ｃ语言中，即使是算数，也充满着各种陷阱。还好 gdb 能看透这种伎俩。

现在我们在 `main` 函数设置断点并运行：
```bash
(gdb) break main
(gdb) run
```
好了，现在程序暂停在第３行，变量 `i` 初始化之前。趁 `i` 还未初始化，我们用 `print` 命令来显示它的值：
```bash
(gdb) print i
$3 = 32767
```
在 C 语言中，本地变量在未初始化前，它的值是不确定的，所以在你的电脑上 gdb 显示的结果可能跟我的不一样！
我们用 `next` 命令执行当前这行代码：
```bash
(gdb) next
(gdb) print i
$4 = 1337
```

## 内存查看命令 `x` 
C 语言为变量分配连续的内存空间。一个变量的内存空间由两方面决定：

1. 内存块的起始字节地址。
2. 内存块的 `byte` 大小。变量内存大小是由变量类型决定的。

Ｃ语言鲜明的特征之一便是，可以直接访问内存空间。取地址操作符 `&` 用于获取变量的地址，而 `sizeof` 操作符用于计算变量占用的内存空间大小。

尝试在 gdb 运行下面的命令:
```bash
(gdb) print &i
$5 = (int *) 0x7fff5fbff584
(gdb) print sizeof(i)
$6 = 4
```

输出结果表明，变量 `i` 的内存起始地址为 `0x7fff5fbff584` ，并且占用４ byte 空间。
我在上面曾说过，变量占用内存的大小由它的类型决定，实际上 `sizeof` 运算符也可以直接对变量类型进行计算：
```bash
(gdb) print sizeof(int)
$7 = 4
(gdb) print sizeof(double)
$8 = 8
```
这说明，在我的机器上， `int` 类型变量占４字节，而 `double` 类型变量占８字节。

 `x` 命令，是 gdb 直接查看内存的强大工具。 `x` 命令查看指定位置起始的内存段，并有一系列格式化命令精确控制查看的字节数以及显示方式。

如有疑问，请在 gdb 中查看帮助 `help x` 。

由于 `&` 操作符用于计算变量地址，因此可以把 `&i` 的结果作为参数传入 `x` ，这样，我们便可以窥探到变量 `i` 在内存的原始字节形式啦：
```bash
(gdb) x/4xb &i
0x7fff5fbff584: 0x39    0x05    0x00    0x00
```
 `4xb` 意思是，我要查看４个数值，格式化为 he `x`  (十六进制), 按1 byte一次显示。我只选择显示４字节，是因为变量 `i` 大小为4个字节。上面的输出显示， `i` 是逐字节存放在内存中的。

在直接查看内存字节时，这里有个细节需要记住：在 Intel 的机器上，字节内容是按照"little-edian" 小端的方式存放的。
与人的直觉相反，小端方式是数值的最低有效位放在内存的最前面。

为了更加直观，举个例子来说明：
```bash
(gdb) set var i = 0x12345678
(gdb) x/4xb &i
0x7fff5fbff584: 0x78 0x56 0x34 0x12
```

## 类型查看命令 `ptype` 
 `ptype` 是我最常用的命令，它可以返回 C 表达式的类型。
```bash
(gdb) ptype i
type = int
(gdb) ptype &i
type = int *
(gdb) ptype main
type = int (void)
```
Ｃ 语言中，有非常复杂的数据类型，不过，利用 `ptype` 命令，便可以以交互的方式来查看，实在是方便的很～

## 指针与数组
数组是 C 语言中相当灵活的内容。我写的这部分就是想通过 gdb 剖析这个简单的程序，进而弄懂数组的含义。
名为 arrays.c 的程序：
```c
int main()
{
    int a[] = {1,2,3};
    return 0;
}
```

用 `-g` 选项编译，装入 gdb 中运行， `next` 直到运行完数组初始化：
```bash
$ gcc -g arrays.c -o arrays
$ gdb arrays
(gdb) break main
(gdb) run
(gdb) next
```

看下经过初始化化后 `a` 中的内容：
```bash
(gdb) print a
$1 = {1, 2, 3}
(gdb) ptype a
type = int [3]
```
好了，程序正确地运行了。接下来我们首先要做的是用 `x` 命令看看 `a` 在内存中到底是怎样一种存在：
```bash
(gdb) x/12xb &a
0x7fff5fbff56c: 0x01  0x00  0x00  0x00  0x02  0x00  0x00  0x00
0x7fff5fbff574: 0x03  0x00  0x00  0x00
```
上面的结果显示，`a` 这块内存起始地址为 `0x7fff5fbff56c` ，第一个４ byte 存储在 `a[0]` ，下一个４ byte 存放在 `a[1]` ，最后４ byte 放在 `a[2]` 。  当然，也可以通过 `sizeof` 得知 `a` 所占内存大小：
```bash
(gdb) print sizeof(a)
$2 = 12
```
从这点上来看，数组确实有其作为数组的特征：有他们自己像数组的类型以及存放数组于连续的内存空间。然而，从特定的角度看，数组的行为又与指针极为相似：
```bash
= preserve do
  :escaped
    (gdb) print a + 1
    $3 = (int *) 0x7fff5fbff570
```
这显示，`a + 1` 是一个指向 `0x7fff5fbff570` 的 `int` 指针。到这里，本能地用 `x` 命令看下这地址的内容：
```bash
= preserve do
  :escaped
    (gdb) x/4xb a + 1
    0x7fff5fbff570: 0x02  0x00  0x00  0x00
```
我们注意到 `0x7fff5fbff570` 比 `a` 的起始地址 `0x7fff5fbff56c` 大４个字节。因为给定 `int` 类型占４byte ，所以 `a + 1` 应该是指向 `a[1]` 。

事实上，数组的下标运算是指针的语法糖：`a[i]` 等同于 `*(a + i)` 。看下面的尝试：
```bash
= preserve do
  :escaped
    (gdb) print a[0]
    $4 = 1
    (gdb) print *(a + 0)
    $5 = 1
    (gdb) print a[1]
    $6 = 2
    (gdb) print *(a + 1)
    $7 = 2
    (gdb) print a[2]
    $8 = 3
    (gdb) print *(a + 2)
    $9 = 3
```

由上面种种可知，在一些环境下 `a` 类似于数组，而在另外一些条件下它又像指向数组第一个元素的指针。这究竟是怎么一回事呢？

这个问题的答案是：当数组名用在 C 表达式中时，它便“退化"为指向数组第一个元素的地址。不过，对于这条规则，有２个例外：

- 数组名作为 `sizeof` 参数时；
- 数组名用于 `&` 操作符时。

这就让人疑惑了，难道同为指针的 `a` 和 `&a` 竟是不相同的吗？

就数值而言，它们都表示同一地址：
```bash
= preserve do
  :escaped
    (gdb) x/4xb a
    0x7fff5fbff56c: 0x01  0x00  0x00  0x00
    (gdb) x/4xb &a
    0x7fff5fbff56c: 0x01  0x00  0x00  0x00
```
**然而，它们本身的类型却是不相同的。**

此前，我们已经知道 `a` 是指向数组第一元素的指针，所以是 `int *` 类型。

至于 `&a` 的话，我们直接用 gdb 查看一下：
```bash
= preserve do
  :escaped
    (gdb) ptype &a
    type = int (*)[3]
```
很明显， `&a` 是指向带有３个整数的数组的指针，亦即 `int [3]` ，显然与 `a` 不一样嘛。
在下面的指针运算中，你可以更加明显的看到 `a` 与 `&a` 的差异：
```bash
= preserve do
  :escaped
    (gdb) print a + 1
    $10 = (int *) 0x7fff5fbff570
    (gdb) print &a + 1
    $11 = (int (*)[3]) 0x7fff5fbff578
```
`a` + `1` 只是在 `a` 的地址上加４，而 `&a`  + `1` 却增加了12！
指针 `a` 实际上是与 `&a[0]` 等同的。

## 总结
希望我上述的内容已经让你心动：gdb 的确是一个深入探索 C 语言不可多得的学习环境！　

心动不如行动，赶紧用起来吧，在实践中不断提高！


**参考** 

1. [GNU Debugger - Wikipedia](https://en.wikipedia.org/wiki/GNU_Debugger) ;
2. [Learning C with gdb](https://www.recurse.com/blog/5-learning-c-with-gdb) ;
3. [Expert C Programming: Deep C Secrets](http://www.electroons.com/8051/ebooks/expert%20C%20programming.pdf) .







---

![扫码_搜索联合传播样式-白色版](http://cdn.talkaboutos.top/%E6%89%AB%E7%A0%81_%E6%90%9C%E7%B4%A2%E8%81%94%E5%90%88%E4%BC%A0%E6%92%AD%E6%A0%B7%E5%BC%8F-%E7%99%BD%E8%89%B2%E7%89%88.png)
