

最近回顾总结了一下今年 9月25日召开的智能网卡研讨会议题。
会上国内外设备厂商、云服务商、科研机构，介绍了不少关于智能网卡方面的研究成果。

保持对行业动态的关注，经常记录。

**专业术语：**
>
> NIC （Network Interface Card），网络接口卡，即网卡。
>
> DPU （Data Processing Unit），数据处理单元。
>
> DOCA（Data-Center-Infrastructure-On-A-Chip Architecture），集数据中心基础设施于芯片架构。
> 

## 1 会议主题
| 序号 	| 主讲人 	| 主题                                                               	| Title                                                           	|
|------	|--------	|--------------------------------------------------------------------	|-----------------------------------------------------------------	|
|   1  	| 王瑞雪 	| 运营商智能网卡部署场景探索及思考                                   	| 中国移动研究院数据中心网络项目经理                              	|
|   2  	| 张远超 	| DPU创新技术赋能5G与数据中心                                        	| 芯启源智能网卡产品总监                                          	|
|   3  	| 张彭城 	| 阿里高性能网络探索与实践                                           	| 阿里云基础设施事业部高性能网络团队高级技术专家                  	|
|   4  	| 林飞   	| 混合态异构高性能计算平台网络发展的趋势和挑战                       	| 奥工科技售前工程师                                              	|
|   5  	| 任凯   	| 从SmartNIC到 DPU，腾讯自研智能网卡的“小才大用”                      	| 腾讯云智能网卡研发负责人                                        	|
|   6  	| 雷晓龙 	| 国产智能网卡在信创云场景的应用实践                                 	| 迈普规划部总经理                                                	|
|   7  	| 孙晓宁 	| 天翼云智能网卡产品的前世、今生和未来                               	| 天翼云高级工程师、硬件加速组负责人                              	|
|   8  	| 宋庆春 	| DPU使数据中心成为了计算单元                                        	| NVIDIA网络亚太区市场开发高级总监                                	|
|   9  	| 阎燕   	| 锐文科技在智能网卡上的探索                                         	| 锐文科技CTO&联合创始人                                          	|
|  10  	| 王昭峰 	| 浪潮智能网卡创新探索                                               	| 浪潮数据中心网络市场总监                                        	|
|  11  	| 吴航   	| 锐捷智能网卡演进之路                                               	| 锐捷网络云数据中心首席架构师                                    	|
|  12  	| 蒋东升 	| 洞见未来-可编程智能网卡Agilio                                      	| 芯启源产品解决方案总监                                          	|
|  13  	| 张然   	| 英特尔基础设施处理器(IPU)平台: 英特尔®FPGA IPU   C5000X/C6000X概览 	| 英特尔现场应用工程师                                            	|
|  14  	| 胡成臣 	| 赛灵思实验室的开源智能网卡工作                                     	| 赛灵思亚太区实验室和亚太区CTO office负责人                      	|
|  15  	| 吕高锋 	| 数据为中心的FPGA加速器技术                                         	| 国防科技大学计算机学院网络空间安全系副研究员                    	|
|  16  	| 黄朝波 	| 软硬件融合——超大规模云计算架构创新之路                             	| 上海矩向科技CEO、《软硬件融合——超大规模云计算架构创新之路》作者 	|

## 2 议题分析

### 2.1 运营商智能网卡部署场景探索及思考
问题现状：应用激增使得数据中心流量以每年 25% 速度增长，网络向高带宽和新型传输体系发展，网络堆栈处理越发复杂。
后摩尔定律时代，CPU 计算能力增速低于网络传输速率增速。

