# AWS 中最最基础的网络概念
原文：[https://grahamlyons.com/article/everything-you-need-to-know-about-networking-on-aws](https://grahamlyons.com/article/everything-you-need-to-know-about-networking-on-aws)

---

如果你有在 AWS 有基础设施服务或应用程序，那么下面所有这些概念你都将会碰到。
这些不仅仅是网络设施的一部分，根据我的经验来看，更是其中最重要的部分之一。

## VPC
VPC （Virtual Private Cloud ），虚拟私有云，是你的基础设施所运行在的一个私有网络空间。你可以自己选择 VPC 的地址空间（CIDR 范围），例如 `10.0.0.0/16`  。这将决定 VPC 内有多少可分配 IP 。因为 VPC 内的每一台服务器都需要一个 IP 地址，所以 VPC 地址空间的大小，将决定该私有网络可容纳的资源上限。 `10.0.0.0/16`  地址空间的可用地址从 `10.0.0.0`  到 `10.0.255.255` , 一共 2^16， 65,536 个 IP 地址。


VPC 是 AWS 中最基础的基础，每一个新创建的账户，在每个 `AZ` （Available Zone，可用区） ，都包含一个带 `子网`  的默认 VPC 。
![VPC](http://cdn.talkaboutos.top/VPC.png)

## Subnet（子网）
subnet（子网）是 VPC 的一部分，有其单独的 CIDR 范围和流量转发规则。其 CIDR 范围是 VPC 的一个子集。举个栗子，10.0.0.0 的一个子网 10.0.1.0/24，地址范围从 10.0.10.0 到 10.0.1.255 ，一共 256 个 IP 地址。


根据是否可以从外部（公网）访问进来，子网也经常被称为“公有”或“私有”子络。这种可见度是由网络路由规则控制的，每个子网都有其自己的规则。
子网必须位于某个区域具体的 AZ 中，因此，在每个区域划分子网是很好的做法。如果你要划分公有子网和私有子网，那么，在每个 AZ 都要有一套。


![subnet](http://cdn.talkaboutos.top/subnet.png)



## Availability Zones （可用区）
前面我们说到，每个 Availability Zone 必须有子网，可是，这到底是什么意思呢 ？
在 AWS 中，为了保证每个地区资源的极高的可用性。每个区域（zone）的资源都被划分为 2 个或更多不同可用区（AZ）。对每个区域（zone）来说，基本上即使其他所有 AZ 都中断了，也要确保至少有一个 AZ 还能正常运行。
![AZ](http://cdn.talkaboutos.top/AZ.png)

## Routing Tables（路由表）
路由表，是一系列规定子网 IP 数据包如何传输到其他不同 IP 地址的规则。
每个 VPC 内，都有一个默认的路由表，只允许流量在本地（VPC 内部）转发。如果某个子网没有关联路由表，则使用该默认路由，也就是一个“私有”子网。
如果你想要能从外部访问子网，那么你就需要创建一个路由表，显式地指定该规则。这样，关联该路由表的子网就是“公有”子网。

## Internet Gateways（互联网网关）
将子网配置为可从外部访问的路由表，需要借助互联网网关来控制外部数据包出入 VPC 。
例如，创建一个网关，配置规则为 `所有去往 0.0.0.0/0 的数据包均需通过网关`  。
![InternetGateways](http://cdn.talkaboutos.top/InternetGateways.png)

## NAT Gateways（NAT 网关）
如果你有一台在 AWS 私有子网内，不允许公网访问的 EC2 实例，那么该实例的数据也没办法发送到公网。我们需要一种机制，将数据包发出去，并且正确地接收到对端的响应包。这便是 NAT ——网络地址转换，跟家里的 wifi 路由器很类似。
NAT 网关是位于公有子网的一个设备，负责接收从私有子网发往公网的数据包，转发至其目的地址；同时转发返回的数据包到源地址。
如果你 VPC 中私有子网中的实例不需要公网访问的话，那么 NAT 并不是必需的。只有你的实例需要访问诸如外部 API，SaaS 数据库等时，你才需要配置 NAT 网关。或者是使用 AWS 提供的 NAT 网关资源（由 AWS 配置，更易于用户管理）。

![NAT_Gateways](http://cdn.talkaboutos.top/NAT_Gateways.png)

•**NAT 网关** 位于公有子网中•从私有子网发出对某个公网地址的请求•根据路由表规则，请求数据包被转发到 NAT 网关•NAT 网关将数据包转发出去

## Security Groups（安全组）
VPC 网络安全组标志 VPC 中的哪些流量可以发往 EC2 实例或从 EC2 发出。安全组指定具体的入向和出向流量规则，并精确到源地址（入向）和目的地址（出向）。这些安全组是与 EC2 实例而非子网关联的。
默认情况下，流量只允许出，不允许入。
入向规则可具体指定源地址——CIDR 段或者另一个安全组亦或是端口范围。当指定的源地址为另一个安全组时，该安全组必需位于同一个 VPC 。例如，VPC 默认的安全组允许任意来自同一安全组的访问流量。如果将该安全组应用于 VPC 内创建的所有资源，那么该 VPC 中的资源之间将可以互通。
![SecurityGroups](http://cdn.talkaboutos.top/SecurityGroups.png)

•实例 (i-67890) 的安全组 (sg-abcde) 允许来自 443 端口的 TCP 流量•来自 IP 10.0.1.123 22 端口的请求不被允许通过•来自 443 端口的请求被允许通过

## 所有这些组装起来
整个虚拟网络完整的架构如下图所示：公有子网/私有子网跨越 AZ 将整个网络划分为两部分。NAT 位于公有子网，用路由表指定数据包的路由规则。 EC2 实例将运行于任意子网，附加安全组。
![whole-arch](http://cdn.talkaboutos.top/whole-arch.png)

## 
## Reference


•1.Amazon Virtual Private Cloud 用户指南 [https://docs.amazonaws.cn/vpc/latest/userguide/what-is-amazon-vpc.html](https://docs.amazonaws.cn/vpc/latest/userguide/what-is-amazon-vpc.html)
