###### tags: `Writings` 

---

# 1 背景
2020年10月，在公众号博主的种草下，花了 400大洋，入手了蜗牛星际A 款 NAS（四盘位） ，即大伙儿口中的**“黑群晖”**。

当时存储需求不大，只配置了 2块 东芝 3TB P300 非叠瓦硬盘。按 RAID 0 配置，未做数据冗余保护，可用空间约为 5.23TB 。

这两年多来一直比较稳定，直到随着备份数据的增长，磁盘容量日趋紧张，直至飘红。

于是不得不，下血本买了块新盘，准备扩上去。

下单后，硬盘倒是很快就到了。

原本以为，扩容也只需把硬盘往硬盘托架一装，就可以完成。

实际下来，我才发现，是远远高估了这个“黑群晖”的易用性和可扩展性。

且看下文实际过程。

# 2 电源供电不足
新硬盘安装好，上电开机，登录控制台后，没多久就报 Error：硬盘 crash 掉了。
![image.png](https://cdn.nlark.com/yuque/0/2022/png/148899/1657983827950-baaa74a7-27ec-4f3c-a4c9-6f36cfa939d2.png#clientId=u48c2262d-207e-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=60&id=u6d0c4c2a&margin=%5Bobject%20Object%5D&name=image.png&originHeight=93&originWidth=1794&originalType=binary&ratio=1&rotation=0&showTitle=false&size=15571&status=done&style=stroke&taskId=ud5d6639c-2e1e-40b4-b497-8ec6b11aef3&title=&width=1148.16)
之前使用过程中，可是从来没有出现过硬盘 crash 的情况啊。

于是试着重启一下看看。

重启后，依然出现这种情况。

又想起，网上许多关于黑群晖的帖子，都是在讲，机器到手后，更换电源、风扇、固态的。因为原装的这些配件，质量是实在太差了。

怀疑是，原装的电源，供电能力不足，支撑不了 3块硬盘。

于是乎，在网友的推荐下，又下单了 益衡 ENP-7025B 250W 电源。
![益衡ENP-7025B-250W电源.png](https://cdn.nlark.com/yuque/0/2022/png/148899/1657984035624-178a0399-0e7d-48bb-b659-50095f8ea460.png#clientId=u48c2262d-207e-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=618&id=u75717189&margin=%5Bobject%20Object%5D&name=%E7%9B%8A%E8%A1%A1ENP-7025B-250W%E7%94%B5%E6%BA%90.png&originHeight=965&originWidth=1293&originalType=binary&ratio=1&rotation=0&showTitle=false&size=1663924&status=done&style=stroke&taskId=u661a84cc-410f-4489-8064-54e2d6e5e04&title=&width=827.52)

虽然有点小贵，但是相比于那几个 TB 的数据来说，也还说的过去。

收到后，周末花了半天时间，换上去后检查，机器顺利点亮，硬盘不再报 crash 错误。

顺利解决供电不足问题。


# 3 黑群晖系统 bug

搞定供电不足的问题后，黑群晖的系统 bug ，把我给挡住了。

我本机原来的的配置是：2块硬盘 RAID 0 配置，构成一个存储空间，通过群晖的 `SharedFolder`功能，共享给 Windows 电脑，Windows 上通过 “映射网络驱动器”挂载到文件系统上。
![image.png](https://cdn.nlark.com/yuque/0/2022/png/148899/1657966696194-7955d9b2-094c-4df6-bd06-1758e22205f8.png#clientId=u9d98f546-90c8-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=280&id=ua667bfc2&margin=%5Bobject%20Object%5D&name=image.png&originHeight=385&originWidth=1243&originalType=binary&ratio=1&rotation=0&showTitle=false&size=46356&status=done&style=stroke&taskId=u09cfd6de-d42f-4680-8824-705e302059b&title=&width=904)
我需要将这块新盘，扩到这个 SharedFolder 上，就是这个 `volume 2`，即 存储空间 2 上。

不过，当我打开 DSM 存储空间管理面板，顿时傻眼了。
![image.png](https://cdn.nlark.com/yuque/0/2022/png/148899/1657966900367-58ca4f86-cb80-461b-8a3b-fef5d40c72b0.png#clientId=u9d98f546-90c8-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=543&id=uf40e66be&margin=%5Bobject%20Object%5D&name=image.png&originHeight=746&originWidth=1253&originalType=binary&ratio=1&rotation=0&showTitle=false&size=88171&status=done&style=stroke&taskId=ufb2a24dc-6dd2-4af7-970f-3b6bc54f6c5&title=&width=911.2727272727273)

按照上面的提示，我这个 `Volume 2` 的 Action 按钮，压根就没有添加硬盘的选项。

那能不能通过加到 `Volume 2`  对应的存储池，来实现呢？
![StoragePool-AddDrive-Snipaste_2022-07-16_13-43-46.png](https://cdn.nlark.com/yuque/0/2022/png/148899/1657967236197-ff0c64c6-312c-45fc-aa94-45108e343167.png#clientId=u9d98f546-90c8-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=540&id=u03e248e6&margin=%5Bobject%20Object%5D&name=StoragePool-AddDrive-Snipaste_2022-07-16_13-43-46.png&originHeight=743&originWidth=1255&originalType=binary&ratio=1&rotation=0&showTitle=false&size=84262&status=done&style=stroke&taskId=u69f644e1-312e-4644-b049-0a324b5c484&title=&width=912.7272727272727)

这回，这里倒是有“Add Drive”按钮了，可是，是灰的，不起作用。

简直是山重水复又无路啊……

# 4 bug 解决思路

上面的黑群晖系统无法扩容添加磁盘，疑似黑群晖系统做了功能阉割，限制磁盘数量。

我看了下当前系统的版本，是 DSM 6.2.3 ，已经有更新的版本发行。于是，想着能否通过重装新的系统，看看这个问题还存不存在。

可是，网上一搜，关于黑群晖升级的教程、帖子，操作步骤都极其繁琐。

且不说能否升级成功吧，就是升级成功了，这个 bug 也不一定就能解决。

还有个重要因素是，我手上只有一台笔记本，重装系统需要显示器、外置键盘，我这都没有。缺了这些，也没法重装。

于是乎，这条路也行不通。

# 5 命令行手动扩容

在研究如何扩容的时候，发现 DSM 系统支持 ssh 命令行。

本质上来说，这个黑群晖，就是一台跑着专用 Linux 系统的服务器嘛，支持 ssh 太正常不过了。

## 5.1 获取 root 权限

首先，是需要获取系统的 root 权限。

先以现有的 admin 账户 ssh 登录进去，参考[网上的教程](https://www.vediotalk.com/archives/2211)，通过几条命令顺利获取到了 root 账号和权限：

```shell
（1）默认账户登录，切换至 root 权限
sudo -i

（2）修改 sshd_config 文件夹权限
chmod 755 sshd_config

（3）修改config配置文件内容允许 root 账户登录，重启
vi /etc/ssh/sshd_config

PermitRootLogin yes

（4）再次以默认账户登录，并切换至 root 权限
ssh admin@10.10.10.172

sudo -i

（5）修改 root 默认密码（xxx改为你要设置的密码）
synouser --setpw root xxx

完成后，即可使用 root 账户 ssh 登录了。

```



## 5.2 群晖 NAS 存储系统层次结构
在进行实际操作前，我们先来了解一下群晖 NAS 存储里面的几个概念和他们之间的关系。

先放上一张图：
![群晖Synology硬盘、存储空间和存储池之间的关系.png](https://cdn.nlark.com/yuque/0/2022/png/148899/1657973544685-780cd25c-7d03-4dbb-bee6-0f6d90895159.png#clientId=ufedf6bb2-669f-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=304&id=u65a3bc23&margin=%5Bobject%20Object%5D&name=%E7%BE%A4%E6%99%96Synology%E7%A1%AC%E7%9B%98%E3%80%81%E5%AD%98%E5%82%A8%E7%A9%BA%E9%97%B4%E5%92%8C%E5%AD%98%E5%82%A8%E6%B1%A0%E4%B9%8B%E9%97%B4%E7%9A%84%E5%85%B3%E7%B3%BB.png&originHeight=418&originWidth=975&originalType=binary&ratio=1&rotation=0&showTitle=true&size=50358&status=done&style=stroke&taskId=u15baf713-6a01-4833-a149-0f74f1179f2&title=%E7%BE%A4%E6%99%96Synology%E7%A1%AC%E7%9B%98%E3%80%81%E5%AD%98%E5%82%A8%E7%A9%BA%E9%97%B4%E5%92%8C%E5%AD%98%E5%82%A8%E6%B1%A0%E4%B9%8B%E9%97%B4%E7%9A%84%E5%85%B3%E7%B3%BB&width=709.0909090909091 "群晖Synology硬盘、存储空间和存储池之间的关系")
图片来源：[https://www.suncan.com.cn/archives/5909](https://www.suncan.com.cn/archives/5909)

群晖 NAS 存储中，有这么几个概念：硬盘、存储空间、存储池和共享文件夹（SharedFolder ）。

**多个硬盘可以聚合成一个存储池，通过存储池可以划分不同的存储空间，而共享文件夹可以设置不同的存储空间存储。**

详细来说，就是：

- 硬盘：存储数据的物理设备，比如机械硬盘、固态硬盘、USB 闪存盘等。
- 存储池：可初略对应上图的中 LVM 层。即将多个物理存储空间，聚合成一个大的存储空间。在存储池上，可以创建多个不同的存储空间。
- 存储空间：对应上图中的 Volume ，即用户可根据需要，在存储池中划出一定容量大小的空间，作为某个特定用途或特定用户使用的存储空间。
- 共享文件夹：对应上图中的 SharedFolder ，即用户将存储空间共享出去，网络上的其他用户可以共享其中的文件资源。


## 5.3 LVM（动态逻辑卷管理器）
上述中的 LVM （Logical Volume Manager）动态逻辑卷管理器，是 Linux 环境下对磁盘分区进行管理的一种机制。

可以实现对磁盘大小进行弹性调整，而且不会丢失数据。

想象一下，当你在规划主机的时候，只给 `/var` 分区划了 200GB 空间，等系统运行一段时间后，应用产生大量的数据，空间不够用了。

按照传统的做法，只能重新加一块硬盘，重新进行分区格式化，将 `/var `上的数据拷贝过来，然后将原来分区卸载，重新挂载新的分区。好不简单。

如果是第二次划分的容量太大了，导致很大磁盘容量浪费，想要把这个分区缩小时，是不是又得按照上面流程再搞一遍？好不麻烦。

LVM 可以聚合多个实体分区，抽象封装之后，对外看起来就像一个磁盘一样。而且，可以在将来新增或移除其他的实体分区到这个 LVM 管理的磁盘中。

这样的话，整个磁盘空间的使用，就具有相当的灵活性了！

关于 LVM 详细的内容，可以参考《鸟哥的 Linux 私房菜基础篇》，在线学习地址：[https://linux.vbird.org/linux_basic/](https://linux.vbird.org/linux_basic/) 。


## 5.4 命令手动扩容操作
了解了上述的群晖存储系统层次结构和 LVM 原理，现在可以开始上手操作了。

**提别提醒：**

- **数据是无价的！**
- **尽可能备份涉及到的存储空间内的数据 。**


### 5.4.1 存储系统基本情况
先来看一下群晖系统现有情况。

在存储管理面板，可以看到， `Volume 2` 是原有的 2 块磁盘构成的存储空间，`Volume 3` 是新增磁盘构成的存储空间。
![current_volume.png](https://cdn.nlark.com/yuque/0/2022/png/148899/1657976626735-8952723b-1d62-4661-aa74-253452118e72.png#clientId=ufedf6bb2-669f-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=248&id=u34f594c1&margin=%5Bobject%20Object%5D&name=current_volume.png&originHeight=341&originWidth=1255&originalType=binary&ratio=1&rotation=0&showTitle=false&size=46801&status=done&style=stroke&taskId=uaf121e31-61da-4b7e-8bed-ada6de76e66&title=&width=912.7272727272727)


ssh 远程界面，我们先看下系统存储空间使用情况：
```shell
# df -h
Filesystem         Size  Used Avail Use% Mounted on
/dev/md0           2.3G 1016M  1.2G  46% /
none               1.9G     0  1.9G   0% /dev
/tmp               1.9G  876K  1.9G   1% /tmp
/run               1.9G  3.4M  1.9G   1% /run
/dev/shm           1.9G  4.0K  1.9G   1% /dev/shm
none               4.0K     0  4.0K   0% /sys/fs/cgroup
cgmfs              100K     0  100K   0% /run/cgmanager/fs
/dev/vg2/volume_3  2.7T   18M  2.7T   1% /volume3
/dev/md2           2.4G   50M  2.3G   3% /volume1
/dev/vg1/volume_2  5.3T  4.8T  524G  91% /volume2
```
可以看得，`/dev/vg1/volume_2` 是原有的 2 块盘形成的空间，`/dev/vg2/volume_3` 是新插入的磁盘形成的空间。

我们这次扩容的目的，就是要将 `vg2/volume_3` 扩到 `vg1/volume_2` 。

再来看一下设备挂载情况。
```shell
# fdisk -l 

Disk /dev/ram0: 640 MiB, 671088640 bytes, 1310720 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 4096 bytes
I/O size (minimum/optimal): 4096 bytes / 4096 bytes
……


Disk /dev/sdb: 15 GiB, 16099835904 bytes, 31444992 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: gpt
Disk identifier: 6E06AAE0-541B-417C-AA76-020C5DC9B4E6

Device       Start      End Sectors  Size Type
/dev/sdb1     2048  4982527 4980480  2.4G Linux RAID
/dev/sdb2  4982528  9176831 4194304    2G Linux RAID
/dev/sdb3  9177088  9308159  131072   64M EFI System
/dev/sdb4  9308160  9437183  129024   63M Microsoft basic data
/dev/sdb5  9437184 14680030 5242847  2.5G Linux RAID


Disk /dev/sdc: 2.7 TiB, 3000592982016 bytes, 5860533168 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 4096 bytes
I/O size (minimum/optimal): 4096 bytes / 4096 bytes
Disklabel type: gpt
Disk identifier: F09693A0-1533-4C6F-88AE-E98E3C48ACF5

Device       Start        End    Sectors  Size Type
/dev/sdc1     2048    4982527    4980480  2.4G Linux RAID
/dev/sdc2  4982528    9176831    4194304    2G Linux RAID
/dev/sdc3  9437184 5860328351 5850891168  2.7T Linux RAID


Disk /dev/sde: 2.7 TiB, 3000592982016 bytes, 5860533168 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 4096 bytes
I/O size (minimum/optimal): 4096 bytes / 4096 bytes
Disklabel type: gpt
Disk identifier: 3187222F-2933-45F8-928B-70D5A5359F35

Device       Start        End    Sectors  Size Type
/dev/sde1     2048    4982527    4980480  2.4G Linux RAID
/dev/sde2  4982528    9176831    4194304    2G Linux RAID
/dev/sde3  9437184 5860328351 5850891168  2.7T Linux RAID


Disk /dev/sdf: 2.7 TiB, 3000592982016 bytes, 5860533168 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 4096 bytes
I/O size (minimum/optimal): 4096 bytes / 4096 bytes
Disklabel type: gpt
Disk identifier: 3367D4CE-B624-4DA1-9EA6-BD3273644B2C

Device       Start        End    Sectors  Size Type
/dev/sdf1     2048    4982527    4980480  2.4G Linux RAID
/dev/sdf2  4982528    9176831    4194304    2G Linux RAID
/dev/sdf3  9437184 5860328351 5850891168  2.7T Linux RAID


……


Disk /dev/md4: 2.7 TiB, 2995655213056 bytes, 5850889088 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 4096 bytes
I/O size (minimum/optimal): 4096 bytes / 4096 bytes


Disk /dev/md3: 5.5 TiB, 5991310426112 bytes, 11701778176 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 4096 bytes
I/O size (minimum/optimal): 65536 bytes / 131072 bytes


Disk /dev/mapper/vg2-syno_vg_reserved_area: 12 MiB, 12582912 bytes, 24576 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 4096 bytes
I/O size (minimum/optimal): 4096 bytes / 4096 bytes


Disk /dev/mapper/vg2-volume_3: 2.7 TiB, 2995639025664 bytes, 5850857472 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 4096 bytes
I/O size (minimum/optimal): 4096 bytes / 4096 bytes


Disk /dev/mapper/vg1-syno_vg_reserved_area: 12 MiB, 12582912 bytes, 24576 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 4096 bytes
I/O size (minimum/optimal): 65536 bytes / 131072 bytes


Disk /dev/mapper/vg1-volume_2: 5.5 TiB, 5991294828544 bytes, 11701747712 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 4096 bytes
I/O size (minimum/optimal): 65536 bytes / 131072 bytes

```

从上面可以看到，15 GB 的 `sdb` 是系统盘，上面划分出了系统分区和其他数据分区。

`sdc`、 `sde`、 `sdf`，是包括我们新盘在内的 3块磁盘。

`/dev/md3: 5.5 TiB` 和 `/dev/md4: 2.7 TiB`分别是原有的 2 块盘、新盘 RAID 后形成的设备。

`/dev/mapper/vg1-volume_2: 5.5 TiB`, `/dev/mapper/vg2-volume_3: 2.7 TiB` 分别对应我们在系统管理后台 存储资源管理面板看到的  `Volume 2` 和  `Volume 3 `。


### 5.4.3 LVM 信息

-  PV（Physical Volume，物理卷 ）信息：

![image.png](https://cdn.nlark.com/yuque/0/2022/png/148899/1657978779955-f6cf9ab2-4e43-46f5-90ed-fc2d1b515098.png#clientId=u1ef8c664-9a88-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=439&id=u85c4bef4&margin=%5Bobject%20Object%5D&name=image.png&originHeight=604&originWidth=1232&originalType=binary&ratio=1&rotation=0&showTitle=false&size=54129&status=done&style=stroke&taskId=u3869ac63-16e2-42ae-9658-18c00a1e56c&title=&width=896)
有 2 个 PV，对应我们新盘、原有磁盘的 RAID 。

- VG （Volume Group，卷组）信息：

![image.png](https://cdn.nlark.com/yuque/0/2022/png/148899/1657978850019-b9497e98-aa5b-43fd-a288-a57e800db3b8.png#clientId=u1ef8c664-9a88-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=675&id=u373ad18e&margin=%5Bobject%20Object%5D&name=image.png&originHeight=928&originWidth=1611&originalType=binary&ratio=1&rotation=0&showTitle=false&size=73100&status=done&style=stroke&taskId=ua6dacec3-8bd1-4686-a05f-91ec6ba65d5&title=&width=1171.6363636363637)
2 个 VG ，对应目前的 `Volume 3` 和 `Volume 2`  。

- LV （Logical Volume，逻辑卷）信息：

![image.png](https://cdn.nlark.com/yuque/0/2022/png/148899/1657978926730-40971041-fefb-4fe4-a092-0d2393bc7bd8.png#clientId=u1ef8c664-9a88-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=580&id=u84b9e1e8&margin=%5Bobject%20Object%5D&name=image.png&originHeight=798&originWidth=1421&originalType=binary&ratio=1&rotation=0&showTitle=false&size=70999&status=done&style=stroke&taskId=u115ca5b8-8a89-49ee-ae9a-ee0a497393b&title=&width=1033.4545454545455)

![image.png](https://cdn.nlark.com/yuque/0/2022/png/148899/1657978939181-a2a07fa2-678b-4970-8c23-b6f3725a8234.png#clientId=u1ef8c664-9a88-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=256&id=uabac7f3f&margin=%5Bobject%20Object%5D&name=image.png&originHeight=352&originWidth=1366&originalType=binary&ratio=1&rotation=0&showTitle=false&size=29303&status=done&style=stroke&taskId=u54911b7a-85dc-4ee5-8924-fd4ed06b309&title=&width=993.4545454545455)
4 个 LV ，每个 VG 底下除了存储数据的数据卷，还有一个 12MB 大小的 VG 保留空间（上面的图片为扩容操作后截的图，少了 `/dev/vg2/volume_3` 的信息 ）。

### 5.4.4 扩容操作

根据 LVM 原理，结合我们现在系统的情况，我们需要将属于 `vg2 `的 `volume_3 `，增加到` vg1 `中。

（1）删除 `vg2` ，把` volume_3` 释放出来
```shell
vgremove vg2
```
![image.png](https://cdn.nlark.com/yuque/0/2022/png/148899/1657979989284-63668c8e-1084-42f7-aa5f-46cb00e2a3c0.png#clientId=u1ef8c664-9a88-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=39&id=ued88aacb&margin=%5Bobject%20Object%5D&name=image.png&originHeight=61&originWidth=1041&originalType=binary&ratio=1&rotation=0&showTitle=false&size=8491&status=done&style=stroke&taskId=u46fa9984-ca79-4f20-8da6-c71adb0621a&title=&width=666.24)
我这里` vg2 `只有 `volume_3` 一个逻辑卷，所以直接执行的 `vgremove ` 。

如果存在多个逻辑卷，需根据实际情况确定。

删除之后，看下 VG 情况：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/148899/1657980078182-f47e4264-cd40-4010-8fc9-dde829345625.png#clientId=u1ef8c664-9a88-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=370&id=u1341e472&margin=%5Bobject%20Object%5D&name=image.png&originHeight=578&originWidth=1034&originalType=binary&ratio=1&rotation=0&showTitle=false&size=49994&status=done&style=stroke&taskId=uba306453-06de-4846-909e-e89abc3799e&title=&width=661.76)

（2）扩容 `vg1 `
```shell
vgextend vg1 /dev/md4
```
![Snipaste_2022-07-16_15-58-14-vgextend-md4-separate.png](https://cdn.nlark.com/yuque/0/2022/png/148899/1657983124460-40fdefd8-9156-49a3-a8a2-c2cb955af76d.png#clientId=u48c2262d-207e-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=42&id=u2a5f12ef&margin=%5Bobject%20Object%5D&name=Snipaste_2022-07-16_15-58-14-vgextend-md4-separate.png&originHeight=66&originWidth=914&originalType=binary&ratio=1&rotation=0&showTitle=false&size=10045&status=done&style=stroke&taskId=u569bbb1f-ba0c-4c05-996a-9ef3cf9260b&title=&width=584.96)
执行完 `vgextend`后，查看下 VG 情况，可以看到 `vg1 `大小，已经扩成 8.17TB 了。
![Snipaste_2022-07-16_15-58-14-vgdisplay(after-vgextend-md4).png](https://cdn.nlark.com/yuque/0/2022/png/148899/1657983219941-13a0b4d5-d3e6-427f-accb-ef0dff72d67b.png#clientId=u48c2262d-207e-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=351&id=u50d39164&margin=%5Bobject%20Object%5D&name=Snipaste_2022-07-16_15-58-14-vgdisplay%28after-vgextend-md4%29.png&originHeight=549&originWidth=1089&originalType=binary&ratio=1&rotation=0&showTitle=false&size=43685&status=done&style=stroke&taskId=u089c15cc-8abd-496c-a122-a769034078d&title=&width=696.96)
（3）扩容 LV

```shell
lvextend -l +714219 /dev/vg1/volume_2
```
![image.png](https://cdn.nlark.com/yuque/0/2022/png/148899/1657980479952-9be167e8-0b72-44ba-b72b-a603bc6eda2b.png#clientId=u1ef8c664-9a88-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=49&id=ueb3ae100&margin=%5Bobject%20Object%5D&name=image.png&originHeight=77&originWidth=1239&originalType=binary&ratio=1&rotation=0&showTitle=false&size=13633&status=done&style=stroke&taskId=u44d182b2-cacb-4d71-b4c6-bf3396f7d8f&title=&width=792.96)
`lvextend` 命令中的 `+714219`参数，来自` vg1 `扩容完成后，`vgdisplay` 中输出的 Free PE 。

即我们在 `lvextend `中，要把这些 free 的 PE（Physical Extend，物理扩展区块） 添加进来。

执行完 `lvextend `，再查看下 LV ：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/148899/1657980633038-167723c6-b27f-4b04-8faa-7480ef0e1531.png#clientId=u1ef8c664-9a88-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=532&id=u7bad13b4&margin=%5Bobject%20Object%5D&name=image.png&originHeight=832&originWidth=1606&originalType=binary&ratio=1&rotation=0&showTitle=false&size=80437&status=done&style=stroke&taskId=u71b3166e-f3b6-4bd9-9d2a-88dabf965f1&title=&width=1027.84)
`/dev/vg1/volume_2` 大小已经扩成 8.17TB。

在文件系统查看一下：
![Snipaste_2022-07-16_16-08-42-df_h(after extend)-edited.png](https://cdn.nlark.com/yuque/0/2022/png/148899/1657980788649-cb4cfb4c-f39b-45d2-b67e-de46d14a230b.png#clientId=u1ef8c664-9a88-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=476&id=u5f591544&margin=%5Bobject%20Object%5D&name=Snipaste_2022-07-16_16-08-42-df_h%28after%20extend%29-edited.png&originHeight=744&originWidth=1031&originalType=binary&ratio=1&rotation=0&showTitle=false&size=66793&status=done&style=stroke&taskId=u8247495e-2d0e-4c50-89c4-e59af10cf9a&title=&width=659.84)
可以看到，当前文件系统中，依然还是 5.3T 。

这是为什么呢？

那是因为，我们还没在文件系统中，同步上述 LVM 中的修改。

（4）将 LV 容量扩容到整个文件系统
```shell
 resize2fs  /dev/vg1/volume_2
```
报错：
```shell
# resize2fs  /dev/vg1/volume_2  
resize2fs 1.39 (29-May-2006)  
resize2fs: Device or resource busy while trying to open  /dev/vg1/volume_2
Couldn't find valid filesystem superblock.
```
网上搜了一下报错，应该是 `resize2fs` 这个不支持 NAS 上的 btrfs 文件系统。

解决方案是：
```shell
btrfs filesystem resize max /dev/vg1/volume_2
```

![Snipaste_2022-07-16_16-27-39-btrfs-separate.png](https://cdn.nlark.com/yuque/0/2022/png/148899/1657981750004-1ed36a75-2642-4ac6-be79-961e853af495.png#clientId=u1ef8c664-9a88-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=44&id=ubc3eba8b&margin=%5Bobject%20Object%5D&name=Snipaste_2022-07-16_16-27-39-btrfs-separate.png&originHeight=68&originWidth=1056&originalType=binary&ratio=1&rotation=0&showTitle=false&size=10800&status=done&style=stroke&taskId=u2ae5eca8-147a-4851-9b48-d9e9ced2ca0&title=&width=675.84)
执行完成，在看下文件系统：
![Snipaste_2022-07-16_16-27-39-df_h(after-btrfs).png](https://cdn.nlark.com/yuque/0/2022/png/148899/1657981778699-fa1662f2-c9a6-4bb1-b033-1ddbdc5bdceb.png#clientId=u1ef8c664-9a88-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=225&id=u0fdb7e92&margin=%5Bobject%20Object%5D&name=Snipaste_2022-07-16_16-27-39-df_h%28after-btrfs%29.png&originHeight=352&originWidth=1114&originalType=binary&ratio=1&rotation=0&showTitle=false&size=46441&status=done&style=stroke&taskId=u6bdb7363-af1d-4910-ab6e-02e2a188172&title=&width=712.96)
已经顺利扩上去了。

至此，基本上完成。 

再去群晖管理后台，存储管理面板看下，
![Snipaste_2022-07-16_16-28-29-storage-manager(after extend).png](https://cdn.nlark.com/yuque/0/2022/png/148899/1657981946211-1eae0400-d487-4bd3-bcfc-3c290cade9ad.png#clientId=u1ef8c664-9a88-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=456&id=uca3b39a9&margin=%5Bobject%20Object%5D&name=Snipaste_2022-07-16_16-28-29-storage-manager%28after%20extend%29.png&originHeight=713&originWidth=910&originalType=binary&ratio=1&rotation=0&showTitle=false&size=49046&status=done&style=stroke&taskId=u1a096f74-93ff-4c47-b421-166d002adbb&title=&width=582.4)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/148899/1657981958242-5da14af0-d057-4192-b1ef-878f22bf22f5.png#clientId=u1ef8c664-9a88-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=477&id=ud62fa2db&margin=%5Bobject%20Object%5D&name=image.png&originHeight=746&originWidth=1248&originalType=binary&ratio=1&rotation=0&showTitle=false&size=59205&status=done&style=stroke&taskId=ue6aacebe-8d73-4162-b1b0-98c2cfbf14f&title=&width=798.72)

可以看到，已经完美生效了！

可以试着重启一下 NAS ，看下重启之后是否依然有效。

重新启动后，存储空间依然稳定不变，证明这次手动扩容有效！


# 6 后记

在学校的时候，机缘巧合曾经学过一段时间的强电。

当时是学校组织的课外时间活动，安排学校负责电力系统保障的师傅，给我们讲强电的一些基础知识，还带我们去学校各个开闭所，讲解里面的电力设施及线路布设情况等。

电力的老师傅曾经说个这么一句话：大家都知道强电很危险，这就要求我们电工必须，胆大心细！

我觉得，这句话用在我们对数据进行操作，也是适用的。

虽然我这只有区区几个 TB 的数据，但是一旦损坏，也会给个人生活、工作带来一定的不便。

所以还是要适当地做好数据备份工作。


# Reference

- [《鸟哥的 Linux 私房菜基础篇》](https://linux.vbird.org/linux_basic/)
- [群晖Synology硬盘、存储空间和存储池之间的关系](https://www.suncan.com.cn/archives/5909)
- [Btrfs ](https://btrfs.wiki.kernel.org/index.php/Main%5FPage)
- [resize2fs: Device or resource busy while trying to open /dev/mmcblk0p3](https://raspberrypi.stackexchange.com/questions/82585/resize2fs-device-or-resource-busy-while-trying-to-open-dev-mmcblk0p3)
- [修改群晖存储池及存储空间顺序](https://blog.kuretru.com/posts/2752cb63/) 
- [群晖 SHR 扩容记录暨群晖分区、SHR机制大解密](https://post.smzdm.com/p/az5005qo/) 
- [群晖 | 6.2最新获取root权限设置root密码方法 ](https://www.vediotalk.com/archives/2211)