智能网卡：在服务器侧引入智能网卡，将网络、存储、操作系统中不合适 CPU 处理的高性能数据处理功能，卸载到硬件芯片执行，提升数据处理能力，释放 CPU 算力。
![smartNIC_in_ISP](http://cdn.talkaboutos.top/smartNIC_in_ISP.png)
应用场景：网络、存储功能卸载；DPDK、SPDK和RDMA等技术集成；针对特定业务逻辑进行硬件加速；解决裸金属存储网络的安全隐患；业务端到端网络可视化。


面临的挑战：

1.标准化待成熟，引入面临解耦压力；

2.集成度、灵活性及可靠性取舍。

<br>

### 2.2 DPU 创新技术赋能 5G 与数据中心

芯启源产品介绍：

芯启源 Corigine DPU，采用全可编程 DPU 芯片，支持丰富的数据面、控制面卸载和虚拟化支持。

<br>

### 2.3 阿里高性能网络探索与实践

现状问题：2017 年后，阿里云做了超大规模架构，并通过自主软硬件研发，实现了数据中心网络架构的自主可控。
在这个过程中，阿里云发现，高性能网络的挑战就在于时延。

主要介绍了阿里高性能网络的演进思路及方向。

<br>

### 2.4 混合态异构高性能计算平台网络发展的趋势和挑战 

高性能网络分类：
* 第一类是集群管理网络和硬件平台监控网络；
* 第二个是存储网络；
* 第三类为用于高性能计算的计算网络。

计算网络则是三类中最重要的一种，InfiniBand 具有高带宽、低延时的网络特性，常常被用于计算节点的数据交互和数据传输。

<br> 

### 2.5 从 SmartNIC 到 DPU，腾讯自研智能网卡的“小才大用” 

腾讯智能网卡 4 大典型应用场景：
* 1.客户自建 KVM 云游戏框架；
* 2.通过物理机部署办公云桌面集群
* 3.音视频 RTP 业务；
* 4.云原生容器化场景。

腾讯云自研智能网卡的一些技术突破：
* 软硬协同热迁移
* 弹性网卡/云盘密度 
* 网络性能

关键技术：
* 自研 vDPA 技术
* Net/Blk 全场景支持
* 硬件自定义标准
* 自研软硬件协同
* 资源池化管理和 CQ 聚合技术
* 自研 VirtIO_net 硬件后端核心 IP
* 自研 vSwitch Fastpath 硬件卸载
* 自研 vSwitch offload 高度软硬协同的硬件驱动层

<br>

### 2.6 国产智能网卡在信创云场景的应用实践

#### 2.6.1 信创云现状及：
* 1.国产 CPU 的算力与非国产 X86 体系仍有差距，对业务卸载优化性能的诉求强烈
* 2.信创本质上要解决安全问题，对于业务和数据安全的要求更高
* 3.产业成熟度有待进一步完善，信创体系的兼容性有待进一步提升

#### 2.6.2 对智能网卡的诉求：
* 1.对算力释放的诉求更加迫切
* 2.对业务和数据的安全要求更高
* 3.对生态适配性的要求更高


#### 2.6.3 迈普国产智能网卡在信创云中的应用实践
##### 信创服务器裸金属部署
* 网络卸载：控制、数据平面全卸载
* 存储卸载：存储卸载采用标准的 Virtio-blk 访问机制，host 发起存储请求。
* 安全加密：全面支持国密 SM1/SM2/SM3/SM4 系列密码算法；采用符合国密算法标准的数据安全保护机制；支持双端口万兆线速加密，满足信创服务器的基本业务安全诉求。


##### 信创服务器性能优化
通过网络、存储、加密卸载，充分释放服务器的算力。

<br>

### 2.7 天翼云智能网卡产品的前世、今生和未来 

现状：目前在内测阶段。

基于 ASIC 架构 的智能网卡。

在网络加速方面，其采用了 RoCE v2 技术、vxlan 隧道技术和 ovs ct功能。

<br>

### 2.8 DPU 使数据中心成为了计算单元

转变以往以计算为中心的思维，建立以数据为中心的新型计算架构。

如果数据需要用 CPU 处理，就应当放在 CPU 上；如果数据需要 GPU 处理，它就放在 GPU 上。

DPU ：通过面向不同的加速引擎，对不同的操作做卸载。

再通过 CPU 或是其他处理器来做控制平面的卸载或是网络协作，让 CPU 卸载的工作依赖于专业处理器，进行数据加速。

<br>

### 2.9 锐文科技在智能网卡上的探索

产品介绍。

<br>

### 2.10 浪潮智能网卡创新探索

#### 2.10.1 架构：FPGA+CPU 架构
* 一是高性能，FPGA 提供了接近 ASIC 的处理能力。
* 二是软硬件全可编程，产品设计更灵活，更能满足客户业务的实际演进

#### 2.10.2 虚拟设备硬件化带来的问题：
* 1.如何管理虚拟化设备
* 2.热迁移

#### 2.10.3 解决方案
1. SR-IOV 引入了两种 PCIe 的 Function，即 PF 和 VF，通常对应着裸金属和虚拟机的应用场景。

在虚拟机场景下，VF 的配置和管理由 VMM 完成，Guest OS 需要支持 VF 的动态热插拔；

在裸金属场景下，PF 的配置和管理由网卡 SoC 上管理程序负责，Host OS 需要支持 PF 的动态热插拔。

2. 基于VDPA 的热迁移方案优化，在感知硬件设备状态上，VDPA 控制和数据平面分离，在监控设备状态同时，提升转发性能。

在迁移过程中跟踪脏页，采用**网卡硬件**监控 DMA 页的跟踪，避免 Host 软件处理引发迁移过程中的性能下降。

#### 2.10.4 智能网卡与服务器的适配问题

智能网卡是大 server 的“小server”，拥有一套小系统，如何管理适配，体现在四个方面：供电、监控、管理、测试。

* 供电方面

小于 75W 的智能网卡，采用**金手指方式**供电；大于 75W 的智能网卡，采用**金手指+外接电源**的方式供电。

* 监控方面

智能网卡是个独立运行的小系统，需要像管理服务器一样，监控整个网卡的硬件状态，记录异常日志、诊断分析故障、以及远程固件升级等。

浪潮采用独立的 BMC 监管设计，既可以解决监控管理需求，又可以避免服务器侧的软硬件修改。

* 管理方面
智能网卡和服务器的管理拓扑分为两种：内部互联和外部互联。

内部互联，通过 UART、金手指的 I2C 以及 NCSI，Host BMC 与网卡 BMC 互联，两者为主从关系；

外部互联，通过网卡和服务器的网口互联，Host BMC 与网卡 BMC 相互独立，分开管理。

* 测试方面

浪潮开发服务器时会引入多品牌智能网卡，因此总结了一套完善的硬件功能测试和软件功能测试规范。

包括基本功能、卸载功能、自定义扩展功能、应用测试、兼容性测试等。

<br>

### 2.11 锐捷智能网卡演进之路
介绍锐捷智能网卡研发演进情况。

<br>

### 2.12 洞见未来-可编程智能网卡 Agilio

产品介绍。

<br>

### 2.13 英特尔基础设施处理器(IPU)平台: 英特尔®FPGA IPU   C5000X/C6000X概览

![intel_ipu](http://cdn.talkaboutos.top/intel_ipu.jpg)

图：Intel IPU 架构

---

为满足数据中心及云服务商（CSP）计算规模的急剧扩张以及对时延的严苛要求，Intel 提出 IPU（Infrastructure Process Unit）概念。

旨在将存储、网络等任务通过 IPU 进行硬件卸载。

避免该类非业务任务挤占 CPU 算力资源，影响云服务商对外的可售卖算力。

<br>

### 2.14 赛灵思实验室的开源智能网卡工作

介绍赛灵思亚太实验室在智能网卡的研究及应用情况。

项目 Github Repo 地址：
* OpenNIC shell(hardware):https://github.com/Xilinx/open-nic-shell

* OpenNIC driver:https://github.com/Xilinx/open-nic-driver

<br>

### 2.15 数据为中心的 FPGA 加速器技术

介绍 FPGA 在计算加速方面的优势，及学术界在该方面的研究成果。

<br>

### 2.16 软硬件融合——超大规模云计算架构创新之路

现状：摩尔定律到达临界点，CPU 性能面临迭代瓶颈。

图灵奖获得者D&J给出的方案是 DSA（Domain Specific Architecture，特定领域架构）。 

云计算是各种复杂场景的叠加，挑战在于：

如何把这么多场景优化融汇到一套平台化方案里；既满足灵活性的要求，又满足性能加速的要求。

提出了全新的设计理念和方法——软硬件融合，期望实现软件灵活性和硬件高效性的统一。

![soft_hardware_def](http://cdn.talkaboutos.top/soft_hardware_def.jpg)

---
期望实现的最终目标：

![Hardware_software_fusion](http://cdn.talkaboutos.top/Hardware_software_fusion.jpg)

---

# 参见
* [研讨会视频回放](https://wx.vzan.com/live/tvchat-523580827?shauid=WyqKJLAWeF_0J1qkwNqu5w**&vprid=0&sharetstamp=1632983084441&ver=ca5bd23b944a4816a33243b7008d52ff#/)
* [演讲资料下载 https://pan.baidu.com/s/1HVDkoj6r-NRpjzpG4jnyFQ](https://pan.baidu.com/s/1HVDkoj6r-NRpjzpG4jnyFQ ) 提取码：msuv

