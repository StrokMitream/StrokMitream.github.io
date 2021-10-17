# OSDev Wiki——操作系统开发入门基础（二）- 必知必会的先修技能


如果你认为可以跳过该部分，或许你正是需要好好看一看本文。

写一个操作系统，可不是一个针对初学者的任务。实际上，写一个操作系统通常被认为是最难的编程任务。

在你开始考虑进行这么一个项目之前，你需要有高于普通开发人员的编程技能。


你需要掌握这些：

## 1.计算机科学基础

精通十六进制、二进制和布尔逻辑，以及相关的计算机基础知识，比如数据结构设计及操作方法，搜索和排序算法，抽象编程概念等等。


## 2.语言和词汇（part 1）

熟练使用英文进行阅读和写作。几乎所有的技术文档，网络上能找到的资源（比如这个 wiki 和 [论坛](http://forum.osdev.org/)），都是英文的。使用不正确的术语，将会使你看起来很蠢，并使得愿意帮你的人感到困惑。


## 3.语言和词汇（part 2）

这个网站提及到的大部分操作系统以及代码片段，是用 C （或 C++ ）写的。即使你选择另外一种语言（如 [FreeBASIC](https://wiki.osdev.org/FreeBASIC) 或 [Pascal](https://wiki.osdev.org/Pascal)）， C 也是编程的通用语言，你必须对这门语言从头到尾理解掌握。

## 4.汇编

你需要了解低级语言————[汇编](https://wiki.osdev.org/Assembly)。翻一翻书，参加学校的汇编课程，写一些用户空间的代码来熟悉汇编。即使你打算用更高级的语言来编写你操作系统的绝大部分，你还是需要用到汇编语言。


## 5.编程经验

希望通过一个操作系统项目来学习编程，这是一个糟糕的想法。

你不仅要对你用于开发的语言里里外外都摸透、弄懂，你还得熟悉版本控制，debug 等等。总之，在你尝试开发一个操作系统之前，你应该用该编程语言成功编写过不少的用户空间程序。



## 6.编程习惯

你应该清楚，如何编写代码和用户文档，以及仔细地记录代码和设计的各个方面；即使该项目只是单纯的你自己使用。
同时，你也需要学习使用合适的[代码管理](https://wiki.osdev.org/Code_Management)方法，包括配置和使用外部仓库（例如，Savannah，Github，GitLab，Heroku）。

培养好这两个习惯，在将来会极大地减少遇到的麻烦。



## 7.UNIX 经验

你将很快发现，操作系统开发中的许多工具，最开始是为 Unix 开发的，随后才移植到 Windows 系统上。Linux 内核常常被用来作为某个功能特性工程实现的参考和举例。


许多的因兴趣爱好开发的操作系统（hobby OS）都与 Unix 存在相似之处。熟悉 Unix 命令行，是一项非常重要的必要条件。（[Cygwin](https://wiki.osdev.org/Cygwin) 为 Windows 提供了一套易于安装的 Unix 命令行环境）。如果你没有 Unix 命令行使用经验，试着用 Linux 或 BSD 系统一段时间。

对于 Windows 用户，通过虚拟化软件安装一个[虚拟机](https://wiki.osdev.org/Emulators)，就可以相当简单的实现，无需对你的机器重新分区。你也可以在 Windows 自身安装 WSL（Windows 子系统 Linux），来访问 Unix 命令行。

对于 macOS 用户，用系统自带的终端（Terminal）就可以。
macOS 内核是 Unix 变种内核（Mach 内核与 BSD 内核的混合体）。因此只要你安装有 Xcode ，模拟器或者虚拟化软件，无需更多的工具（尽管跨平台编译器还是需要的）。
macOS 默认的终端 shell 是 Bourne-Again shell，C shell 和 KornShell 也是支持的。



## 8.工具链

你应该详细地知道你的编译器，汇编器，链接器，和 make 工具的工作行为。你应该清楚知道发出的警告和错误提示的涵义。对于你使用的工具，手册文档应该随时查阅，在向社区提问之前先查询手册文档。

尽管放心，关于 [GCC](https://wiki.osdev.org/GCC) ,[GUN as](https://wiki.osdev.org/GAS),[NASM](https://wiki.osdev.org/NASM),[GUN ld](https://wiki.osdev.org/LD),[Visual Studio](https://wiki.osdev.org/Visual_Studio)，以及 [GRUB](https://wiki.osdev.org/GRUB) 的初学者所有可能提出的问题，网上的回答不止一两个。发问前先搜索。



## 9.模拟器和虚拟化软件

熟悉 [Bochs](https://wiki.osdev.org/Bochs)，[VirtualBox](https://wiki.osdev.org/VirtualBox)，[QEMU](https://wiki.osdev.org/QEMU)，[Virtual PC](https://wiki.osdev.org/Microsoft_Virtual_PC) 这些工具，会让你在开发过程中事半功倍。这些工具，在你的真实硬件与你的测试系统之间，构建起一层缓冲（buffer）。尽管你在操作系统开发的时候将会有针对性地了解这些，不过你肯定在开发之前就想了解这些工具是什么，哪里可以获取到。


## 10.可执行文件格式

相较于应用程序（用户空间）开发，内核空间编程有许多额外的要求。理解[可执行文件](https://wiki.osdev.org/Category:Executable_Formats)的能力便是其中之一（你当然希望你的 OS 装载和执行应用程序，不是吗）。去弄懂熟悉可执行文件的格式，内部结构，以及链接器是如何生成可执行文件的。



## 11.平台

熟悉你开发的操作系统的目标平台（处理器）的技术文档。它们将为你提供，你的内核设计起始部分所需的信息。询问可以从文档轻易获得的信息，将会得到“请阅读（……）手册”的回复，或是 RTFM 。



## 12.概念


你应该从技术层面，对现有的操作系统的功能特性有一个了解（例如，通过阅读一些[书籍](https://wiki.osdev.org/Books)），哪些是优秀的，哪些是糟糕的，以及你自己的系统又将如何设计。


# 参阅


关于这个本文的主题，推荐一本相当优秀的书籍（略微遗憾是目前尚未完成）：[操作系统从 0 到 1](https://github.com/tuhdo/os01/blob/master/Operating_Systems_From_0_to_1.pdf)（Operating Systems From 0 to 1）。

这本书集中讨论了所有你需要知道的知识点，从晶体管到逻辑电路，汇编语言和 C 语言，ELF 格式和 debug 等等。

阅读（并理解）这本书，有助于你快速掌握本文提到的这些先修技能。
