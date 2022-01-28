# pfSense——跑在 VMWare 上的防火墙

如果你正在学习防火墙配置，而又没有可以随意修改/配置的防火墙设备；那么，本文或许可以助你一臂之力！
且随我慢慢道来。


本文要介绍的是—— `pfSense`  ，是的，就是它!
`pfSense`  是一个基于 `FressBSD` ，专为防火墙和路由器功能定制的开源版本。它被安装在计算机上作为网络中的防火墙和路由器，并以可靠性著称，且提供往往只存在于昂贵商业防火墙才具有的特性(如 VPN、IDS、IPS)。


# 1 下载系统镜像
首先，我们去[官网](https://www.pfsense.org/download/)下载安装镜像：
![pfsense_download](http://cdn.talkaboutos.top/pfsense_download.png)
图：pfSense 镜像下载


当前最新版本为 `2.4.5-p1` ，选择架构 `AMD-64 位` ，安装类型为 `ISO 镜像` ，选择离我们最近的下载节点 `Singapore` ，点击下载即可。顺带把底下的 `SHA256 Checksum ` 也下载下来，方便等下校验。


ISO 下载完成后，校验无误即可开始安装了。记得一定要校验！


# 2 安装
打开 `Vmware Workstation ` ，新建虚拟机。
配置 1 个 CPU 核心，1 GB 内存就可以；如果内存不充裕，512MB 也跑得起来。
除了有几个地方需要注意，其他默认就行：

- 1.客户机操作系统要选择 ` FreeBSD 64 位` ，毕竟 `pfSense ` 是基于 `FreeBSD`  的。
- 2.至少配置两张网卡，一个用作 `WAN `口，一个用作 `LAN`  口。
- 3.至少要有一张网卡设置为 `桥接模式` 或 `仅主机` 模式。即确保我们可以从物理机连接到 `pfSense `防火墙上。同时，记录下设置为 `桥接模式` 网卡的 `MAC 地址` （选择“桥接”网卡，点击“高级”，即可查看到 `MAC 地址` ）。（后面有用）

![image.png](https://cdn.nlark.com/yuque/0/2020/png/148899/1607957745409-47c9db7b-e4f5-4823-94b9-2a73bc979ea5.png)
图：查看桥接网卡 MAC 地址


做好这些配置，就可以开始系统安装了。


# 3 配置 pfSense
安装开始后，前几个选项，选择默认的配置即可。
直到配置 `WAN 口` 和 `LAN 口` 地址处：
`pfSense`  默认的网卡名称为 `em0`  和 `em1` ， `WAN 口` 地址与 `LAN 口` 地址可以先分别配置为 `em0`  与 `em1`  ， `LAN 口` 的  `IP`  需能与主机正常通信（主机能 ping 通）。
配置完成后，大概是这个样子：
![pfsense_wan_lan_config](http://cdn.talkaboutos.top/pfsense_wan_lan_config.png)
图：WAN 口和 LAN 口配置


可以分别看到 `WAN 口` 与 `LAN 口` 的 IP 地址 。
此时，输入 `1`  ，即查看网卡信息，即可看到 `WAN 口` 、 `LAN 口` 的 `MAC` ` 地址` ：
![pfsense_show_mac](http://cdn.talkaboutos.top/pfsense_show_mac.png)
图：pfSense 查看网卡 MAC 地址


可以看到：
`em0` （LAN 口）的 MAC 为： `00:0C:29:E4:9D:8F` 
`em1`  （WAN 口）的 MAC 为： `00:0C:29:E4:9D:99` 
确保 `LAN 口` 的 MAC 与我们前面配置的**桥接网卡**的地址一致即可（只有这样，我们才能通过物理机 Web 访问到 `pfSense`  防火墙）。


接下来，见证奇迹的时刻到了：
在物理机浏览器输入 http://LAN 口 IP ，即可登录 `pfSense`  防火墙 Web 端了。
默认用户名密码为： `admin/pfsense`  .
![pfsense_dashboard](http://cdn.talkaboutos.top/pfsense_dashboard.png)
图：pfSense 首页


可以畅快地玩耍了！


oh，no，no
忘了一个东西，记得把 Web 界面语言设置为 “Chinese （Simplified）”!
![pfsenese_set_language-01](http://cdn.talkaboutos.top/pfsenese_set_language-01.png)
![pfsenese_set_language-02](http://cdn.talkaboutos.top/pfsenese_set_language-02.png)
图：pfSense 设置中文语言


OK ！


# Reference：

- 1.[https://blog.51cto.com/fxn2025/2447266](https://blog.51cto.com/fxn2025/2447266)
- 2.[https://www.freebuf.com/sectool/214820.html](https://www.freebuf.com/sectool/214820.html)
